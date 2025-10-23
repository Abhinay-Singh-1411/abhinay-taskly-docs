# cURL Tutorial

Master the Taskly API using cURL commands in your terminal. This tutorial shows the complete workflow—create, list, and update tasks—using only command-line tools.

## Prerequisites

- **cURL**: Pre-installed on macOS, Linux. On Windows: download from [curl.se](https://curl.se/windows/) or use PowerShell's `Invoke-RestMethod`
- **Terminal/Command Prompt**: Any OS
- **jq** (optional but recommended): For pretty-printing JSON. Install: `brew install jq` (macOS), `sudo apt install jq` (Ubuntu), or download from [jqlang.github.io/jq](https://jqlang.github.io/jq/)

## Step 1: Create a Task

Send a POST request to create your first task. This command creates a task titled "Terminal Task".

### Basic cURL Command

curl -X POST https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{
"title": "Terminal Task",
"due": "2025-11-01"
}'


### With Pretty Output (using jq)

curl -s -X POST https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{
"title": "Terminal Task",
"due": "2025-11-01"
}' | jq '.'


**Expected Response:**

{
"id": "task_ghi456",
"title": "Terminal Task",
"due": "2025-11-01",
"status": "pending",
"created_at": "2025-10-23T14:30:00Z"
}


**Save the Task ID** (you'll need it for Step 3). From the response above, your `task_id` is `task_ghi456`.

**Pro Tip**: Save the response to a file for later use:

curl -s -X POST https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"title":"Terminal Task","due":"2025-11-01"}'
| jq -r '.id' > task_id.txt


Now `cat task_id.txt` shows just the ID.

---

## Step 2: List All Tasks

Retrieve all tasks in your account with a simple GET request.

### Basic List Command

curl -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"


### Pretty-Printed List (with jq)

curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
| jq '.'


### Filtered List (Show Only Titles and Status)

curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
| jq 'map({title: .title, status: .status, due: .due})'


**Expected Output:**
[
{
"title": "Terminal Task",
"status": "pending",
"due": "2025-11-01"
},
{
"title": "Demo Task",
"status": "completed",
"due": "2025-10-30"
}
]


### One-Line Summary

curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
| jq -r '.[] | "
.status)
.status).title) (due $$.due))"'


**Output:**
pending Terminal Task (due 2025-11-01)
completed Demo Task (due 2025-10-30)


---

## Step 3: Complete a Task

Update your task status from "pending" to "completed" using PATCH. Replace `TASK_ID` with the ID from Step 1.

### Basic Update Command

curl -X PATCH https://api.taskly.app/v1/tasks/TASK_ID
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"status": "completed"}'


### Using Your Saved Task ID

If you saved the ID to `task_id.txt`:

TASK_ID=$(cat task_id.txt)
curl -X PATCH "https://api.taskly.app/v1/tasks/$TASK_ID"
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"status": "completed"}' | jq '.'


**Expected Response:**

{
"id": "task_ghi456",
"title": "Terminal Task",
"status": "completed",
"due": "2025-11-01",
"updated_at": "2025-10-23T14:35:00Z"
}


---

## Complete Workflow (Script)

Create a bash script (`taskly_curl.sh`) for the full workflow:

#!/bin/bash

Taskly API cURL Workflow
BASE_URL="https://api.taskly.app/v1"
API_KEY="DEMO_KEY"

echo "🚀 Taskly cURL Tutorial"
echo "========================"

#### Step 1: Create task
echo -e "\n1️⃣ Creating task..."
TASK_DATA='{"title":"cURL Workflow Task","due":"2025-11-02"}'

RESPONSE=$(curl -s -w "\nHTTP_STATUS:%{http_code}"
-X POST "$BASE_URL/tasks"
-H "Authorization: Bearer $API_KEY"
-H "Content-Type: application/json"
-d "$TASK_DATA")

TASK_ID=$(echo "$RESPONSE" | jq -r '.id')
HTTP_CODE=$(echo "$RESPONSE" | grep "HTTP_STATUS" | cut -d: -f2-)

if [[ $HTTP_CODE == "201" ]]; then
echo "✅ Created: $(echo "$RESPONSE" | jq -r '.title') (ID: $TASK_ID)"
else
echo "❌ Create failed: $HTTP_CODE"
echo "$RESPONSE"
exit 1
fi

#### Step 2: List tasks
echo -e "\n2️⃣ Listing tasks..."
curl -s -X GET "$BASE_URL/tasks"
-H "Authorization: Bearer $API_KEY"
| jq -r '.[] | "
.status)
.status).title) (due $$.due))"'

#### Step 3: Complete task
echo -e "\n3️⃣ Completing task $TASK_ID..."
curl -s -X PATCH "$BASE_URL/tasks/$TASK_ID"
-H "Authorization: Bearer $API_KEY"
-H "Content-Type: application/json"
-d '{"status":"completed"}'
| jq -r '{id: .id, title: .title, status: .status}'

