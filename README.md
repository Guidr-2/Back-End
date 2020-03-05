# Guidr Back-End

| Method | Description                           | Endpoint             |
| ------ | ------------------------------------- | -------------------- |
| POST   | Create a user account                 | /auth/register       |
| POST   | Login a user                          | /auth/login          |
| POST   | Adds a profile                        | /profiles            |
| POST   | Adds a trip                           | /trips               |
| GET    | All Users                             | /users               |
| GET    | Gets a specific user                  | /users/:id           |
| GET    | Gets a user, profile and trip data    | /users/data/:id      |
| GET    | Gets a users trips by user id         | /users/trips/:id     |
| GET    | Gets a users profile by user id       | /users/profiles/:id  |
| GET    | Gets all profiles                     | /profiles            |
| GET    | Gets a profile by a profile ID        | /profiles/:id        |
| GET    | Gets a public profile by a user ID    | /profiles/public/:id |
| GET    | Gets all trips                        | /trips               |
| GET    | Gets a trip by a trip ID              | /trips/:id           |
| PUT    | Edits/Updates a profile               | /profiles/:id        |
| PUT    | Edits/Updates a trip                  | /trips/:id           |
| DELETE | Deletes a user and all user content   | /users/:id           |
| DELETE | Deletes a trip                        | /trips/:id           |


#### Base URL: https://guidr-2.herokuapp.com/api/
#### Verify Server is Running: https://guidr-2.herokuapp.com/

#### Seeded users
```
"username": "nathansl2003",
 "password": "password"

"username": "jsmith",
"password": "jsmith"

"username": "klock",
"password": "klock"
```

**Update 3/5/2020**

One of the developers on my other team found a bug in the /api/users/:id that returns the user, profile and trip data.  There can only be 1 user and 1 profile, but many or no trips assigned to the user.  If there are many trips, it returns multiple objects with redundent information in multiple objects, so if there are 2 trips it will return the user and profile information twice, one for each trip.  **The bug get introduced when there is not a profile created or a trip created the /api/users/:id will return an empty dataset.** I can make it return data with NULLs but it might be hard for you get the data out of the objects.  
I am making the following changes:

New Route- /api/users/data/:id - this will return the data just like I mentioned above with NULLs

Modified Route - /api/users/id - this will just return the id and username of the user

**Suggested Action**

Instead of using the /api/users/data/:Id use the user's id and do the following:

/api/users/:id

/api/users/profiles/:id

/api/users/trips/:id

This is 3 calls that will get you everything on a specific user

## Table Layouts
### Users Table
| Key      | Type    | Required                |
| -------- | ------- | ----------------------- |
| id       | integer | Yes (server controlled) |
| username | string  | Yes                     |
| password | string  | Yes                     |

### Profiles Table
| Key              | Type            | Required                |
| ---------------- | --------------  | ----------------------- |
| id               | integer         | Yes (server controlled) |
| user_id          | integer         | Yes                     |
| title            | string          | Yes                     |
| tagline          | string          | Yes                     |
| guide_specialty  | string          | Yes                     |
| age              | integer         | Yes                     |
| years_experience | integer         | Yes                     |
| avatar_url       | text            | No                      |
| public_url       | text            | Yes                     |

### Trips Table
| Key              | Type            | Required                |
| ---------------- | --------------  | ----------------------- |
| id               | integer         | Yes (server controlled) |
| user_id          | integer         | Yes                     |
| title            | text            | Yes                     |
| description      | text            | Yes                     |
| is_private       | integer(0/1)    | Yes                     |
| is_professional  | integer(0/1)    | Yes                     |
| duration         | text            | Yes                     |
| distance         | text            | Yes                     |
| date             | text            | Yes                     |
| trip_type        | text            | Yes                     |


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
**HTTP Method:** *POST*

**URL:** */auth/login*

This logs in a user, it will return the 202 message below with a token, this token needs to be stored in Local Storage.  The token contains the following data

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

## Get All Users (Protected)
**HTTP Method:** *GET*

**URL:** */users/*

Gets all users in the database.

### Example

None

### Responses
```
Code: 200 (OK)
 {
     "id": 1,
     "username": "nathansl2003"
 }
    
Code: 401 (Unauthorized)
 {
    "message": "Unauthorized access"
 }
    
Code: 500 (Internal Server Error)
 {
    "message": "Internal Server Error, Error Returned: <error>"
 }
```

## Get a specific user (Protected)
**HTTP Method:** *GET*

**URL:** */users/:id*

