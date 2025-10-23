# Python Tutorial

Get hands-on with the Taskly API using Python and the `requests` library. In this tutorial, you'll create a task, list your tasks, and mark one as complete.

## Prerequisites

Before starting, ensure you have:

- Python 3.8+ installed
- The `requests` library: `pip install requests` (run this in your terminal or virtual environment)

## Step 1: Create Your First Task

Send a POST request to create a new task. This example creates a task titled "Learn Python" with a due date.

import requests

#### API endpoint and demo key
base_url = "https://api.taskly.app/v1"
api_key = "DEMO_KEY"

#### Create task data
task_data = {
"title": "Learn Python",
"due": "2025-11-01"
}

#### Send POST request
response = requests.post(
f"{base_url}/tasks",
headers={
"Authorization": f"Bearer {api_key}",
"Content-Type": "application/json"
},
json=task_data
)

#### Check response
if response.status_code == 201:
task = response.json()
print(f"✅ Task created successfully!")
print(f" ID: {task['id']}")
print(f" Title: {task['title']}")
print(f" Due: {task['due']}")
task_id = task['id'] # Save for next steps
else:
print(f"❌ Error: {response.status_code}")
print(f" {response.text}")


**Expected Output:**

✅ Task created successfully!
ID: task_abc123
Title: Learn Python
Due: 2025-11-01


Save this code as `create_task.py` and run with `python create_task.py`.

---

## Step 2: List All Tasks

Retrieve all your tasks with a GET request. This shows both your new task and any demo data.

import requests

#### API configuration (reuse from Step 1)
base_url = "https://api.taskly.app/v1"
api_key = "DEMO_KEY"

#### Get all tasks
response = requests.get(
f"{base_url}/tasks",
headers={"Authorization": f"Bearer {api_key}"}
)

if response.status_code == 200:
tasks = response.json()
print("📋 Your tasks:")
for task in tasks:
status_emoji = "✅" if task.get("status") == "completed" else "⏳"
print(f" {status_emoji} {task['id'][:8]}: {task['title']} (Due: {task['due']})")
else:
print(f"❌ Error: {response.status_code}")
print(f" {response.text}")


**Expected Output:**
📋 Your tasks:
⏳ task_abc12: Learn Python (Due: 2025-11-01)
⏳ task_def45: Welcome Task (Due: 2025-10-30)


---

## Step 3: Mark Task Complete

Update your task status from "pending" to "completed" using PATCH.

import requests

#### API configuration
base_url = "https://api.taskly.app/v1"
api_key = "DEMO_KEY"
task_id = "task_abc123" # Replace with your actual task ID from Step 1

#### Update task status
response = requests.patch(
f"{base_url}/tasks/{task_id}",
headers={
"Authorization": f"Bearer {api_key}",
"Content-Type": "application/json"
},
json={"status": "completed"}
)

if response.status_code == 200:
updated_task = response.json()
print(f"🎉 Task {task_id} marked complete!")
print(f" New status: {updated_task['status']}")
print(f" Title: {updated_task['title']}")
else:
print(f"❌ Error: {response.status_code}")
print(f" {response.text}")


**Expected Output:**
🎉 Task task_abc123 marked complete!
New status: completed
Title: Learn Python


---

## Complete Workflow Script

Combine all steps into a single script (`taskly_workflow.py`):

import requests
import json

def main():
base_url = "https://api.taskly.app/v1"
api_key = "DEMO_KEY"

print("🚀 Taskly API Python Tutorial")
print("=" * 40)

# Step 1: Create task
print("\n1️⃣ Creating task...")
task_data = {"title": "Learn Python", "due": "2025-11-01"}
create_resp = requests.post(
    f"{base_url}/tasks",
    headers={
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    },
    json=task_data
)

if create_resp.status_code == 201:
    task = create_resp.json()
    task_id = task['id']
    print(f"   ✅ Created: {task['title']} (ID: {task_id})")
else:
    print(f"   ❌ Create failed: {create_resp.status_code}")
    return

# Step 2: List tasks
print("\n2️⃣ Listing tasks...")
list_resp = requests.get(
    f"{base_url}/tasks",
    headers={"Authorization": f"Bearer {api_key}"}
)

if list_resp.status_code == 200:
    tasks = list_resp.json()
    print(f"   📋 Found {len(tasks)} tasks:")
    for t in tasks:
        status = "✅" if t['status'] == "completed" else "⏳"
        print(f"      {status} {t['title']}")
else:
    print(f"   ❌ List failed: {list_resp.status_code}")

# Step 3: Complete task
print(f"\n3️⃣ Completing task {task_id}...")
complete_resp = requests.patch(
    f"{base_url}/tasks/{task_id}",
    headers={
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json"
    },
    json={"status": "completed"}
)

if complete_resp.status_code == 200:
    print("   🎉 Task completed successfully!")
else:
    print(f"   ❌ Complete failed: {complete_resp.status_code}")


if name == "main":
main()


Run with `python taskly_workflow.py` to see the complete workflow.

---

## Error Handling

Common issues and solutions:

| Error | Cause | Solution |
|-------|-------|----------|
| `ImportError: No module named 'requests'` | Missing library | Run `pip install requests` |
| `401 Unauthorized` | Invalid API key | Use `DEMO_KEY` in headers |
| `ConnectionError` | Network issue | Check internet connection |
| `404 Not Found` | Wrong endpoint | Use `/v1/tasks` path |

---

## Next Steps

- **JavaScript Tutorial:** See how to use `fetch()` in browser or Node.js
- **cURL Tutorial:** Command-line examples for terminal workflows
- **CLI Documentation:** Install and use the Taskly command-line tool

## Troubleshooting

- **Silent failures?** Add `print(response.status_code)` and `print(response.text)` after each request
- **JSON errors?** Use `response.json()` only after checking `response.status_code == 200`
- **Rate limits?** This demo has no limits, but production APIs do

---

!!! tip "Pro Tips"
    - Always check `response.status_code` before processing JSON
    - Use virtual environments for project dependencies: `python -m venv taskly_env`
    - Store API keys in environment variables, not hardcoded
    - Test endpoints in [Swagger Editor](https://editor.swagger.io/) with your OpenAPI spec
