# Quickstart - Hello Taskly

Get started with Taskly API in just a few minutes. This guide walks you through creating your first task.

## Prerequisites

Before starting, ensure you have:

- **API Key**: Use `DEMO_KEY` for this demo (or get one from [Taskly dashboard](https://api.taskly.app))
- **Tools**: cURL or Python 3.8+

## Step 1: Create a Task (cURL)

Use cURL to send your first POST request. This creates a task titled "My First Task".

```bash
curl -X POST https://api.taskly.app/v1/tasks \
  -H "Authorization: Bearer DEMO_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "My First Task",
    "due": "2025-11-01"
  }'

Expected Response: HTTP 201 with JSON of the new task.

Step 2: Get Tasks (Python Example)
Fetch all tasks using Python's requests library.

import requests

response = requests.get(
    "https://api.taskly.app/v1/tasks",
    headers={"Authorization": "Bearer DEMO_KEY"}
)
print(response.json())

Expected Output:

[
  {
    "id": "task_123",
    "title": "My First Task",
    "due": "2025-11-01",
    "status": "pending"
  }
]

Step 3: Use the CLI
For command-line workflow, install and try the Taskly CLI:

# Install (in your venv)
pip install taskly-cli

# Create task
taskly create "Buy groceries" --due 2025-11-01

# List tasks
taskly list

!!! success "All Set!"
You've successfully created and retrieved tasks! Next, explore the tutorials section for language-specific guides.

Troubleshooting
401 Unauthorized: Check your DEMO_KEY in headers

Connection errors: Verify the base URL https://api.taskly.app/v1

More help: Troubleshooting page

- **Why Better?** Each code block uses language tags (``````python, ```

