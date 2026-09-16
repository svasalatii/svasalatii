# [PLATFORM] User Management API Guide

## Table of contents
- [\[PLATFORM\] User Management API Guide](#platform-user-management-api-guide)
  - [Table of contents](#table-of-contents)
  - [Overview](#overview)
    - [Base URL](#base-url)
    - [Authentication](#authentication)
    - [Request format](#request-format)
  - [Creating API token in \[PLATFORM\]](#creating-api-token-in-platform)
  - [User management](#user-management)
    - [Create user](#create-user)
    - [Update user](#update-user)
    - [Delete user](#delete-user)
  - [User group management](#user-group-management)
    - [Create user group](#create-user-group)
    - [Update user group](#update-user-group)
  - [HTTP status codes](#http-status-codes)
  - [Error response](#error-response)

---

## Overview

The [PLATFORM] User Management API enables you to create, update, and delete users and user groups.

### Base URL

```text
https://api.platform.com/v1
```

### Authentication

All API requests require a bearer token.

```http
Authorization: Bearer <access_token>
```

### Request format

Send request bodies as JSON and include the following header:

```http
Content-Type: application/json
```
---

## Creating API token in [PLATFORM]
To authenticate API requests, create an API token in the [PLATFORM] **Admin panel**. In the navigation menu, find and open the **API Tokens** section, then click **Create API token**.
<div align="center">
<img src="../Images/Platform User Management API Guide/APITokens_Menu-Access.png" alt="Access API Tokens" width="600"/>
<p><em>Figure 1. Accessing API Tokens menu</em></p>
</div>

In the **Create New API Token** window that opens next, enter a name for the token and select its type. *Full account admin access* is usually required for administrative API operations. You can also indicate a user group whose members will be allowed to use the API token, as well as the scope of the API token operation - access to what component(s) or module(s) it will enable. Click **Create**.
<div align="center">
<img src="../Images/Platform User Management API Guide/APITokens_Settings-NDA.png" alt="Populate Token Form" width="600"/>
<p><em>Figure 2. Configuring API token</em></p>
</div>

The new token is created and displayed. Copy it to a secure local file and use it for API authentication when sending requests. Do not share the token or include it in publicly accessible files.
<div align="center">
<img src="../Images/Platform User Management API Guide/APITokens_CopyDone-NDA.png" alt="Copy Token" width="600"/>
<p><em>Figure 3. Copying API token</em></p>
</div>

---

## User management

### Create user

Creates a new user.

**Endpoint**

```http
POST /users
```

**Request body**

```json
{
  "email": "john.doe@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "groupId": "group_123"
}
```

| Field | Type | Required | Description |
| ----- | ---- | :------: | ----------- |
| `email` | string | Yes | User's email address. |
| `firstName` | string | Yes | User's first name. |
| `lastName` | string | Yes | User's last name. |
| `groupId` | string | No | ID of the group to which the user belongs. |

**Example request**

```bash
curl -X POST "https://api.platform.com/v1/users" \\
  -H "Authorization: Bearer <access_token>" \\
  -H "Content-Type: application/json" \\
  -d '{
    "email": "john.doe@example.com",
    "firstName": "John",
    "lastName": "Doe",
    "groupId": "group_123"
  }'
```

**Example response**

```json
{
  "id": "user_123",
  "email": "john.doe@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "groupId": "group_123",
  "createdAt": "2026-01-15T10:30:00Z"
}
```

---

### Update user

Updates an existing user.

**Endpoint**

```http
PATCH /users/{userId}
```

**Path parameter**

| Parameter | Type | Required | Description |
| --------- | ---- | :------: | ----------- |
| `userId` | string | Yes | ID of the user to update. |

**Request body**

Include only those fields you want to change.

```json
{
  "firstName": "Jonathan",
  "lastName": "Doe",
  "groupId": "group_456"
  "email": "jdoe@platform.com"
}
```

| Field | Type | Required | Description |
| ----- | ---- | :------: | ----------- |
| `firstName` | string | No | Updated first name. |
| `lastName` | string | No | Updated last name. |
| `groupId` | string | No | Updated group ID. |
| `email` | string | No | Updated email address. |

**Example request**

```bash
curl -X PATCH "https://api.platform.com/v1/users/user_123" \\
  -H "Authorization: Bearer <access_token>" \\
  -H "Content-Type: application/json" \\
  -d '{
    "firstName": "Jonathan",
    "lastName": "Doe",
    "groupId": "group_456"
    "email": jdoe@platform.com
  }'
```

**Example response**

```json
{
  "id": "user_123",
  "email": "jdoe@platform.com",
  "firstName": "Jonathan",
  "lastName": "Doe",
  "groupId": "group_456",
  "updatedAt": "2026-01-15T11:00:00Z"
}
```

---

### Delete user

Deletes an existing user.

**Endpoint**

```http
DELETE /users/{userId}
```

**Path parameter**

| Parameter | Type | Required | Description |
| --------- | ---- | :------: | ----------- |
| `userId` | string | Yes | ID of the user to delete. |

**Example request**

```bash
curl -X DELETE "https://api.platform.com/v1/users/user_123" \\
  -H "Authorization: Bearer <access_token>"
```

**Example response**

A successful request returns:

```http
204 No Content
```

---

## User group management

### Create user group

Creates a new user group.

**Endpoint**

```http
POST /groups
```

**Request body**

```json
{
  "name": "Administrators",
  "description": "Users with administrative permissions."
}
```

| Field | Type | Required | Description |
| ----- | ---- | :------: | ----------- |
| `name` | string | Yes | Name of the user group. |
| `description` | string | No | Description of the user group. |

**Example request**

```bash
curl -X POST "https://api.platform.com/v1/groups" \\
  -H "Authorization: Bearer <access_token>" \\
  -H "Content-Type: application/json" \\
  -d '{
    "name": "Administrators",
    "description": "Users with administrative permissions."
  }'
```

**Example response**

```json
{
  "id": "group_123",
  "name": "Administrators",
  "description": "Users with administrative permissions.",
  "createdAt": "2026-01-15T10:00:00Z"
}
```

---

### Update user group

Updates an existing user group.

**Endpoint**

```http
PATCH /groups/{groupId}
```

**Path parameter**

| Parameter | Type | Required | Description |
| --------- | ---- | :------: | ----------- |
| `groupId` | string | Yes | ID of the group to update. |

**Request body**

```json
{
  "name": "Platform Administrators",
  "description": "Users with platform administration permissions."
}
```

| Field | Type | Required | Description |
| ----- | ---- | :------: | ----------- |
| `name` | string | No | Updated group name. |
| `description` | string | No | Updated group description. |

**Example request**

```bash
curl -X PATCH "https://api.platform.com/v1/groups/group_123" \\
  -H "Authorization: Bearer <access_token>" \\
  -H "Content-Type: application/json" \\
  -d '{
    "name": "Platform Administrators",
    "description": "Users with platform administration permissions."
  }'
```

**Example response**

```json
{
  "id": "group_123",
  "name": "Platform Administrators",
  "description": "Users with platform administration permissions.",
  "updatedAt": "2026-01-15T11:15:00Z"
}
```

---

## HTTP status codes

| Status code | Meaning |
| ----------- | ------- |
| `200 OK` | The request was successful and returned a response body. |
| `201 Created` | A user or group was created successfully. |
| `204 No Content` | The request was successful and returned no response body. |
| `400 Bad Request` | The request body or parameters are invalid. |
| `401 Unauthorized` | Authentication failed or the access token is missing. |
| `403 Forbidden` | The authenticated client does not have permission to perform the operation. |
| `404 Not Found` | The specified user or group does not exist. |
| `409 Conflict` | The request conflicts with an existing resource. |
| `500 Internal Server Error` | An unexpected server error occurred. |

## Error response

Errors are returned as JSON objects.

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "The specified user was not found."
  }
}
```

| Field | Type | Description |
| ----- | ---- | ----------- |
| `error.code` | string | Machine-readable error code. |
| `error.message` | string | Human-readable error description. |
