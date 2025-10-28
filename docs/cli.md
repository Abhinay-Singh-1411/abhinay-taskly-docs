# Taskly CLI Documentation

Welcome to the Command-Line Interface (CLI) docs for Taskly.  
This guide covers installation, key commands, usage examples, options, and helpful tips to get you started—whether you’re new to CLIs or want quick copy-paste help.

---

## Installation

*This CLI is for demonstration only.*

- Install via pip _(imaginary)_:
pip install taskly
- Or, download the binary for your OS (see [GitHub Releases](https://github.com/YOUR_USERNAME/abhinay-taskly-docs/releases)).

---

## Quick Start

-Create and manage tasks directly from your terminal!

Create a new task
 taskly create "Buy groceries" --due 2025-11-01<br>

List all tasks
 taskly list<br>

Complete a task
 taskly complete TASK_ID<br>

Get help info
 taskly --help<br>


---

## Command Reference

| Command                                | Description               |
|:----------------------------------------|:--------------------------|
| `taskly create "Title"` <br> `--due DATE`        | Create a new task (set title, optionally due date) |
| `taskly list`                          | List all tasks            |
| `taskly complete ID`                   | Mark a specific task as done |
| `taskly --help`                        | Show help and usage info  |

---

## Usage Examples

### 1. Create a Task

taskly create "Call client" --due 2025-11-06  <br>

_Output:_
Task created with ID: task_001  

### 2. List All Tasks

taskly list  
_Output:_  
task_001: Call client (Due: 2025-11-06) [pending]  <br>
task_002: Buy groceries (Due: 2025-11-01) [completed]<br>


### 3. Mark Task Complete

taskly complete task_001  <br>
_Output:_  
Task task_001 marked as completed.  

### 4. See CLI Help Output

taskly --help  <br>
_Output:_<br>
Usage: taskly [OPTIONS] COMMAND [ARGS]...

Commands:
create Create a new task
list List all tasks
complete Mark a task as done

Global Flags:
--api-key KEY Override API key
--verbose Show more detailed output

Other:
--version Show CLI version
--help Show this message and exit


---

## Global Flags

- `--api-key KEY` — Use a specific API key for this run (overrides default)
- `--verbose` — Print extra debug/logging output

---

## Exit Codes

| Code | Meaning  |
|:-----|:---------|
| 0    | Success  |
| 1    | Error    |

---

## Shell Completion

For easier typing, enable Bash completion by adding this line to your `.bashrc`:<br>

eval "$(taskly completion bash)"<br>

_Open a new terminal after saving to use completion._

---

## Troubleshooting

| Issue/Message    | Solution                       |
|------------------|-------------------------------|
| `taskly: command not found` | Confirm install; check `$PATH`           |
| Error about API key       | Use `--api-key` or set the default          |
| Unexpected behavior       | Try `--verbose` to see more details         |
| Need more info            | Run `taskly --help`                         |

---

!!! tip "Best Practices"
    - Use quotes around multi-word task titles.
    - Use descriptive task names (“Call client”, not just “Call”).
    - Use shell completion to speed up workflow!

---

