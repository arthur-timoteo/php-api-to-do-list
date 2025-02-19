# PHP API TO-DO-LIST v.2.0

<code><img height="50" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/php/php.png"></code>
<code><img height="50" src="https://raw.githubusercontent.com/github/explore/80688e429a7d4ef2fca1e82350fe8e3517d3494d/topics/mysql/mysql.png"></code>

This API aims to present a brief to consume a API resources, mainly for students in the early years of Computer Science courses and the like. For this reason, it has few EndPoints (resources) to use, and can be expanded according to the need.

As it is an instructional project, **it is not recommended** that it be applied in a production environment, as safety routines and tests have not been implemented. These resources must be researched and implemented, following the current rules, in addition to good practices. Built in **PHP 7** (see below), it allows the beginner to understand the mechanisms of access to the resources of an API.

```html
PHP 7.4.3 (cli) (built: Jul  5 2021 15:13:35) ( NTS )
Copyright (c) The PHP Group Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies
```
 
## How to use this content?

This content has _free license for use_ (CC BY-SA 4.0).

If you want collaborate in this repository with any improvements you have made. To do this, just make a Fork and send Pull Requests.

## Composer

Changes should be updated via <code>composer dump-autoload -o</code> on your local machine.

## TODO

* ~~Fixed `token auth` in update mode~~
* ~~Fixed system login~~
* Implement Update `username` and `password`
* Implement EndPoint ``user/delete``

# Documentation

This API provides functionality for creating and maintaining users to control a simple To-Do-List application. The following shows the API structure for **users** and **tasks** resources.

## API Structure

```
+---api
    \task\
        ---delete
        ---edit
        ---new
        ---search
        ---update
    \user\
        ---new
        ---login
        ---update
+---src
    \---Database
    \---Helpers
    \---Task
    \---User
\---vendor
    \---composer
```

## _Database_

The development uses the MySQL 5, which can be changed at any time according to the need for use.
The database should be configured in <code>Database\Database.php</code>

### Scripts SQL

```sql
CREATE DATABASE <name>;
```

```sql
CREATE TABLE users (
    id          INT(3) 	    NOT NULL PRIMARY KEY AUTO_INCREMENT,
    name        VARCHAR(50) NOT NULL,
    email       VARCHAR(50) NOT NULL,
    username    VARCHAR(20) NOT NULL,
    password    VARCHAR(20) NOT NULL,
    token       VARCHAR(20) NOT NULL
);
```

```sql
CREATE TABLE tasks (
    id          INT(3) 	    NOT NULL PRIMARY KEY AUTO_INCREMENT,
    userId      INT(3)      NOT NULL,    
    name        VARCHAR(50) NOT NULL,
    date        date        NOT NULL,
    realized    INT(1)      NOT NULL
);
```

## Token

To use this API, a user must first be created with resource below.

A TOKEN will be returned that should be used in all subsequent requests for both user and task data manipulation.

# _Resources_ (User)
| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **NEW** | /api/**user**/**new**/ | **POST** |
---

```json
url = 'http://URI/api/user/new/';

payload = {
    "name": "name",
    "email": "email",
    "username": "username",
    "password": "password"
}

header = {
    "content-type": "application/json"
}
```

###### Success (User created)

```json
{
    "message": "User Successfully Added",
    "id": "user_id",
    "token": "TOKEN value"
}
```

###### Warnings

```json
{
  "message": "Invalid Arguments Number (Expected Four)"
}
```

```json
{
  "message": "Could Not Add User"
}
```

```json
{
  "message": "User Already Exists"
}
```

---

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **LOGIN** | /api/**user**/**login**/ | **POST** |

```json
url = 'http://URI/api/user/login/';

payload = {
    "username": "username",
    "password": "password"
}

header = {
    "content-type": "application/json"
}
```

###### Success

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@domain.com",
  "token": "YOUR_TOKEN"
}
```

###### Warnings

```json
{
  "message": "Invalid Arguments Number (Expected Two)"
}
```

```json
{
  "message": "Incorrect username and/or password"
}
```

---

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **UPDATE** | /api/**user**/**update**/ | **PUT** |

Attention: `username` and `password` can not be changed in this version.

```json
url = 'http://URI/api/user/update/';

payload = {
    "name": "name",
    "email": "email",
    "username": "username",
    "password": "password"
}

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success