Gets a specific user by id

### Example

None

### Responses
```
Code: 200 (OK)
 {
     "id": 1,
     "username": "nathansl2003"
 }
    
Code: 401 (Unauthorized)
 {
    "message": "Unauthorized access"
 }
    
Code: 500 (Internal Server Error)
 {
    "message": "Internal Server Error, Error Returned: <error>"
 }
```

## Get Users Data (Protected)
**HTTP Method:** *GET*

**URL:** */users/data/:id*

This gets a specific user and returns the user, profile and all trip data, each trip (1 user can have multiple trips) will have profile data and username attached it in case it is needed. It will return NULL on each key where data is not present.

### Example

None

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
        "public_url": "https://guidr1.herokuapp.com/api/profiles/public/2
        "trips_title": "3 Day Midwest Tour",
        "description": "Travel the midwest on paved roads through central Illinois",
        "is_private": 0,
        "is_professional": 1,
        "duration": "3 days",
        "distance": "90 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Bike Touring"
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Delete User (Protected)
**HTTP Method:** *DELETE*

**URL:** */users/:id*

This deletes the user and all associated data (profiles and trips).

### Example

None

### Responses
```
Code: 200 (OK)
{
    "removed": 1
}
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}

Code: 404 (Not found)
{
    "message": "Could not find user with given id"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get Public Profile (Non-protected)
**HTTP Method:** *GET*

**URL:** */profiles/public/:id*

Gets a profile of a specific user that is a non-protected router for guest accounts. It is the responsibility of the front-end to populate the web url (public_url) of the user's public profile

### Example

None

### Responses
```
Code: 200 (OK)
{
        "id": 3,
        "user_id": 3,
        "title": "Packrafting With the Best",
        "tagline": "Water is my home",
        "guide_specialty": "Packrafting",
        "age": 34,
        "years_experience": 15,
        "avatar_url": null,
        "public_url": "/public/3"
    }
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get all Profiles (Protected)
**HTTP Method:** *GET*

**URL:** */profiles*

This returns all profiles

### Example

None