echo -e "\n🎉 Complete!"


Make it executable and run:

chmod +x taskly_curl.sh
./taskly_curl.sh


---

## Advanced cURL Features

### Pagination

List tasks with pagination:

curl -s "https://api.taskly.app/v1/tasks?page=1&limit=5"
-H "Authorization: Bearer DEMO_KEY" | jq '.'

Next page
curl -s "https://api.taskly.app/v1/tasks?page=2&limit=5"
-H "Authorization: Bearer DEMO_KEY" | jq '.'


### Save Response to File

#### Save tasks to JSON file
curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY" \

my_tasks.json

#### View with jq
cat my_tasks.json | jq '.'


### One-Liner Pipeline

Create, list, and complete in one command chain:

#### Create task and pipe to complete it
TASK_ID=$(curl -s -X POST https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"title":"Quick Task","due":"2025-11-01"}' | jq -r '.id')

echo "Created task: $TASK_ID"

#### Immediately complete it
curl -X PATCH "https://api.taskly.app/v1/tasks/$TASK_ID"
-H "Authorization: Bearer DEMO_KEY"
-H "Content-Type: application/json"
-d '{"status":"completed"}' | jq '.'


---

## Response Code Reference

| Status Code | Meaning | When It Occurs |
|-------------|---------|----------------|
| `200` | Success | GET requests, successful updates |
| `201` | Created | POST requests creating resources |
| `400` | Bad Request | Invalid JSON or missing required fields |
| `401` | Unauthorized | Missing or invalid API key |
| `404` | Not Found | Task ID doesn't exist |
| `429` | Rate Limited | Too many requests (not in demo) |

---

## jq Cheatsheet

`jq` makes JSON much more readable:

| Command | What It Does |
|---------|--------------|
| `jq '.'` | Pretty-print entire response |
| `jq '.id'` | Extract specific field |
| `jq -r '.id'` | Raw output (no quotes) |
| `jq '.[]'` | Array elements |
| `jq 'map({title, status})'` | Select specific fields from array |
| `jq 'length'` | Count array elements |

Install jq and try: `curl [URL] | jq --help`

---

## Troubleshooting

### Common cURL Issues

| Problem | Solution |
|---------|----------|
| `curl: command not found` | Install cURL or use PowerShell `Invoke-RestMethod` |
| `curl: (6) Could not resolve host` | Check internet connection, try `ping api.taskly.app` |
| `jq: command not found` | Install jq or remove `| jq` from commands |
| `401 Unauthorized` | Verify `Bearer DEMO_KEY` header format |
| **Windows line endings** | Use Git Bash or PowerShell, avoid Command Prompt |

### PowerShell Alternative (Windows)

If cURL isn't available, use PowerShell:

#### Create task
$headers = @{
'Authorization' = 'Bearer DEMO_KEY'
'Content-Type' = 'application/json'
}
$body = @{title='PowerShell Task'; due='2025-11-01'} | ConvertTo-Json

$response = Invoke-RestMethod -Uri 'https://api.taskly.app/v1/tasks' -Method Post -Headers $headers -Body $body
Write-Host "Created task ID: $($response.id)"


---

## Automation Ideas

### Bash Script for Daily Check

Create `check_tasks.sh`:

#!/bin/bash
echo "📊 Daily Task Summary"
echo "====================="

Count pending tasks
PENDING=$(curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY" | jq '[.[] | select(.status == "pending")] | length')

echo "⏳ Pending tasks: $PENDING"

Show due today
echo -e "\n📅 Tasks due today:"
curl -s -X GET https://api.taskly.app/v1/tasks
-H "Authorization: Bearer DEMO_KEY"
| jq -r '.[] | select(.due == "'$(date +%Y-%m-%d)'") | "
.title)(ID:.title)(ID:.id))"'


### Cron Job (Linux/macOS)

#### Add to crontab (crontab -e)
#### Run daily at 8 AM
0 8 * * * /path/to/check_tasks.sh >> /path/to/task_log.txt 2>&1


---

## Next Steps

- **Python Tutorial**: Programmatic integration with error handling
- **JavaScript Tutorial**: Browser and Node.js fetch examples
- **CLI Documentation**: Install and use Taskly command-line tool

---

!!! success "You've Mastered cURL!"
    You can now use the Taskly API from any terminal, automate workflows, and integrate with shell scripts.

!!! tip "Advanced Usage"
    - Use `-v` flag for verbose output: `curl -v -X GET ...`
    - Save authentication: Create `.curlrc` with `--header "Authorization: Bearer DEMO_KEY"`
    - Chain commands: `curl ... | jq '.id' | xargs -I {} curl -X PATCH .../{}`

