# Ex03 To-Do List using JavaScript
## Date:

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```
todo.html


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>To-do list</title>
  <link rel="stylesheet" href="todo.css">
</head>
<body>
  <main class="board">
    <h1>Today's tasks</h1>
    <p id="count" class="count">No tasks yet</p>

    <div class="add">
      <input id="taskInput" type="text" placeholder="Add a task" aria-label="New task">
      <button id="addBtn" type="button">Add task</button>
    </div>

    <ul id="taskList" class="tasks"></ul>

    <div class="filters" role="group" aria-label="Filter tasks">
      <button type="button" data-filter="all" class="active">All</button>
      <button type="button" data-filter="open">Open</button>
      <button type="button" data-filter="done">Done</button>
    </div>
  </main>

  <script>
    const input = document.getElementById("taskInput");
    const list = document.getElementById("taskList");
    const count = document.getElementById("count");
    let tasks = [];
    let filter = "all";

    function render() {
      list.innerHTML = "";
      const shown = tasks.filter(t =>
        filter === "all" || (filter === "done" ? t.done : !t.done));

      if (shown.length === 0) {
        const empty = document.createElement("li");
        empty.className = "empty";
        empty.textContent = "Nothing here. Type a task above and press Enter.";
        list.appendChild(empty);
      }

      shown.forEach(t => {
        const li = document.createElement("li");
        li.className = "task" + (t.done ? " done" : "");

        const box = document.createElement("input");
        box.type = "checkbox";
        box.checked = t.done;
        box.setAttribute("aria-label", "Mark done: " + t.text);
        box.onchange = () => { t.done = box.checked; render(); };

        const label = document.createElement("span");
        label.textContent = t.text;

        const del = document.createElement("button");
        del.type = "button";
        del.className = "delete";
        del.textContent = "Delete";
        del.onclick = () => { tasks = tasks.filter(x => x !== t); render(); };

        li.append(box, label, del);
        list.appendChild(li);
      });

      const left = tasks.filter(t => !t.done).length;
      count.textContent = tasks.length === 0
        ? "No tasks yet"
        : left + (left === 1 ? " task" : " tasks") + " left";
    }

    function addTask() {
      const text = input.value.trim();
      if (!text) return;
      tasks.push({ text, done: false });
      input.value = "";
      render();
    }

    document.getElementById("addBtn").onclick = addTask;
    input.addEventListener("keydown", e => { if (e.key === "Enter") addTask(); });

    document.querySelectorAll(".filters button").forEach(b => {
      b.onclick = () => {
        filter = b.dataset.filter;
        document.querySelectorAll(".filters button")
          .forEach(x => x.classList.toggle("active", x === b));
        render();
      };
    });

    render();
  </script>
</body>
</html>
```


```
todo.css


:root {
  --paper: #f6f7f2;
  --card: #ffffff;
  --ink: #1f2a2e;
  --muted: #6b787c;
  --line: #dde2dc;
  --accent: #1f6f5c;
  --accent-dark: #17564a;
  --danger: #b3412f;
}

* { box-sizing: border-box; }

body {
  margin: 0;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: 48px 16px;
  background: var(--paper);
  color: var(--ink);
  font-family: "Segoe UI", system-ui, -apple-system, Arial, sans-serif;
}

.board {
  width: 100%;
  max-width: 520px;
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: 14px;
  padding: 28px;
}

h1 {
  margin: 0;
  font-size: 1.75rem;
  font-weight: 700;
  letter-spacing: -0.01em;
}

.count {
  margin: 4px 0 20px;
  color: var(--muted);
  font-size: 0.95rem;
}

.add {
  display: flex;
  gap: 8px;
}

.add input {
  flex: 1;
  min-width: 0;
  padding: 12px 14px;
  font: inherit;
  border: 1px solid var(--line);
  border-radius: 8px;
  background: var(--paper);
}

button {
  font: inherit;
  cursor: pointer;
}

.add button {
  padding: 12px 18px;
  border: 0;
  border-radius: 8px;
  background: var(--accent);
  color: #fff;
  font-weight: 600;
}

.add button:hover { background: var(--accent-dark); }

input:focus-visible,
button:focus-visible {
  outline: 3px solid #8cc7b8;
  outline-offset: 2px;
}

.tasks {
  list-style: none;
  margin: 20px 0 0;
  padding: 0;
}

.task {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 0;
  border-bottom: 1px solid var(--line);
}

.task input[type="checkbox"] {
  width: 20px;
  height: 20px;
  accent-color: var(--accent);
  cursor: pointer;
}

.task span {
  flex: 1;
  overflow-wrap: anywhere;
}

.task.done span {
  color: var(--muted);
  text-decoration: line-through;
}

.delete {
  border: 0;
  background: none;
  color: var(--danger);
  font-size: 0.9rem;
  padding: 4px 6px;
  border-radius: 6px;
}

.delete:hover { background: #f8e6e2; }

.empty {
  padding: 24px 0;
  color: var(--muted);
  text-align: center;
}

.filters {
  display: flex;
  gap: 8px;
  margin-top: 20px;
}

.filters button {
  padding: 6px 14px;
  border: 1px solid var(--line);
  border-radius: 999px;
  background: var(--card);
  color: var(--ink);
}

.filters button.active {
  background: var(--ink);
  border-color: var(--ink);
  color: #fff;
}

@media (max-width: 420px) {
  .board { padding: 20px; }
  .add { flex-direction: column; }
}
```
## OUTPUT
![alt text](<Screenshot 2026-11-17 084255.png>)

## RESULT
The program for creating To-do list using JavaScript is executed successfully.