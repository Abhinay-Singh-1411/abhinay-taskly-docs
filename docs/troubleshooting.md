# Troubleshooting

Stuck? This page will help you fix common issues quickly when working with the Taskly API or CLI.

---

## Common Errors & Solutions

### 401 Unauthorized

**Problem:** Invalid API key or key missing.  
**Solution:** Use `DEMO_KEY` for testing, or check your environment variable.

**How to Test:**
curl -H "Authorization: Bearer DEMO_KEY" https://api.taskly.app/v1/tasks <br>
If it works, your key setup is good!

---

### 400 Bad Request

**Problem:** Required fields missing or wrong format in your request body.  
**Solution:** Always include at least `"title": "Task name"` in your JSON.

**Example Fix:**
{
"title": "Buy groceries",
"due": "2025-11-01"
}

---

### 429 Too Many Requests

**Problem:** You've hit the rate limit.  
**Solution:** Wait one minute, then retry. (Not enforced for demo.)

---

### CLI tool not found

**Problem:** Command `taskly` not recognized.  
**Solution:** Install the CLI with pip:
pip install taskly<br>
Then check install:  
taskly --version  

---

## General Tips

- **Check connectivity:**  
  Test with: `ping api.taskly.app`
- **Enable debug logs:**  
  Use the `--verbose` flag for more detailed output in CLI.

---

## Example Error Messages

| Error             | When It Appears     | Quick Fix                        |
|-------------------|---------------------|----------------------------------|
| `401 Unauthorized`| Wrong/missing key   | Use correct API key (demo: `DEMO_KEY`) |
| `400 Bad Request` | Bad/missing JSON    | Add all required fields          |
| `429 Too Many Requests` | Too many tries | Wait and retry (demo: not enforced) |
| `command not found` | CLI not installed  | Run `pip install taskly`         |

---

!!! info "Still stuck?"
    Double-check your command for typos, and see the [Quickstart](/quickstart/) or [API Reference](/api/) for more code examples.
    If you still need help, create a GitHub issue or ask in the community!

