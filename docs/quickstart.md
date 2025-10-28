# Quickstart — Hello Taskly

Welcome! This guide helps you create your first task, list tasks, and mark one as complete, demonstrating how to interact with the Taskly API from different tools.

---

## Prerequisites
- **API Key:** Use `DEMO_KEY` (just for demo/testing).
- **Tools:**  
  - `curl` (installed by default on most Linux/Mac; on Windows, use PowerShell or Git Bash)  
  - Python 3.8+ with `requests` library (`pip install requests`)  
  - Web browser for dashboard or Swagger (optional)

---

## Step 1: Create a Task Using cURL

curl -X POST https://api.taskly.app/v1/tasks  
-H "Authorization: Bearer DEMO_KEY"  
-H "Content-Type: application/json"  
-d '{"title": "My first task", "due": "2025-11-01"}'  

**Expected Response:**

{  
"id": "task_12345",  
"title": "My first task",  
"due": "2025-11-01",  
"status": "pending"  
}  


**Tip:** Save `id` for later (e.g., use `task_12345`).

---

## Step 2: List All Tasks

curl -X GET https://api.taskly.app/v1/tasks  
-H "Authorization: Bearer DEMO_KEY"  

**Sample Output:**

[  
{  
"id": "task_12345",  
"title": "My first task",  
"due": "2025-11-01",  
"status": "pending"  
},  
{  
"id": "task_67890",  
"title": "Another task",  
"due": "2025-10-31",  
"status": "completed"  
}  
]  


---

## Step 3: Mark the Task as Complete

Replace `task_12345` with your task ID from step 1.

curl -X PATCH https://api.taskly.app/v1/tasks/task_12345  
-H "Authorization: Bearer DEMO_KEY"  
-H "Content-Type: application/json"  
-d '{"status": "completed"}'  

**Response:**

{  
"id": "task_12345",  
"title": "My first task",  
"due": "2025-11-01",  
"status": "completed"  
}  


---

## Step 4: Try the CLI (Optional)

Run these commands in your terminal:  

taskly create "Buy milk" --due 2025-11-01  
taskly list  
taskly complete task_XXXXX # replace "task_XXXXX" with actual ID from create  


**Note:** The CLI is imaginary here; it’s for illustration.

---

## Next Steps

- Explore further API features (update, delete).
- Practice with Python scripts.
- Integrate in your favorite tools or CI/CD pipelines.

---

## Troubleshooting

| Issue | Solution |
|--------|-----------|
| `curl: command not found` | Install or use PowerShell. |
| `404 Not Found` | Check URLs, file paths, or task ID. |
| `401 Unauthorized` | Use correct `DEMO_KEY`. |
| `python: command not found` | Reinstall Python or activate your virtual env. |

---

## Note
This demo backend is static; no real server handles these requests. Only for tutorial purposes.

Enjoy learning how APIs work!