```json
{
    "message": "User Successfully Updated'
}
```

###### Warnings

```json
{
  "message": "Invalid Arguments Number (Expected Four)"
}
```

```json
{
  "message": "Incorrect username and/or password"
}
```

```json
{
  "message": "Could Not Update User"
}
```

---

# _Resources_ (Task)
| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **NEW** | /api/**task**/**new**/ | **POST** |
---

```json
url = 'http://URI/api/task/new/';

payload = {
    "name": "Task name"
}

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success (Task created)

```json
{
    "message": "Task Successfully Added"
}
```

###### Warnings

```json
{
  "message": "Invalid Arguments Number (Expected Two)"
}
```

```json
{
  "message": "Could Not Add Task"
}
```

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **SEARCH** | /api/**task**/**search**/ | **POST** |
---

Payload is not necessary, as the control is performed by token.

**Realized** field accept values: "0" (open) and "1" (realized)

```json
url = 'http://URI/api/task/search/';

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success
```json
  [
      {
          "id": 1,
          "userId": 1,
          "name": "task name",
          "date": "2021-08-16",
          "realized": 0
      }
  ]
```

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **UPDATE** | /api/**task**/**update**/ | **PUT** |
---

```json
url = 'http://URI/api/task/update/';

payload = {
    "id": "value",
    "name": "Task name",
    "realized": "value"
}

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success
```json
{
    "message": "Task Successfully Updated"
}
```

###### Warnings

```json
{
  "message": "Task(s) not found"
}
```

```json
{
  "message": "Method Not Allowed"
}
```

```json
{
  "message": "Invalid Arguments Number (Expected Three)"
}
```

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **EDIT** | /api/**task**/**edit**/ | **POST** |
---

```json
url = 'http://URI/api/task/edit/';

payload = {
    "id": "value"
}

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success
```json
[
  {
      "id": 2,
      "userId": 1,
      "name": "Task name",
      "date": "2021-08-16",
      "realized": 0
  }
]
```

###### Warnings

```json
{
  "message": "Payload Precondition Failed"
}
```

```json
{
  "message": "Invalid or Missing Token"
}
```

```json
{
  "message": "Invalid Arguments Number (Expected One)"
}
```

```json
{
  "message": "Bad Request (Invalid Syntax)"
}
```

```json
{
  "message": "Token Refused"
}
```

| Resource |      URI      |  Method  |
|:--------:|:-------------:|:--------:|
|  **DELETE** | /api/**task**/**delete**/ | **DELETE** |
---

```json
url = 'http://URI/api/task/delete/';

payload = {
    "id": "id_task"
}

header = {
    "content-type": "application/json",
    "Authorization": "YOUR_TOKEN"
}
```

###### Success
```json
{
    "message": "Task deleted Successfuly"
}
```

```json
{
    "message": "Task not exist"
}
```

---
###### Other Warnings

```json
{
  "message": "Bad Request (Invalid Syntax)"
}
```

```json
{
  "message": "Token Refused"
}
```

```json
{
  "message": "Invalid or Missing Token"
}
```

```json
{
  "message": "Payload Precondition Failed"
}
```

```json
{
  "message": "Method Not Allowed"
}
```

```json
{
  "message": "<SQL Code>"
}
```

```json
{
  "message": "<Unknow>"
}
```

---

<a name="tryonline"></a>

## Try Online
- URI: [http://emsapi.esy.es/todolist](http://emsapi.esy.es/todolist/)

--- 

## How to cite this content
```
DE SOUZA, Edson Melo (2021, August 16). PHP API TO-DO-LIST v.2.0.
Available in: https://github.com/EdsonMSouza/php-api-to-do-list
```

Or BibTeX for LaTeX:

```latex
@misc{desouza2020phpapi,
  author = {DE SOUZA, Edson Melo},
  title = {PHP API TO-DO-LIST v.2.0},
  url = {https://github.com/EdsonMSouza/php-api-to-do-list},
  year = {2021},
  month = {August}
}
```

## License

[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa]

This work is licensed under a
[Creative Commons Attribution-ShareAlike 4.0 International License][cc-by-sa].

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa]

[cc-by-sa]: http://creativecommons.org/licenses/by-sa/4.0/
[cc-by-sa-image]: https://licensebuttons.net/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg