# Guidr Back-End

| Method | Description                           | Endpoint         |
| ------ | ------------------------------------- | ---------------- |
| POST   | Create a user account                 | /auth/register   |
| POST   | Login a user                          | /auth/login      |
| GET    | Gets a user, proile and all trip data | /users/:id       |

#### Base URL: https://guidr1.herokuapp.com/api/

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

## Get User (Protected)
**HTTP Method:** *GET*

**URL:** */users/:id*

This gets a specific user and returns the user, profile and all trip data, each trip (1 user can have multiple trips) will have profile data and username attached it in case it is needed.

### Get User Table

| Key              | Type            | Required |
| ---------------- | --------------  | -------- |
| username         | string          | Yes      |
| profile_title    | string          | Yes      |
| tagline          | string          | Yes      |
| guide_specialty  | string          | Yes      |
| age              | integer         | Yes      |
| years_experience | integer         | Yes      |
| avatar_url       | text            | No       |
| trip_title       | text            | Yes      |
| description      | text            | Yes      |
| is_private       | integer(0/1)    | Yes      |
| is_professional  | integer(0/1)    | Yes      |
| duration         | text            | Yes      |
| distance         | text            | Yes      |
| date             | text            | Yes      |
| trip_type        | text            | Yes      |


### Responses
```
Code: 200 (OK)
    {
        "username": "jsmith",
        "profile_title": "Bike Touring the Scenic Routes",
        "tagline": "I love riding!",
        "guide_specialty": "Bike Touring",
        "age": 29,
        "years_experience": 10,
        "avatar_url": null,
        "trips_title": "3 Day Midwest Tour",
        "description": "Travel the midwest on paved roads through central Illinois",
        "is_private": 0,
        "is_professional": 1,
        "duration": "3 days",
        "distance": "90 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Bike Touring"
    }
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```
