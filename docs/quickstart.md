# Quickstart — Hello Taskly

## Prerequisites
- You need an API key (for this demo use `DEMO_KEY` or set a placeholder)
- `curl` or Python 3.10+

## 1) Create a task (curl)
curl -X POST https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"title":"My first task","due":"2025-11-01"}'
Expected response: 201 Created with JSON of the new task.

## 2) Get tasks (Python example)
import requests
resp = requests.get('https://api.taskly.app/v1/tasks',
headers={'Authorization':'Bearer DEMO_KEY'})
print(resp.json())

## 3) Try the CLI
taskly create "Buy groceries" --due 2025-11-01

### Troubleshooting
- If curl fails, check your internet connection and ensure the URL is correct.
- Python errors? Ensure requests is installed: `pip install requests`.
- CLI not found? This is a demo—install would be `pip install taskly` (imaginary).
- Getting 404 errors? The API is fictional for this documentation demo.
