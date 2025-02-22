# GitHub User API Specification
**Base URL**: `https://api.github.com`
**API Version**: `2022-11-28`
**Content-Type**: `application/vnd.api+json`

## Endpoints

### Get User Profile
```http
GET /users/ewdlop
```

#### Response
```json
{
  "data": {
    "type": "users",
    "id": "ewdlop",
    "attributes": {
      "login": "ewdlop",
      "name": "string",
      "bio": "string",
      "created_at": "string",
      "updated_at": "string",
      "public_repos": "integer",
      "followers": "integer",
      "following": "integer"
    },
    "links": {
      "self": "https://api.github.com/users/ewdlop"
    }
  }
}
```

### List User Repositories
```http
GET /users/ewdlop/repos
```

#### Parameters
| Name     | Type    | Description                                      |
|----------|---------|--------------------------------------------------|
| sort     | string  | Can be one of: created, updated, pushed, full_name |
| direction| string  | Can be one of: asc or desc                        |
| per_page | integer | Results per page (max 100)                        |
| page     | integer | Page number of the results to fetch               |

#### Response
```json
{
  "data": [{
    "type": "repositories",
    "id": "string",
    "attributes": {
      "name": "string",
      "full_name": "string",
      "description": "string",
      "private": "boolean",
      "fork": "boolean",
      "created_at": "string",
      "updated_at": "string",
      "language": "string"
    },
    "relationships": {
      "owner": {
        "data": {
          "type": "users",
          "id": "ewdlop"
        }
      }
    },
    "links": {
      "self": "https://api.github.com/repos/ewdlop/{repo_name}"
    }
  }],
  "links": {
    "self": "https://api.github.com/users/ewdlop/repos",
    "first": "https://api.github.com/users/ewdlop/repos?page=1",
    "prev": "https://api.github.com/users/ewdlop/repos?page={prev_page}",
    "next": "https://api.github.com/users/ewdlop/repos?page={next_page}",
    "last": "https://api.github.com/users/ewdlop/repos?page={last_page}"
  },
  "meta": {
    "total_count": "integer"
  }
}
```

### List User's Starred Repositories
```http
GET /users/ewdlop/starred
```

#### Response
```json
{
  "data": [{
    "type": "repositories",
    "id": "string",
    "attributes": {
      "name": "string",
      "full_name": "string",
      "description": "string",
      "starred_at": "string"
    },
    "links": {
      "self": "https://api.github.com/repos/{owner}/{repo}"
    }
  }],
  "links": {
    "self": "https://api.github.com/users/ewdlop/starred",
    "first": "https://api.github.com/users/ewdlop/starred?page=1",
    "prev": "https://api.github.com/users/ewdlop/starred?page={prev_page}",
    "next": "https://api.github.com/users/ewdlop/starred?page={next_page}",
    "last": "https://api.github.com/users/ewdlop/starred?page={last_page}"
  }
}
```

### List User's Followers
```http
GET /users/ewdlop/followers
```

#### Response
```json
{
  "data": [{
    "type": "users",
    "id": "string",
    "attributes": {
      "login": "string",
      "avatar_url": "string"
    },
    "links": {
      "self": "https://api.github.com/users/{username}"
    }
  }],
  "links": {
    "self": "https://api.github.com/users/ewdlop/followers"
  }
}
```

### List User's Following
```http
GET /users/ewdlop/following
```

#### Response
```json
{
  "data": [{
    "type": "users",
    "id": "string",
    "attributes": {
      "login": "string",
      "avatar_url": "string"
    },
    "links": {
      "self": "https://api.github.com/users/{username}"
    }
  }],
  "links": {
    "self": "https://api.github.com/users/ewdlop/following"
  }
}
```

## Error Responses

When an error occurs, you may receive a response in this format:

```json
{
  "errors": [{
    "status": "404",
    "title": "Resource not found",
    "detail": "The requested resource could not be found"
  }]
}
```

## Common Status Codes

| Status Code | Description                                      |
|-------------|--------------------------------------------------|
| 200         | Success                                          |
| 304         | Not modified                                     |
| 401         | Requires authentication                          |
| 403         | Forbidden                                        |
| 404         | Resource not found                               |
| 422         | Validation failed                                |
| 429         | Too many requests                                |

## Authentication

All requests must include one of the following authentication methods:

1. OAuth2 token in Authorization header:
```http
Authorization: Bearer your-token
```

2. Personal access token:
```http
Authorization: token your-personal-access-token
```

## Rate Limiting

API calls are subject to rate limiting. The current limits are returned in the response headers:

```http
X-RateLimit-Limit: 5000
X-RateLimit-Remaining: 4999
X-RateLimit-Reset: 1372700873
```
