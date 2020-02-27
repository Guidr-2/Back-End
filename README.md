# Guidr Back-End

| Method | Description           | Endpoint         |
| ------ | --------------------- | ---------------- |
| POST   | Create a user account | '/auth/register' |
| POST   | Login a user          | '/auth/login'    |

#### Base URL: https://guidr-2.herokuapp.com/api/

#### Seeded users
```
"username": "nathansl2003",
 "password": "password"

"username": "jsmith",
"password": "jsmith"

"username": "klock",
"password": "klock"
```

## Register (Non-protected)
**HTTP Method:** *POST*

**URL:** */auth/register*

This registers a new user, it will return the 201 message below with a token, this token needs to be stored in Local Storage.  The token contains the following data
```
{
  "userid": 2,
  "username": "jsmith",
  "iat": 1582819234,
  "exp": 1582822834
}
```

In order to decode the token, you need to install jwt-decode https://github.com/auth0/jwt-decode and install
```
npm i jwt-decode
```

### Register Table

| Key      | Type   | Required |
| -------- | ------ | -------- |
| username | string | Yes      |
| password | string | Yes      |

### Example

```
{
   "username": "jsmith",
   "password": "jsmith"
}
```

### Responses
```
Code: 201 (Created)
{
   "message": "Registration successful jsmith!",
   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imtsb2NrIiwiaWF0IjoxNTgyODE1NzEyLCJleHAiOjE1ODI4MTkzMTJ9.YaduCwtuESqfPocXdzS2ggRZVxF9lQ5fB0lh7DpXQb8"
}

Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Login (Non-protected)
This logs in a user, it will return the 202 message below with a token, this token needs to be stored in Local Storage.  The token contains the following data

**HTTP Method:** *POST*

**URL:** */auth/login*
### Login Table

| Key      | Type   | Required |
| -------- | ------ | -------- |
| username | string | Yes      |
| password | string | Yes      |

### Example
```
{
   "username": "jsmith",
   "password": "jsmith"
}
```

### Responses
```
Code: 202 (Successful Login)
{
   "message": "Welcome jsmith!",
   "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Imtsb2NrIiwiaWF0IjoxNTgyODE1NzEyLCJleHAiOjE1ODI4MTkzMTJ9.YaduCwtuESqfPocXdzS2ggRZVxF9lQ5fB0lh7DpXQb8"
}

Code: 401 (Unauthorized)
{
   "message": "Invalid username or password"
}

Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

