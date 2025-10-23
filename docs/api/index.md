# Taskly API Reference

Welcome! This page documents the core endpoints for the Taskly API using the OpenAPI 3.0 specification.  
All endpoints are organized for quick reference and clarity, even if the backend is only a demo.

## Overview

- **Base URL:** `https://api.taskly.app/v1`
- **Auth Method:** Bearer token, via header `Authorization: Bearer DEMO_KEY`
- **Content Type:** `application/json`

---

## Raw OpenAPI YAML

openapi: 3.0.3
info:
title: Taskly API
description: Simple demo Task management API
version: "1.0.0"
servers:

url: https://api.taskly.app/v1
paths:
/tasks:
get:
summary: List tasks
responses:
'200':
description: OK
post:
summary: Create a task
requestBody:
required: true
content:
application/json:
schema:
$ref: '#/components/schemas/NewTask'
responses:
'201':
description: Created
/tasks/{id}:
get:
summary: Get task
parameters:
- name: id
in: path
required: true
schema:
type: string
responses:
'200':
description: OK
components:
schemas:
NewTask:
type: object
properties:
title:
type: string
due:
type: string
format: date
required:
- title


---


---

## Endpoints Summary

| Method | Endpoint       | Description             | Response |
|--------|---------------|------------------------|----------|
| GET    | `/tasks`      | List all tasks         | 200 OK   |
| POST   | `/tasks`      | Create a new task      | 201 Created |
| GET    | `/tasks/{id}` | Get details of a task  | 200 OK   |

---

## Example Usage

### List Tasks (cURL)

curl -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"


### Create Task (Python)

import requests

resp = requests.post(
"https://api.taskly.app/v1/tasks",
headers={"Authorization": "Bearer DEMO_KEY"},
json={"title": "Buy groceries", "due": "2025-11-01"}
)
print(resp.status_code)
print(resp.json())


---

## Notes

- This is a demo spec; endpoints don't work against a live backend.
- Extend the API by editing the `openapi.yaml` file in your project.
- For troubleshooting or contributing, check the [Troubleshooting](../troubleshooting/) and [Contributing](../contributing/) pages.