### Responses
```
Code: 200 (OK)
    {
        "id": 1,
        "user_id": 1,
        "title": "Thru-hiking Expert",
        "tagline": "I am happiest in the wilderness",
        "guide_specialty": "Backpacking",
        "age": 43,
        "years_experience": 6,
        "avatar_url": null
        "public_url": "https://guidr1.herokuapp.com/api/profiles/public/1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get a Profile by Profile ID (Protected)
**HTTP Method:** *GET*

**URL:** */profiles/:id*

This returns a profile by profile ID

### Example

None

### Responses
```
Code: 200 (OK)
    {
        "id": 1,
        "user_id": 1,
        "title": "Thru-hiking Expert",
        "tagline": "I am happiest in the wilderness",
        "guide_specialty": "Backpacking",
        "age": 43,
        "years_experience": 6,
        "avatar_url": null
        "public_url": "https://guidr1.herokuapp.com/api/profiles/public/1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get a Specific Users Profile (Protected)
**HTTP Method:** *GET*

**URL:** */users/profiles/:id*

This returns a the user's profile by user id

### Example

None

### Responses
```
Code: 200 (OK)
    {
        "id": 1,
        "user_id": 1,
        "title": "Thru-hiking Expert",
        "tagline": "I am happiest in the wilderness",
        "guide_specialty": "Backpacking",
        "age": 43,
        "years_experience": 6,
        "avatar_url": null
       "public_url": "https://guidr1.herokuapp.com/api/profiles/public/1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Post New Profile (Protected)
**HTTP Method:** *POST*

**URL:** */profiles*

Adds a new profile to the database.  **Note: you cannot add a user_id that does not exist due to foriegn key constraints nor can you add a profile from with a user_id that already exists in the database, a user can only have 1 profile.**

### Example

```
{
     "user_id": 6,
     "title": "Thru-hiking Obsessionist",
     "tagline": "I am in love with the woods",
     "guide_specialty": "All things wilderness",
     "age": 48,
     "years_experience": 25,
     "avatar_url": null
     "public_url": "https://guidr1.herokuapp.com/api/profiles/public/6
}
 ```

### Responses
```
Code: 200 (OK)
{
   6
}
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get all Trips (Protected)
**HTTP Method:** *GET*

**URL:** */trips*

This returns all trips

### Example

None

### Responses
```
Code: 200 (OK)
{
        "id": 1,
        "title": "7 Long Trail Section Hike",
        "description": "Hike from the Appalachian approach trail where the Long Trail southern terminus is for a 7 day adventure",
        "is_private": 1,
        "is_professional": 0,
        "duration": "7 days",
        "distance": "70 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Backpacking",
        "user_id": 1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get a trip by trip ID (Protected)
**HTTP Method:** *GET*

**URL:** */trip/:id*

This returns a trip by trip ID

### Example

None

### Responses
```
Code: 200 (OK)
{
        "id": 1,
        "title": "7 Long Trail Section Hike",
        "description": "Hike from the Appalachian approach trail where the Long Trail southern terminus is for a 7 day adventure",
        "is_private": 1,
        "is_professional": 0,
        "duration": "7 days",
        "distance": "70 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Backpacking",
        "user_id": 1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Get a Specific Users Trips (Protected)
**HTTP Method:** *GET*

**URL:** */users/trips/:id*

This returns a the user's trips by user id

### Example

None

### Responses
```
Code: 200 (OK)
{
        "id": 1,
        "title": "7 Long Trail Section Hike",
        "description": "Hike from the Appalachian approach trail where the Long Trail southern terminus is for a 7 day adventure",
        "is_private": 1,
        "is_professional": 0,
        "duration": "7 days",
        "distance": "70 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Backpacking",
        "user_id": 1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Post New Trip (Protected)
**HTTP Method:** *POST*

**URL:** */trips*

Adds a new trip to the database.  **Note: you cannot add a user_id that does not exist due to foriegn key constraints.**

### Example

```
{
    "title": "Appalachain trail hike in Pennsylvania",
    "description": "We will pick a random spot on the AT in Pennsylvania and start hiking!",
    "is_private": 1,
    "is_professional": 0,
    "duration": "3 days",
    "distance": "30 miles",
    "date": "2020-06-25 08:00:00:000",
    "trip_type": "Backpacking",
    "user_id": 1
}
```

### Responses
```
Code: 200 (OK)
{
   6
}
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Edit/Update a Trip (Protected)
**HTTP Method:** *PUT*

**URL:** */trips/:id*

Edits/Update a trip

### Example

```
{
    "title": "Appalachain trail hike in Pennsylvania",
    "description": "We will pick a random spot on the AT in Pennsylvania and start hiking!",
    "is_private": 1,
    "is_professional": 0,
    "duration": "3 days",
    "distance": "30 miles",
    "date": "2020-06-25 08:00:00:000",
    "trip_type": "Backpacking",
    "user_id": 1
}
```

### Responses
```
Code: 200 (OK)
{
        "id": 1,
        "title": "7 Day Long Trail Section Hike",
        "description": "Hike from the Appalachian approach trail (3.2 miles) where the Long Trail southern terminus is for a 7 day adventure",
        "is_private": 1,
        "is_professional": 0,
        "duration": "7 days",
        "distance": "70 miles",
        "date": "2020-06-01 08:00:00:000",
        "trip_type": "Backpacking",
        "user_id": 1
    }
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}

Code: 404 (Not found)
{
    "message": "Could not find a trip with given id"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Deletes a Trip (Protected)
**HTTP Method:** *DELETE*

**URL:** */trips/:id*

Deletes a trip

### Example

None

### Responses
```
Code: 200 (OK)
{
    "removed": 1
}
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}

Code: 404 (Not found)
{
    "message": "Could not find a trip with given id"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```

## Edit/Update a Profile (Protected)
**HTTP Method:** *PUT*

**URL:** */profiles/:id*

Edits/Update a profile

### Example

```
{
    "id": 1,
    "user_id": 1,
    "title": "Thru-hiking Expert",
    "tagline": "I am happiest on the trails",
    "guide_specialty": "Backpacking",
    "age": 43,
    "years_experience": 6,
    "avatar_url": null
    "public_url": "https://guidr1.herokuapp.com/api/profiles/public/1
}
```

### Responses
```
Code: 200 (OK)
{
    "id": 1,
    "user_id": 1,
    "title": "Thru-hiking Expert",
    "tagline": "I am happiest on the trails",
    "guide_specialty": "Backpacking",
    "age": 43,
    "years_experience": 7,
    "avatar_url": null
}
    
Code: 401 (Unauthorized)
{
   "message": "Unauthorized access"
}

Code: 404 (Not found)
{
    "message": "Could not find profile with given id"
}
    
Code: 500 (Internal Server Error)
{
   "message": "Internal Server Error, Error Returned: <error>"
}
```
