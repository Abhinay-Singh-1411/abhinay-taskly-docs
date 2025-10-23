# JavaScript Tutorial

Learn to integrate Taskly API in both browser and Node.js environments using the native `fetch` API or libraries like Axios. This tutorial covers the same workflow: create, list, and update tasks.

## Prerequisites

### For Browser
- Modern browser (Chrome 43+, Firefox 39+, Safari 10.1+, Edge 14+)
- Enable CORS in browser or use a CORS extension for development

### For Node.js
- Node.js 14+ (fetch is built-in, no extra install needed)
- Or install Axios: `npm install axios`

## Step 1: Create a Task (Browser/Node.js)

### Using Native fetch()

// API configuration
const baseUrl = 'https://api.taskly.app/v1';
const apiKey = 'DEMO_KEY';

// Task data
const taskData = {
title: 'Learn JavaScript',
due: '2025-11-01'
};
// Create task
fetch(${baseUrl}/tasks, {
method: 'POST',
headers: {
'Authorization': Bearer ${apiKey},
'Content-Type': 'application/json'
},
body: JSON.stringify(taskData)
})
.then(response => {
if (!response.ok) {
throw new Error(HTTP ${response.status}: ${response.statusText});
}
return response.json();
})
.then(task => {
console.log('✅ Task created!');
console.log('ID:', task.id);
console.log('Title:', task.title);
console.log('Full response:', task);
return task.id; // Return ID for next steps
})
.catch(error => {
console.error('❌ Create task failed:', error);
});


### Using Axios (Node.js)

const axios = require('axios');

const baseUrl = 'https://api.taskly.app/v1';
const apiKey = 'DEMO_KEY';

async function createTask() {
try {
const response = await axios.post(${baseUrl}/tasks, {
title: 'Learn JavaScript',
due: '2025-11-01'
}, {
headers: {
'Authorization': Bearer ${apiKey},
'Content-Type': 'application/json'
}
});

    const task = response.data;
    console.log('✅ Task created!');
    console.log('ID:', task.id);
    console.log('Title:', task.title);
    return task.id;
} catch (error) {
    console.error('❌ Create task failed:', error.response?.status, error.message);
}

}

// Run the function
createTask();


**Expected Output:**

✅ Task created!
ID: task_xyz789
Title: Learn JavaScript


---

## Step 2: List Tasks

### Native fetch()

const baseUrl = 'https://api.taskly.app/v1';
const apiKey = 'DEMO_KEY';

fetch(${baseUrl}/tasks, {
headers: {
'Authorization': Bearer ${apiKey}
}
})
.then(response => {
if (!response.ok) {
throw new Error(HTTP ${response.status});
}
return response.json();
})
.then(tasks => {
console.log('📋 All tasks:');
tasks.forEach(task => {
const status = task.status === 'completed' ? '✅' : '⏳';
console.log(${status} ${task.title} (Due: ${task.due}));
});
})
.catch(error => console.error('❌ List failed:', error));


### Axios Version

async function listTasks() {
try {
const response = await axios.get(${baseUrl}/tasks, {
headers: {'Authorization': Bearer ${apiKey}}
});

    console.log(`📋 Found ${response.data.length} tasks:`);
    response.data.forEach(task => {
        const status = task.status === 'completed' ? '✅' : '⏳';
        console.log(`${status} ${task.title} (Due: ${task.due})`);
    });
} catch (error) {
    console.error('❌ List failed:', error.response?.status);
}

}

listTasks();


**Expected Output:**
📋 Found 2 tasks:
⏳ Learn JavaScript (Due: 2025-11-01)
⏳ Demo Task (Due: 2025-10-31)


---

## Step 3: Update Task Status

Mark your task as complete using PATCH.

// Using the task ID from Step 1
const taskId = 'task_xyz789'; // Replace with your actual ID
const baseUrl = 'https://api.taskly.app/v1';
const apiKey = 'DEMO_KEY';

