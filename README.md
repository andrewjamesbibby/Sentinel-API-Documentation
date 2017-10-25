# Sentinel API Documentation
YUDU Sentinel API Documentation - Version 2.0




## Contents


## Introduction

This documentation outlines Sentinel API v2.0 which enables the management of users, contacts, groups, broadcasts and documents. Access to the API requires a full Sentinel system which will be available from a url provided to you by YUDU at the time of deployment.

For the purposes of this documentation the url of https://sentinel.comms.zone will be used in all examples. Of course this should be replaced by the url which is provided by YUDU. 


## Terminology

The following terminology is used in this documentation:

- User - A Sentinel user who has access to the Sentinel App. Some users can be given permissions to access the Sentinel backend and also be given api access. They appear in the app directory.
- Contact - A contact listed in the Sentinel backend. They do not have access to the Sentinel app and cannot access the Sentinel backend. They appear in the app directory
- Group - A named collections of assigned users, contacts and documents. 
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

| GET    | /users                            | Returns all users                                             |
|--------|-----------------------------------|---------------------------------------------------------------|
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
| GET    | /verification/{id}                | Sends a verification email to a specific user                 |


## Request Headers

When making requests to the API it is important to include some specific values in the request header. 

- Ensure that the Accept header is set to application/json. This will ensure json responses are always returned. 

- Ensure the Authorization header is set with the value of Bearer followed by the api token.If the Authorization header is not present then the request will be rejected.

- Set the Content-Type to application/x-www-form-urlencoded for any POST/PUT requests.

``` 
Accept: application/json 
Authorization: Bearer 943hf4983tLKHJSLWPW
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
The requested record does not exist

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

``` https://sentinel.comms.zone/api/v2/ ```

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
    "token": "oxJflpOXP2KGKkQ9t79GLS1T3AP3OFn7R3kXuWpiE9SaPRKc5zjUIZOmTi0B",
    "editions": [
        207790172,
        581340043
    ]
}
```

### Requesting the App directory

In order to retrieve a paginated App directory make the following request using the api token in the Authorization header.


```
GET /api/v2/app/directory
accept: application/json
authorization: Bearer vpDt63w22qc2WjIibQddw1HikTyauvq63W5AovjuiOznSCQYg08EXfCBY2A7
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
authorization: Bearer vpDt63w22qc2WjIibQddw1HikTyauvq63W5AovjuiOznSCQYg08EXfCBY2A7
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
POST /api/v2/contacts
accept: application/json
authorization: Bearer vpDt63w22qc2WjIibQddw1HikTyauvq63W5AovjuiOznSCQYg08EXfCBY2A7
content-type: application/x-www-form-urlencoded
host: sentinel.comms.zone

name=Peter&surname=Smith&email=peter.smith@yudu.com&mobile_code=44&mobile=284938493999
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
            "id": 13,
            "name": "Peter",
            "surname": "Smith",
            "mobile": "+44284938493999",
            "landline": null,
            "email": "peter.smith@yudu.com",
            "position": null,
            "company": null,
            "location": null,
            "active": false,
            "visible_in_directory": false,
            "avatar": "",
            "created": "2017-10-25T22:57:25+00:00",
            "updated": "2017-10-25T22:57:25+00:00"
        }
    }
}

```

### Creating a group

To create a group a unique name must be specified. Also it must be specified if the group type is broadcast, document or both. At least one group type must be chosen.

```
POST /api/v2/groups
accept: application/json
authorization: Bearer vpDt63w22qc2WjIibQddw1HikTyauvq63W5AovjuiOznSCQYg08EXfCBY2A7
content-type: application/x-www-form-urlencoded
host: sentinel.comms.zone
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
            "id": 5,
            "name": "Group 1",
            "broadcast": true,
            "document": true,
            "created": "2017-10-25T23:04:35+00:00",
            "updated": "2017-10-25T23:04:35+00:00"
        }
    }
}
```

### Assigning a user to a group

### Assigning a contact to a group

### Creating a broadcast

### Sending a broadcast
 


## Support

If you require any assistance with this documentation please contact support at [support@yudu.com](fgdf)




