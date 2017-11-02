<h1 align='center'> YUDU Sentinel API Documentation - Version 2.0</h1>

</br>
<p align="center">
  <img src="https://cloud.githubusercontent.com/assets/13904941/24360374/14e47384-12ff-11e7-8b3a-6134c5c50647.jpg?raw=true" alt="Sentinel"  />
</p></br>


## Contents

- [Introduction](#introduction)
- [Terminology](#terminology)
- [List of Endpoints](#list-of-endpoints)
    - [Unprotected](#unprotected)
    - [App](#app)
    - [Full Access](#full-access)
- [Request Headers](#request-headers)
-[Permissible fields](#permissible-fields)
    - [Users](#users)
    - [Contacts](#contacts)
    - [Groups](#groups)
    - [Broadcasts](#broadcasts)
- [HTTP Responses and errors](#http-responses-and-errors)
- [Examples](#examples)
    - [Obtaining an api token](#obtaining-an-api-token)
    - [Requesting the App directory](#requesting-the-app-directory)
    - [Creating a user](#creating-a-user)
    - [Creating a contact](#creating-a-contact)
    - [Creating a group](#creating-a-group)
    - [Assigning a user to a group](#assigning-a-user-to-a-group)
    - [Getting a groups users](#getting-a-groups-users)
    - [Assigning a contact to a group](#assigning-a-contact-to-a-group)
    - [Requesting a list of documents](#requesting-a-list-of-documents)
    - [Assigning a document to a user](#assigning-a-document-to-a-user)
    - [Getting a users' documents](#getting-a-users--documents)
    - [Creating a broadcast](#creating-a-broadcast)
    - [Sending a broadcast](#sending-a-broadcast)
    - [Checking recipients and broadcast status](#checking-recipients-and-broadcast-status)
    - [Sending a verification email](#sending-a-verification-email)
    - [Checking system activity](#checking-system-activity)
- [Rate Limiting](#rate-limiting)
- [Pagination](#pagination)
- [CORS](#cors)
- [Dates](#dates)
- [Tools](#tools)
- [Support](#support)


## Introduction

This documentation outlines Sentinel API v2.0 which enables the management of users, contacts, groups, broadcasts and documents. Access to the API requires a full Sentinel system which will be available from a url provided to you by YUDU at the time of deployment.

For the purposes of this documentation the url of https://sentinel.comms.zone will be used in all examples. Of course this should be replaced by the url which is provided by YUDU. 


## Terminology

The following terminology is used in this documentation:

- User - A Sentinel user who has access to the Sentinel App. Some users can be given permissions to access the Sentinel backend and also be given api access. They appear in the app directory.
- Contact - A contact listed in the Sentinel backend. They do not have access to the Sentinel app and cannot access the Sentinel backend. They appear in the app directory
- Group - A named collection of assigned users, contacts and documents. 
- Broadcast - A Sentinel broadcast which can be sent by inapp, sms or email methods.
- Document - A YUDU publisher edition. The document can be assigned to individual users, or groups. The documents once assigned will appear in the Sentinel App for a logged in user. 
- Recipient - A recipient is a user or contact which has been added to a broadcast to recieve a broadcast message once sent.
- Directory - A directory is list of users/contacts


## List of Endpoints
The following tables list all of the possible Sentinel API end points and related HTTP methods giving a brief description. They are split into 3 sections. 

- Unprotected - These routes do not require an api token and are publicly accessible. 

- App - These routes require an api token to be passed in the header of each request and are for retrieval of information only. 

- Full access - These routes require an api token to be passed and also require that the api user has full api access permissions. This can be set in the Sentinel backend.  


### Unprotected 

| Method | Route           | Info                                                                                                     |
|--------|-----------------|----------------------------------------------------------------------------------------------------------|
| POST   | /authentication | Expects email and password. Returns api_token, edition node data, and response boolean.                  |
| POST   |  /admin         | Expects an email address, returns true/false as to whether a particular user (email) has backend access. |


### App

| Method | Route                        | Info                                                                               |
|--------|------------------------------|------------------------------------------------------------------------------------|
| GET    | /app/directory               | Returns directory of all Sentinel users and contacts                               |
| GET    | /app/directory/users         | Returns directory of Sentinel users only                                           |
| GET    | /app/directory/users/{id}    | Returns a specific Sentinel user                                                   |
| GET    | /app/directory/contacts      | Returns directory of Sentinel contacts only                                        |
| GET    | /app/directory/contacts/{id} | Return a specific Sentinel contact                                                 |
| GET    | /app/messages                | Returns all inapp broadcast messages that have been sent to the authenticated user |
| GET    | /app/groups                  | Returns a list of all Sentinel Groups the authenticated user belongs to            |
| GET    | /app/groups/directory        | Returns a directory from only the groups the authenticated user belongs to         |
| GET    | /app/groups/{id}/directory   | Returns a directory for a specific group                                           |

### Full Access

| Method | Route                             | Info                                                          |
|--------|-----------------------------------|---------------------------------------------------------------|
| GET    | /users                            | Returns all users                                             |
| GET    | /users/{id}                       | Returns a specific user                                       |
| POST   | /users                            | Creates a new user                                            |
| PUT    | /users/{id}                       | Edits a user                                                  |
| DELETE | /users/{id}                       | Deletes a user                                                |
| GET    | /users/{id}/groups                | Returns all groups a specific user belongs to                 |
| GET    | /users/{id}/broadcasts            | Returns all broadcasts that have been sent to a specific user |
| GET    | /users/{id}/documents             | Returns all documents assigned to a specific user             |
| POST   | /users/{id}/documents/{document}  | Assigns a document to a specific user                         |
| DELETE | /users/{id}/documents/{document}  | Unassigns a document from a specific user                     |
| GET    | /contacts                         | Returns all contacts                                          |
| GET    | /contacts/{id}                    | Returns a specific contact                                    |
| POST   | /contacts                         | Creates a new contact                                         |
| PUT    | /contacts/{id}                    | Edits a contact                                               |
| DELETE | /contacts/{id}                    | Deletes a contact                                             |
| GET    | /groups                           | Returns all groups                                            |
| GET    | /groups/{id}                      | Returns a specific group                                      |
| POST   | /groups                           | Creates a new group                                           |
| PUT    | /groups/{id}                      | Edits a group                                                 |
| DELETE | /groups/{id}                      | Deletes a group                                               |
| GET    | /groups/{id}/users                | Returns all users belonging to a specific group               |
| POST   | /groups/{id}/users/{user}         | Assigns a user to a group                                     |
| DELETE | /groups/{id}/users/{user}         | Unassigns a user from a group                                 |
| GET    | /groups/{id}/contacts             | Returns all contacts belonging to a specific group            |
| POST   | /groups/{id}/contacts/{contact}   | Assigns a contact to a group                                  |
| DELETE | /groups/{id}/contacts/{contact}   | Unassigns a contact from a group                              |
| GET    | /groups/{id}/documents            | Returns all documents assigned to a group                     |
| POST   | /groups/{id}/documents/{document} | Assigns a document to a group                                 |
| DELETE | /groups/{id}/documents/{document} | Unassigns a document from a group                             |
| GET    | /documents                        | Returns all YUDU publisher documents available to Sentinel    |
| GET    | /broadcasts                       | Returns all broadcasts                                        |
| GET    | /broadcasts/{id}                  | Returns a specific broadcast                                  |
| POST   | /broadcasts                       | Creates a broadcast                                           |
| POST   | /broadcasts/{id}/send             | Sends a broadcast                                             |
| POST   | /broadcasts/{id}/archive          | Archives a broadcast                                          |
| POST   | /broadcasts/{id}/unarchive        | Unarchives a broadcast                                        |
| GET    | /recipients                       | Returns all recipients from all broadcasts                    |
| GET    | /recipients/{id}                  | Returns a specific recipient                                  |
| GET    | /broadcasts/{id}/recipients       | Returns all recipients associated with a specific broadcast   |
| GET    | /statistics                       | Returns some basic system information                         |
| GET    | /usage                            | Returns the usage information                                 |
| GET    | /activity                         | Returns the logging activity                                  |
| POST   | /verification/{id}                | Sends a verification email to a specific user                 |


## Request Headers

When making requests to the API it is important to include some specific values in the request header. 

- Ensure that the Accept header is set to application/json. This will ensure json responses are always returned. 

- Ensure the Authorization header is set with the value of Bearer followed by the api token.If the Authorization header is not present then the request will be rejected.

- Set the Content-Type to application/x-www-form-urlencoded for any POST/PUT requests.

``` 
Accept: application/json 
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded
```

## Permissible fields 

The following, details which fields can be passed as parameters when creating or updating records. If any values passed in a request break the rules then a JSON response will be returned detailing what was wrong.


### Users


| Field                          | Rule                                                                  |
|--------------------------------|-----------------------------------------------------------------------|
| name                           | max:50                                                                |
| surname                        | max:50                                                                |
| password                       | min:8 / max:100                                                       |
| email                          | Must be valid email and not already be used by another user / max:250 |
| position                       | max:100                                                               |
| department                     | max:100                                                               |
| company                        | max:100                                                               |
| country                        | Must be a valid country code                                          |
| location                       | max:100                                                               |
| work_mobile_code               | Numeric / Must be a valid dial code                                   |
| work_mobile                    | Numeric / Max:20                                                      |
| work_landline_code             | Numeric / Must be a valid dial code                                   |
| work_landline                  | Numeric / Max:20                                                      |
| personal_mobile_code           | Numeric / Must be a valid dial code                                   |
| personal_mobile                | Numeric / Max:20                                                      |
| personal_landline_code         | Numeric / Must be a valid dial code                                   |
| personal_landline              | Numeric / Max:20                                                      |
| office                         | max:100                                                               |
| reports_to                     | max:100                                                               |
| emergency_contact_name         | max:100                                                               |
| emergency_contact_number_code  | Numeric / Must be a valid dial code                                   |
| emergency_contact_number       | Numeric / Max:20                                                      |
| emergency_contact_relationship | max:100                                                               |
| active                         | boolean 1/0                                                           |
| verified                       | boolean 1/0                                                           |
| visible_in_directory           | boolean 1/0                                                           |
| can_see_private_fields         | boolean 1/0                                                           |
| has_full_api_access            | boolean 1/0                                                           |
| has_backend_access             | boolean 1/0                                                           |

### Contacts


| Field                | Rule                                                                  |
|----------------------|-----------------------------------------------------------------------|
| name                 | max:50                                                                |
| surname              | max:50                                                                |
| mobile_code          | Numeric / Must be a valid dial code                                   |
| mobile               | Numeric / Max:20                                                      |
| landline_code        | Numeric / Must be a valid dial code                                   |
| landline             | Numeric / Max:20                                                      |
| email                | Must be valid email and not already be used by another user / max:250 |
| position             | max:100                                                               |
| company              | max:100                                                               |
| country              | Must be a valid country code                                          |
| location             | max:100                                                               |
| notes                | max:500                                                               |
| active               | boolean 1/0                                                           |
| visible_in_directory | boolean 1/0    


### Groups

| Field     | Rule                        |
|-----------|-----------------------------|
| name      | Must be a unique group name |
| broadcast | boolean 1/0                 |
| document  | boolean 1/0                 |


### Broadcasts


| Field             | Rule                      |
|-------------------|---------------------------|
| title             | min:2 / max:100           |
| sms               | boolean 1/0               |
| inapp             | boolean 1/0               |
| email             | boolean 1/0               |
| message           | min:5 / max:1000          |
| sms_message       | min:5 / max:160           |
| groups            | Array of valid group ID's |
| response_required | boolean 1/0               |
| question          | max:160                   |


## HTTP Responses and errors

When making requests to the API all responses will be returned with an HTTP status code and also depending on the request a JSON response giving an explanation. Belows deatils the response codes returned from the API and what they mean.

**400 BAD REQUEST**
The request cannot be processed because of a client error.

**401 UNAUTHORIZED**
Authentication is required and has not been supplied or failed. 

**403 FORBIDDEN**
The request was valid but you do not have the correct permissions

**404 NOT FOUND**
The requested endpoint or record does not exist

**405 METHOD NOT ALLOWED**
The request method is not allowed for the endpoint. 

**422 UNPROCESSABLE ENTITY**
The request parameters have failed validation

**429 TOO MANY ATTEMPTS**
Too many requests have been made in a short time.

**200 OK**
Successful request

**201 CREATED**
The request was successful and a new record has been created

**204 NO CONTENT**
The request was successful, no content returned

**500 INTERNAL SERVER ERROR**
There was an unexpected internal error


##	Examples

When making calls to the API the routes are preceded by the client domain and /api/v2. e.g 

``` https://yourdomain.comms.zone/api/v2/ ```

Some headers in the example responses have been omitted for brevity.

### Obtaining an api token

As most requests require an api token to be passed the first task would be to obtain the token for all future requests. First ensure you have a valid Sentinel user setup from the Sentinel backend. 


To obtain the api token make the following request, replacing the post data with your relevant credentials.

```
POST /api/v2/authentication HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Content-Type: application/x-www-form-urlencoded

email=m.jackson%40yudu.com&password=jHYG8ghQ£hPPu


```

Providing the user exists in the Sentinel backend and the password is correct a response similar to this will be given:

```

HTTP/1.1 200
status: 200
content-type: application/json
date: Wed, 25 Oct 2017 22:28:38 GMT

{
    "response": 1,
    "token": "YOUR-API-TOKEN-HERE",
    "editions": [
        207790172,
        581340043
    ]
}
```

Every time a request is made to /authentication a new API token is generated. Each token will expire after 30 minutes. If an expired token is used the following response will be returned.

```
HTTP/1.1 401
status: 401
content-type: application/json

{
    "error": "API token has expired."
}

```


### Requesting the App directory

In order to retrieve a paginated App directory make the following request using the api token in the Authorization header.


```
GET /api/v2/app/directory
accept: application/json
authorization: Bearer YOUR-API-TOKEN-HERE
host: sentinel.comms.zone
```

The response will contain a directory of all users and contacts from the sentinel system. The directory can be navigated by using the next_page_url and prev_page_url. Simply make a similar GET request to these urls for the desire page. 

```
HTTP/1.1 200
status: 200
content-type: application/json
date: Wed, 25 Oct 2017 22:34:54 GMT

{
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 1,
                "type": "user",
                "name": "Andrew",
                "surname": "Stevens",
                "email": "a.stevens@yudu.com",
                "position": "Developer",
                "department": "Operations",
                "location": "Clitheroe",
                "work_mobile": "+440559494",
                "work_landline": "+44546650403",
                "country": "GB",
                "office": "CS 1",
                "reports_to": "Charlie",
                "avatar": ""
            },
            {
                "id": 6,
                "type": "user",
                "name": "Caden",
                "surname": "Collier",
                "email": "cmante@example.net",
                "position": "Administrator",
                "department": "Investigations",
                "location": "Cool Village",
                "work_mobile": "+44074698973876",
                "work_landline": "+445466567946",
                "country": "GB",
                "office": "maiores",
                "reports_to": "Mohamed Murray",
                "avatar": ""
            },
            {
                "id": 12,
                "type": "contact",
                "name": "Charlie",
                "surname": "Contact",
                "mobile": "+447937238777",
                "landline": "",
                "email": "charlie.peters@yudu.com",
                "position": "",
                "company": "",
                "avatar": ""
            },
            {
                "id": 7,
                "type": "contact",
                "name": "Lenny",
                "surname": "Cronin",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "barton.dulce@example.net",
                "position": "Manager",
                "company": "Misc",
                "avatar": ""
            },
            {
                "id": 5,
                "type": "contact",
                "name": "Noel",
                "surname": "Franecki",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "weissnat.willa@example.com",
                "position": "Manager",
                "company": "Misc",
                "avatar": ""
            },
            {
                "id": 9,
                "type": "contact",
                "name": "Neil",
                "surname": "Goldner",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "sauer.bessie@example.net",
                "position": "Analyst",
                "company": "Smiths & Sons",
                "avatar": ""
            },
            {
                "id": 12,
                "type": "user",
                "name": "Collin",
                "surname": "Hayes",
                "email": "abigayle.roob@example.com",
                "position": "Developer",
                "department": "Special Operations",
                "location": "Upper Highburn",
                "work_mobile": "+44074698973876",
                "work_landline": "+445466567946",
                "country": "",
                "office": "ea",
                "reports_to": "Holden Graham",
                "avatar": ""
            },
            {
                "id": 9,
                "type": "user",
                "name": "Moriah",
                "surname": "Howell",
                "email": "ipaucek@example.net",
                "position": "Manager",
                "department": "Training",
                "location": "Cool Village",
                "work_mobile": "+44074698973876",
                "work_landline": "+445466567946",
                "country": "US",
                "office": "nihil",
                "reports_to": "Dr. Keely Senger DDS",
                "avatar": ""
            },
            {
                "id": 17,
                "type": "user",
                "name": "Moriah",
                "surname": "Smith",
                "email": "h5hghgh@example.net",
                "position": "Manager",
                "department": "Training",
                "location": "Cool Village",
                "work_mobile": "+440748368256",
                "work_landline": "+4454667555777",
                "country": "US",
                "office": "Clitheroe",
                "reports_to": "Dr. Keely Senger DDS",
                "avatar": ""
            },
            {
                "id": 11,
                "type": "contact",
                "name": "Jamar",
                "surname": "Jacobson",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "ldavis@example.com",
                "position": "Administrator",
                "company": "Smiths & Sons",
                "avatar": ""
            },
            {
                "id": 3,
                "type": "contact",
                "name": "Caroline",
                "surname": "Jast",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "derick.cassin@example.net",
                "position": " Safety Officer",
                "company": "Smiths & Sons",
                "avatar": ""
            },
            {
                "id": 5,
                "type": "user",
                "name": "Sylvia",
                "surname": "Keebler",
                "email": "frami.jewell@example.net",
                "position": "Assistant",
                "department": "Special Operations",
                "location": "Snoozeville",
                "work_mobile": "+44074698973876",
                "work_landline": "+445466567946",
                "country": "RU",
                "office": "voluptate",
                "reports_to": "Emie Wilderman",
                "avatar": ""
            },
            {
                "id": 2,
                "type": "contact",
                "name": "Alene",
                "surname": "Kuphal",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "gunnar16@example.com",
                "position": "Administrator",
                "company": "Bloggs Ltd",
                "avatar": ""
            },
            {
                "id": 7,
                "type": "user",
                "name": "Una",
                "surname": "Lehner",
                "email": "amos.damore@example.org",
                "position": "Sales",
                "department": "Investigations",
                "location": "Royton",
                "work_mobile": "+44074698973876",
                "work_landline": "+445466567946",
                "country": "RU",
                "office": "ut",
                "reports_to": "Dr. Hermann Ledner IV",
                "avatar": ""
            },
            {
                "id": 8,
                "type": "contact",
                "name": "Erica",
                "surname": "Mante",
                "mobile": "+44425672457254",
                "landline": "+448256825682",
                "email": "cali87@example.com",
                "position": "Consultant",
                "company": "Acme",
                "avatar": ""
            }
        ],
        "from": 1,
        "last_page": 2,
        "next_page_url": "https://sentinel.comms.zone/api/v2/app/directory?page=2",
        "path": "https://sentinel.comms.zone/api/v2/app/directory",
        "per_page": 15,
        "prev_page_url": null,
        "to": 15,
        "total": 30
    }
}
```


### Creating a user
	
Create a user by making the following request. These fields are the minimum required in order to create user.

```
POST /api/v2/users
accept: application/json
authorization: Bearer YOUR-API-TOKEN-HERE
content-type: application/x-www-form-urlencoded
host: sentinel.comms.zone

name=Peter&surname=Smith&email=peter.smith@yudu.com&password=HYhs^&£jLOPk
```

Once created a successful response will look as follows.

```
HTTP/1.1 201
status: 201
content-type: application/json
date: Wed, 25 Oct 2017 22:51:17 GMT

{
    "success": {
        "message": "Record Created.",
        "status_code": 201,
        "data": {
            "id": 19,
            "name": "Peter",
            "surname": "Smith",
            "email": "peter.smith@yudu.com",
            "avatar": "",
            "position": "",
            "department": "",
            "company": "",
            "country": "",
            "location": "",
            "office": "",
            "reports_to": "",
            "work_mobile": null,
            "work_landline": null,
            "personal_mobile": null,
            "personal_landline": null,
            "emergency_contact_name": "",
            "emergency_contact_number": null,
            "emergency_contact_relationship": "",
            "active": true,
            "verified": false,
            "visible_in_directory": true,
            "can_see_private_fields": false,
            "has_full_api_access": false,
            "has_backend_access": false,
            "created": "2017-10-25T22:51:17+00:00",
            "updated": "2017-10-25T22:51:17+00:00"
        }
    }
}
```	
	
### Creating a contact
	
Creating a contact is similar to creating a user.

```
POST /api/v2/contacts HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded

name=Albert&surname=Roux&mobile_code=44&mobile=38957438573&email=a.roux%40yudu.com
```

Which returns the following:

```
HTTP/1.1 201
status: 201
content-type: application/json
date: Wed, 25 Oct 2017 22:57:25 GMT

{
    "success": {
        "message": "Record Created.",
        "status_code": 201,
        "data": {
            "id": 14,
            "name": "Albert",
            "surname": "Roux",
            "mobile": "+4438957438573",
            "landline": null,
            "email": "a.roux@yudu.com",
            "position": null,
            "company": null,
            "location": null,
            "active": true,
            "visible_in_directory": true,
            "avatar": "",
            "created": "2017-10-26T09:16:54+00:00",
            "updated": "2017-10-26T09:16:54+00:00"
        }
    }
}
```

### Creating a group

To create a group a unique name must be specified. Also it must be specified if the group type is broadcast, document or both. At least one group type must be chosen.

```
POST /api/v2/groups
accept: application/json
authorization: Bearer YOUR-API-TOKEN-HERE
content-type: application/x-www-form-urlencoded
host: sentinel.comms.zone

name=Group+1&broadcast=1&document=1
```

The successful response will look as follows.

```
HTTP/1.1 201
status: 201
content-type: application/json
date: Wed, 25 Oct 2017 23:04:35 GMT

{
    "success": {
        "message": "Record Created.",
        "status_code": 201,
        "data": {
            "id": 6,
            "name": "Group 1",
            "broadcast": true,
            "document": true,
            "created": "2017-10-26T08:51:58+00:00",
            "updated": "2017-10-26T08:51:58+00:00"
        }
    }
}
```

### Assigning a user to a group

Once there are users and groups availble. Using the user and group IDs it is simple to assign users to groups.

POSTING to the following endpoint replacing the parameters in curly braces with the group and user IDs.

``` https://sentinel.comms.zone/groups/{id}/users/{user} ```


The following request can be made to assign User with ID 19 to group with ID  6.

```
POST /api/v2/groups/6/users/19
accept: application/json
authorization: Bearer YOUR-API-TOKEN-HERE
content-type: application/x-www-form-urlencoded
host: sentinel.comms.zone
```

The response will return a 204 with no content if the user is successfully assigned to the group.

```
HTTP/1.1 204
status: 204
date: Thu, 26 Oct 2017 09:00:03 GMT
```

### Getting a groups users

To look up a specific group and see what users are assigned to it, make the following request:

```
GET /api/v2/groups/6/users HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```

A response like this will be returned. It can be seen that the user that was assigned is indeed now a member of the group.

```
HTTP/1.1 200
status: 200
content-type: application/json
date: Thu, 26 Oct 2017 09:03:52 GMT

{
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 19,
                "name": "Peter",
                "surname": "Smith",
                "email": "peter.smith@yudu.com",
                "avatar": "",
                "position": "",
                "department": "",
                "company": "",
                "country": "",
                "location": "",
                "office": "",
                "reports_to": "",
                "work_mobile": null,
                "work_landline": null,
                "personal_mobile": null,
                "personal_landline": null,
                "emergency_contact_name": "",
                "emergency_contact_number": null,
                "emergency_contact_relationship": "",
                "active": true,
                "verified": false,
                "visible_in_directory": true,
                "can_see_private_fields": false,
                "has_full_api_access": false,
                "has_backend_access": false,
                "created": "2017-10-25T22:51:17+00:00",
                "updated": "2017-10-25T22:51:17+00:00"
            }
        ],
        "from": 1,
        "last_page": 1,
        "next_page_url": null,
        "path": "https://sentinel.comms.zone/api/v2/groups/6/users",
        "per_page": 15,
        "prev_page_url": null,
        "to": 1,
        "total": 1
    }
}

```


### Assigning a contact to a group

Assigning a contact to a group is similar to assigning a user. So POSTing to the following endpoint replacing the parameters in curly braces with the group and contact IDs.

``` 
https://sentinel.comms.zone/groups/{id}/contacts/{contact}
```

```
POST /api/v2/groups/6/contacts/14 HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded
```

The response will return a 204 with no content if the contact is successfully assigned to the group.

```
HTTP/1.1 204
status: 204
date: Thu, 26 Oct 2017 09:51:50 GMT
```

### Requesting a list of documents

Documents are automatically syncronized between YUDU publisher and Sentinel. So any documents that are published will appear listed in Sentinel automatically. At any time to see which documents are available then make the following request.

```
GET /api/v2/documents HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded
```

Providing there are YUDU publisher documents available in YUDU publisher a response like below will be returned.

```
HTTP/1.1 200
status: 200
content-type: application/json
date: Thu, 26 Oct 2017 10:04:56 GMT

{
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 1,
                "node": 485930,
                "name": "First Aid - heart Attack",
                "thumbnail": "https://publisher.yudu.com/Yudu/public/thumbnail.jpg?pageId=4938493479384"
            },
            {
                "id": 2,
                "node": 934892,
                "name": "Generic Emergency Evacuation Plan",
                "thumbnail": "https://publisher.yudu.com/Yudu/public/thumbnail.jpg?pageId=534095759894"
            },
            {
                "id": 3,
                "node": 947399,
                "name": "Fire Drill",
                "thumbnail": "https://publisher.yudu.com/Yudu/public/thumbnail.jpg?pageId=2937129846234"
            }
        ],
        "from": 1,
        "last_page": 1,
        "next_page_url": null,
        "path": "https://sentinel.comms.zone/api/v2/documents",
        "per_page": 15,
        "prev_page_url": null,
        "to": 3,
        "total": 3
    }
}
```

If you need help adding documents to YUDU publisher please contact [support@yudu.com](support@yudu.com)

### Assigning a document to a user

Using the document information from the previous step it is possible to assign specific documents to users. Once assigned these documents will be visble to the user when they log into the Sentinel App. 

Make a request like the following to assign a document to a user. This will assign Document ID 1 (First Aid - heart attack) to User ID 19 (Peter Smith)

```
POST /api/v2/users/19/documents/1
accept: application/json
authorization: Bearer YOUR-API-TOKEN-HERE
host: sentinel.comms.zone
```

Once assigned successfully a 204 response will be returned.

```
HTTP/1.1 204
status: 204
date: Thu, 26 Oct 2017 10:24:44 GMT
```

### Getting a users' documents 

To check which documents are already assigned to a user make the following request. 

```
GET /api/v2/users/19/documents HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache
Postman-Token: 605c9eb3-ecb8-a8f6-e00f-478e4660ee25
```

The response will show which documents have been assigned. In this example it can be seen that the First Aid - heart Attack document is assigned to User ID 19 (Peter Smith).


```
HTTP/1.1 200
status: 200
content-type: application/json
date: Thu, 26 Oct 2017 10:26:44 GMT
```

### Creating a broadcast

Once groups, users and contacts have been setup it is possible to create broadcasts in preparation for sending. 

It is important to specify at least one send method for the broadcast from the available inapp, sms and email methods. Also an array of group ID's must be specified. The recipients of any broadcast is made up from any groups users and contacts.

In this example broadcast, a fire alert will be broadacst by all 3 broadcast methods. The broadcast will require a response from all recipients. The broadcast will be sent to all members of a group with the ID of 6.

```
POST /api/v2/broadcasts HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
Content-Type: application/x-www-form-urlencoded

title=Fire+Alert&message=A+fire+has+broken+out+in+the+kitchens+of+Building+A.+Please+exit+all+company+buildings+and+meet+at+your+designated+fire+assembly+point.+&sms=1&sms_message=A+fire+has+broken+out%2C+please+exit+all+buildings+safely.&inapp=1&email=1&groups%5B%5D=6&response_required=1&question=Are+you+at+the+fire+assembly+point%3F
```

Once recieved the broadcast will be created and the following response will be given. Please note that the broadcast has only been created in the system. It has not been sent yet. 

```
HTTP/1.1 201
status: 201
content-type: application/json
date: Thu, 26 Oct 2017 10:55:41 GMT

{
    "success": {
        "message": "Record Created.",
        "status_code": 201,
        "data": {
            "id": 28,
            "title": "Fire Alert",
            "sms": true,
            "email": true,
            "inapp": true,
            "message": "A fire has broken out in the kitchens of Building A. Please exit all company buildings and meet at your designated fire assembly point.",
            "sms_message": "A fire has broken out, please exit all buildings safely.",
            "response_required": true,
            "question": "Are you at the fire assembly point?",
            "sent": false,
            "archived": false,
            "created": "2017-10-26T10:55:40+00:00",
            "updated": "2017-10-26T10:55:40+00:00"
        }
    }
}
```


### Sending a broadcast

When the time comes to send a particular broadcast this can be done by simply using the Broadcast ID of the broadcast. Make the following request to send broadcast ID 28 which was created in the previous step.

```
POST /api/v2/broadcasts/28/send HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```

If the broadcast has not been sent already then a 204 response will be given. The broadcasts will be queued to be sent out via the specified methods. 

```
HTTP/1.1 204
status: 204
date: Thu, 26 Oct 2017 11:01:28 GMT
```

If you wish to resend a broadcast the same request cannot be made again. A new broadcast would need to be created. 

If the request is made again for the same broadcast the following response will be given. 

```
POST /api/v2/broadcasts/28/send HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE

{
    "error": {
        "message": "This broadcast has already been sent",
        "status_code": 400
    }
}
```

### Checking recipients and broadcast status

Once a broadacast has been sent the status of each recipient can be checked. This is useful to find out if a broadcast has been sent successfully through a broadcast channel for example if a SMS message has been sent. Also responses to any questions can be checked. 

To check the recipients for the the above broadcast ID 28. The following request is made.

```
GET /api/v2/broadcasts/28/recipients HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```

This will return a list of all recipient data for the broadcast.

```
HTTP/1.1 200
status: 200
content-type: application/json
date: Thu, 26 Oct 2017 11:11:35 GMT


{
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 345,
                "broadcast_id": 28,
                "type": "User",
                "name": "Peter",
                "surname": "Smith",
                "email": "peter.smith@yudu.com",
                "mobile": "",
                "sms_status": "Not Sent",
                "email_status": "Failed",
                "inapp_status": "Sent",
                "response_status": 0,
                "response": null,
                "token": "59f1c0635f783"
            },
            {
                "id": 346,
                "broadcast_id": 28,
                "type": "Contact",
                "name": "Albert",
                "surname": "Roux",
                "email": "a.roux@yudu.com",
                "mobile": "4438957438573",
                "sms_status": "Failed",
                "email_status": "Failed",
                "inapp_status": "Sent",
                "response_status": 0,
                "response": null,
                "token": "59f1c0636859e"
            }
        ],
        "from": 1,
        "last_page": 1,
        "next_page_url": null,
        "path": "https://sentinel.comms.zone/api/v2/broadcasts/28/recipients",
        "per_page": 15,
        "prev_page_url": null,
        "to": 2,
        "total": 2
    }
}
```


### Sending a verification email

It is possible to send a verification email out to a user. Once recieved the user will be able to  follow a link which will allow them to update their user profile by themselves. This is a good way to periodically ensure all user data is up to date. Also it allows the user to remove any data they do not wish to be held about them. Once sent, the email verification link will be available for 24hours.

To send a verification email create the following request.

```
POST /api/v2/verification/19 HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```

If successful a 204 response will be returned. The user will receive an email in their inbox shortly after.

```
HTTP/1.1 204
status: 204
date: Thu, 26 Oct 2017 11:27:54 GMT
```

### Checking system activity

When actions take place in Sentinel events are logged. You may want to retrieve a list of events that have taken place in order to see what has happened between a sepcific period of time. Or you may wish to see the most recent activity. 

To retrieve a paginated list of the most recent activity make the following request.

```
GET /api/v2/activity
Host: sentinel.comms.zone
accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```

This will return the last 30 days of activity by default. Something like the following response.

```
HTTP/1.1 200
status: 200
content-type: application/json
date: Wed, 01 Nov 2017 18:43:36 GMT

{
    "data": {
        "current_page": 1,
        "data": [
            {
                "id": 585,
                "created_by": "Peter Smith",
                "name": "updated_recipient",
                "message": "Admin user Andrew Bibby responded to broadcast #28 for Albert Roux",
                "created_at": "2017-11-01T15:01:56+00:00"
            },
            {
                "id": 581,
                "created_by": "Peter Smith",
                "name": "logged_in",
                "message": "Logged in",
                "created_at": "2017-11-01T15:00:24+00:00"
            },
            {
                "id": 580,
                "created_by": "Charlie Stephenson",
                "name": "logged_in",
                "message": "Logged in",
                "created_at": "2017-11-01T14:30:47+00:00"
            },
            {
                "id": 579,
                "created_by": "Shaun Black",
                "name": "deleted_contact",
                "message": "deleted contact Charlie Peters",
                "created_at": "2017-11-01T12:09:13+00:00"
            },
            {
                "id": 571,
                "created_by": "Shaun Black",
                "name": "deleted_contact",
                "message": "deleted contact Andy Brown",
                "created_at": "2017-11-01T11:53:06+00:00"
            }
        ],
        "from": 1,
        "last_page": 1,
        "next_page_url": null,
        "path": "https://sentinel.comms.zone/api/v2/activity",
        "per_page": 100,
        "prev_page_url": null,
        "to": 5,
        "total": 5
    }
}
```

It is possible to retrieve activity between 2 dates by specifying start and end date query parameters. So for example to return all activity between 1st October 2017 and 31st October 2017 then make the following request.

```
GET /api/v2/activity?start=2017-10-01 00:00:00&end=2017-10-03 23:59:59 HTTP/1.1
Host: sentinel.comms.zone
Accept: application/json
Authorization: Bearer YOUR-API-TOKEN-HERE
```
 

## Rate Limiting

Requests to the Sentinel API are limited currently to 100 requests per minute. When making requests to the API all responses will include rate limit headers. These headers can indicate how many requests are remaining for a given minute. They will look like below.

```
x-ratelimit-limit: 100
x-ratelimit-remaining: 99
``` 

If too many requests are made within a minute a 429 Too many requests error will be returned. The retry-after header will indicate how many seconds must be waited until a request will be accepted again. The x-ratelimit-reset header provides a Unix UTC timestamp, which is the exact time that a new rate limit kicks in.

```
HTTP/1.1 429
status: 429
date: Thu, 26 Oct 2017 11:52:44 GMT
x-ratelimit-limit: 100
x-ratelimit-remaining: 0
retry-after: 60
x-ratelimit-reset: 1509018824
```


## Pagination

Data returned is paginated when there is more than 1 record returned. So for example if all users are requested the return JSON is split into manageable chunks. After the data at the end of the response there will be some metadata which gives information about the results. This is particularly useful as it can be used to access the next or previous pages of information.  

```
"from": 16,
"last_page": 3,
"next_page_url": "https://sentinel.comms.zone/api/v2/users?page=3",
"path": "https://sentinel.comms.zone/api/v2/users",
"per_page": 15,
"prev_page_url": "https://sentinel.comms.zone/api/v2/users?page=1",
"to": 30,
"total": 37
```

By default per_page value is 100. Which means for every request that is paginated 100 records will be returned. This value can be changed to whatever is required. Simply pass a per_page parameter along with the request specifying how many records should be returned per page. The minimum is 1 and the maximum is 1000. Any values outside of this range will return the default 100 records. 


## CORS

The Sentinel API supports CORS or Cross Origin Resource Sharing. Without CORS it would not be possible to retrieve information for example in an app or a website on a different domain from the Sentinel API. The Sentinel API supports CORS requests from any domain.

## Dates

All dates returned in any responses are in Coordinated Universal Time (UTC) and shown in ISO 8601 format.

## Tools

When using the API you may find it useful to test using Postman. Postman is some useful software which will assist you in making the requests detailed in this documentation. It can be downloaded from [https://www.getpostman.com/](https://www.getpostman.com/). All raw requests and responses can be seen through the Postman console and will speed up your development time and can be useful if support is required. 


## Support

If you require any assistance with this documentation please contact support at [support@yudu.com](support@yudu.com)