fetch(${baseUrl}/tasks/${taskId}, {
method: 'PATCH',
headers: {
'Authorization': Bearer ${apiKey},
'Content-Type': 'application/json'
},
body: JSON.stringify({status: 'completed'})
})
.then(response => {
if (response.ok) {
return response.json();
}
throw new Error(Update failed: ${response.status});
})
.then(updatedTask => {
console.log('🎉 Task completed!');
console.log('Updated status:', updatedTask.status);
})
.catch(error => console.error('❌ Update failed:', error));


---

## Complete Workflow (Async/Await)

For Node.js or modern browsers, here's a complete async workflow:

async function tasklyWorkflow() {
const baseUrl = 'https://api.taskly.app/v1';
const apiKey = 'DEMO_KEY';

console.log('🚀 Taskly JavaScript Tutorial');
console.log('=' .repeat(40));

try {
    // 1. Create task
    console.log('\n1️⃣ Creating task...');
    const createResponse = await fetch(`${baseUrl}/tasks`, {
        method: 'POST',
        headers: {
            'Authorization': `Bearer ${apiKey}`,
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            title: 'JavaScript Integration',
            due: '2025-11-02'
        })
    });
    
    if (!createResponse.ok) throw new Error(`Create failed: ${createResponse.status}`);
    const createdTask = await createResponse.json();
    const taskId = createdTask.id;
    console.log(`   ✅ Created task ${taskId}: ${createdTask.title}`);
    
    // 2. List tasks
    console.log('\n2️⃣ Listing tasks...');
    const listResponse = await fetch(`${baseUrl}/tasks`, {
        headers: {'Authorization': `Bearer ${apiKey}`}
    });
    
    if (listResponse.ok) {
        const tasks = await listResponse.json();
        console.log(`   📋 Found ${tasks.length} tasks:`);
        tasks.slice(0, 3).forEach(task => {
            console.log(`      ⏳ ${task.title} (ID: ${task.id})`);
        });
    }
    
    // 3. Complete task
    console.log(`\n3️⃣ Completing task ${taskId}...`);
    const updateResponse = await fetch(`${baseUrl}/tasks/${taskId}`, {
        method: 'PATCH',
        headers: {
            'Authorization': `Bearer ${apiKey}`,
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({status: 'completed'})
    });
    
    if (updateResponse.ok) {
        const updatedTask = await updateResponse.json();
        console.log('   🎉 Task completed successfully!');
    } else {
        console.log(`   ❌ Complete failed: ${updateResponse.status}`);
    }
    
} catch (error) {
    console.error('❌ Workflow error:', error.message);
}


}

// Run the workflow
tasklyWorkflow();


---

## Browser vs Node.js Differences

| Feature | Browser | Node.js |
|---------|---------|---------|
| `fetch()` | Built-in | Built-in (Node 18+) |
| CORS | May need extension | No CORS issues |
| Environment variables | `process.env` not available | Use `process.env` |
| Package manager | N/A | `npm install` |

---

## Troubleshooting

### Common Issues

- **CORS Errors (Browser)**: Install a CORS extension or use Node.js for development
- **"fetch is not defined" (Older Node)**: Install Axios (`npm install axios`) or upgrade Node.js
- **401 Unauthorized**: Ensure `Bearer DEMO_KEY` header is correct
- **JSON Parse Error**: Check `response.ok` before calling `.json()`

### Debug Tips

Add error handling to see what's happening:

fetch(url, options)
.then(response => {
console.log('Status:', response.status);
console.log('Headers:', [...response.headers.entries()]);
if (!response.ok) {
return response.text().then(text => {
throw new Error(HTTP ${response.status}: ${text});
});
}
return response.json();
})


---

## Next Steps

- **Python Tutorial**: See the same workflow using `requests` library
- **cURL Tutorial**: Command-line examples for terminal automation
- **CLI Documentation**: Use Taskly's command-line interface

---

!!! info "Production Ready"
    In production, store API keys in environment variables:
    ```
    const apiKey = process.env.TASKLY_API_KEY || 'DEMO_KEY';
    ```

!!! warning "Rate Limits"
    This demo has no limits, but production APIs typically limit requests per minute.


