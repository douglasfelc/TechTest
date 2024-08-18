# API REST Resources Documentation

See the diagrams in the general documentation, in the `Architecture Overview` section.

[Click here](../README.md) to return to general documentation.


### GET /post

Description: Retrieves all posts of the logged in user, and returns with pagination. \
Authentication: Required.

**Method:** GET \
**URL:** /post

#### Response:

**Status:** 200 OK \
**Content-Type:** application/json

Body:

```bash
{
  "totalItems": 100,
  "currentPage": 1,
  "itemsPerPage": 10,
  "totalPages": 10,
  "data": [
    {
      "id": 1,
      "content": "Sample Data",
      "createdAt": "2024-08-12T10:30:00.000Z"
    },
    {
      "id": 2,
      "content": "Sample Data",
      "createdAt": "2024-08-12T11:00:00.000Z"
    }
  ]
}
```

#### Error Responses:

* 400: Bad Request
* 401: Unauthorized
* 500: Internal Server Error


### POST /post

Description: Stores a new post from the logged in user in the database. \
Authentication: Required.

#### Request:

**Method:** POST \
**URL:** /post

**Content-Type:** application/json

Body:

```bash
{
  "content": "Sample Data"
}
```

#### Response:

**Status:** 201 Created \
**Content-Type:** application/json

Body:

```bash
{
  "id": 3,
  "content": "Sample Data",
  "createdAt": "2024-08-12T12:00:00.000Z"
}
```

#### Error Responses:

* 400: Bad Request
* 401: Unauthorized
* 500: Internal Server Error
