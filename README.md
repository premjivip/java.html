Objective: Create a fully functional To-Do List application using ES6 JavaScript. This project will help you understand and apply ES6 features, such as classes, modules, arrow functions, template literals, and the fetch API.

Requirements: User Interface:

A clean, intuitive user interface that allows users to add, edit, delete, and mark tasks as complete. Tasks should be displayed in a list format. Each task should have a title, description, due date, and a checkbox to mark it as complete. Features:

Add Task: Allow users to add a new task with a title, description, and due date. Edit Task: Allow users to edit the details of an existing task. Delete Task: Allow users to delete a task. Complete Task: Allow users to mark a task as complete or incomplete. Filter Tasks: Provide options to filter tasks by their completion status (e.g., all, completed, incomplete). Persist Data: Store the tasks in the browser's local storage so that the list persists across page reloads. Fetch Tasks: Optionally, allow users to fetch tasks from a provided API endpoint and display them in the list. Code Structure:

Use ES6 classes to create the task objects and manage the to-do list. Separate your code into modules to keep it organized and maintainable. Use ES6 arrow functions, template literals, and other modern syntax features. Styling:

Style the application using CSS to make it visually appealing. Ensure the application is responsive and works well on different screen sizes. Documentation:

Comment your code to explain the functionality of different parts. Write a README file to explain how to set up and run the project, as well as an overview of the application's features. Implementation Steps: Setup:

Create a new project directory and initialize it with npm (optional). Set up your project structure with separate folders for HTML, CSS, and JavaScript files. HTML:

Create the basic structure of the HTML file with a container for the to-do list and form elements for adding tasks. CSS:

Style the application to make it user-friendly and visually appealing. JavaScript:

Task Class: Create a class to represent a task with properties like title, description, due date, and completion status. ToDoList Class: Create a class to manage the list of tasks, including methods to add, edit, delete, and filter tasks. Local Storage: Implement methods to save and retrieve tasks from the local storage. Event Listeners: Add event listeners to handle user interactions, such as adding, editing, and deleting tasks. Fetch API: Optionally, add functionality to fetch tasks from an external API and populate the list.

## Program:
```c
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do List App</title>
  <link rel="stylesheet" href="java.css">
</head>
<body>
  <div class="container">
    <h1>To-Do List</h1>
    <form id="todoForm">
      <label>Title Of The Work:</label>
      <input type="text" id="titleInput" placeholder="Enter title..." required>
      <br>
      <label>description Of The Work:</label>
      <input type="text" id="descriptionInput" placeholder="Enter description..." required>
      <br>
      <label>Due Date:</label>
      <input type="date" id="dueDateInput" required>
      <br>
      <label>Click Here To Add Task:</label>
      <button type="submit">Add Task</button>
    </form>
    <div>
      <label>
        Show:
        <select id="filterSelect">
          <option value="all">All</option>
          <option value="completed">Completed</option>
          <option value="incomplete">Incomplete</option>
        </select>
      </label>
    </div>
    <ul id="todoList"></ul>
  </div>

  <script src="task.js"></script>
</body>
</html>
```
## java.css
```c
body {
    font-family: monospace;
    background-color: blue;
    margin:0;
    padding:0;
  }
  
  .container {
    max-width: 600px;
    margin: 50px auto;
    padding: 20px;
    background-color: rgb(194, 40, 47);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
  }
  
  form {
    display: flexbox;
    flex-direction: column;
    align-items: center;
  }
  
  form input, form textarea, form button {
    margin: 10px 0;
  }
  
  .filters {
    display: flex;
    justify-content: center;
    margin: 20px 0;
  }
  
  .filters button {
    margin: 0 5px;
    padding: 10px;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px;
    background-color: #1f29b9;
    margin: 10px 0;
    border-radius: 4px;
  }
  
  li.completed {
    text-decoration: line-through;
    color: rgb(114, 114, 114);
  }
  ```
  ## jask.js
  ```c
  
// Initialize tasks array
let tasks = [];

// Function to render tasks
const renderTasks = () => {
  const todoList = document.getElementById('todoList');
  const filter = document.getElementById('filterSelect').value;

  // Clear existing list
  todoList.innerHTML = '';

  tasks.forEach((task) => {
    if (filter === 'all' || (filter === 'completed' && task.completed) || (filter === 'incomplete' && !task.completed)) {
      const li = document.createElement('li');
      li.innerHTML = `
        <input type="checkbox" ${task.completed ? 'checked' : ''}>
        <span class="${task.completed ? 'completed' : ''}">${task.title} - ${task.description} (Due: ${task.dueDate})</span>
        <button class="delete-btn">Delete</button>
      `;
      li.querySelector('input').addEventListener('change', () => toggleTaskCompleted(task.id));
      li.querySelector('.delete-btn').addEventListener('click', () => deleteTask(task.id));
      todoList.appendChild(li);
    }
  });
};

// Function to add a new task
const addTask = (title, description, dueDate) => {
  const newTask = {
    id: Date.now(),
    title,
    description,
    dueDate,
    completed: false
  };
  tasks.push(newTask);
  saveTasks();
  renderTasks();
};

// Function to delete a task
const deleteTask = (id) => {
  tasks = tasks.filter(task => task.id !== id);
  saveTasks();
  renderTasks();
};

// Function to toggle task completion status
const toggleTaskCompleted = (id) => {
  tasks = tasks.map(task => {
    if (task.id === id) {
      return { ...task, completed: !task.completed };
    }
    return task;
  });
  saveTasks();
  renderTasks();
};

// Function to save tasks to local storage
const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks));
};

// Function to load tasks from local storage
const loadTasks = () => {
  const storedTasks = localStorage.getItem('tasks');
  tasks = storedTasks ? JSON.parse(storedTasks) : [];
  renderTasks();
};

// Event listener for form submission
document.getElementById('todoForm').addEventListener('submit', (event) => {
  event.preventDefault();
  const title = document.getElementById('titleInput').value.trim();
  const description = document.getElementById('descriptionInput').value.trim();
  const dueDate = document.getElementById('dueDateInput').value;
  if (title && description && dueDate) {
    addTask(title, description, dueDate);
    document.getElementById('titleInput').value = '';
    document.getElementById('descriptionInput').value = '';
    document.getElementById('dueDateInput').value = '';
  } else {
    alert('Please fill in all fields.');
  }
});

// Event listener for filter select change
document.getElementById('filterSelect').addEventListener('change', renderTasks);

// Initial load of tasks from local storage
loadTasks();
```
## OUTPUT:
![alt text](<Screenshot (86).png>)
