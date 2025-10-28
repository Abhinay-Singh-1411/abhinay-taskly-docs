# Python Tutorial

Learn how to interact with Taskly API using Python and the `requests` library.  
This tutorial walks through creating tasks, listing them, and marking a task complete.

---

## Goal

Create a task, list tasks, and mark one complete using Python.

---

## Prerequisites

- Python 3.10 or newer
- `requests` library: install with   
pip install requests  


---

## Step 1: Create a Task

Send a POST request to create a new task with a title and due date.  
import requests  
  
base_url = "https://api.taskly.app/v1"  
api_key = "DEMO_KEY"  
  
task_data = {  
"title": "Learn Python",  
"due": "2025-11-01"  
}    
  
response = requests.post(  
f"{base_url}/tasks",  
headers={  
"Authorization": f"Bearer {api_key}",  
"Content-Type": "application/json"  
},  
json=task_data  
)  

print(response.json()) # Shows the new task ID and details  


---

## Step 2: List Tasks

Retrieve all your tasks.
  
response = requests.get(  
f"{base_url}/tasks",  
headers={"Authorization": f"Bearer {api_key}"}  
)  
  
tasks = response.json()  
print(tasks) # List of tasks in JSON format  


---

## Step 3: Mark a Task Complete (Patch Example)

Replace `task_id` with the ID you received in Step 1.  

task_id = "your_task_id_here"  
  
response = requests.patch(  
f"{base_url}/tasks/{task_id}",  
headers={  
"Authorization": f"Bearer {api_key}",  
"Content-Type": "application/json"  
},  
json={"status": "completed"}  
)  
  
print(response.json()) # Updated task details  


---

## Expected Output

Sample JSON for created task:  

{  
"id": "123",  
"title": "Learn Python",  
"due": "2025-11-01",  
"status": "pending"  
}  


---

## Troubleshooting

- **ImportError:** Run `pip install requests`
- **401 Unauthorized:** Check you are using `DEMO_KEY`
- **JSON decode errors:** Verify API endpoint URL and headers

---

Explore more advanced covers in [JavaScript](tutorial-js.md) or [cURL](tutorial-curl.md).

