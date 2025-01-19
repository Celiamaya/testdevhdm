# README

## Project Overview

This project is a **To-Do List** application built using **React** for the front-end and **Node.js** with **MySQL** for the back-end. The application allows users to add, edit, and delete tasks, and provides a simple UI with real-time updates.

## Technologies Used

- **React** for front-end development.
- **Node.js** with **Express** for the back-end.
- **MySQL** for data storage.
- **Docker** to containerize the database.
- **React Toastify** for notifications.

## Features Implemented

1. **Task Management:**
   - Users can add new tasks.
   - Users can edit existing tasks.
   - Users can delete tasks.
   - Tasks are stored persistently in a MySQL database.

2. **User Notifications:**
   - Success and error notifications are displayed using **React Toastify** whenever an action (add, edit, delete) is performed.

3. **Persistent Data:**
   - Tasks are stored in the **MySQL database**, ensuring data persistence even after the user refreshes the page.
   - Tasks are also stored locally in the browser's `localStorage` for a better user experience (though database should be the primary source of truth).

## Challenges & Decisions

### 1. **State Management and Data Fetching:**
   - For managing the list of tasks, I used **React's `useState`** hook. This allows me to dynamically update the task list when new tasks are added, edited, or deleted.
   - I used **`useEffect`** to fetch tasks from the API when the component first mounts. This ensures that the latest tasks from the database are always displayed.

   **Breakpoints Encountered:**
   - Initially, I was struggling with handling the state after adding a task. I had to ensure that new tasks would both be displayed immediately on the front-end and be persisted in the database.
   - The solution involved updating the `tasks` state with the newly added task and also storing the updated list in the browser’s `localStorage`.

### 2. **Backend API (Express & MySQL):**
   - The back-end API is built with **Express.js** and uses a **MySQL database** to persist tasks.
   - I created basic CRUD (Create, Read, Update, Delete) routes to handle the task operations. For example:
     - `POST /tasks`: Adds a new task to the database.
     - `GET /tasks`: Retrieves all tasks from the database.
     - `PUT /tasks/:id`: Updates the task based on the task ID.
     - `DELETE /tasks/:id`: Deletes the task based on the task ID.

   **Breakpoints Encountered:**
   - Initially, I had some issues with CORS (Cross-Origin Resource Sharing) when trying to make requests from the React front-end to the Express back-end. This was resolved by setting appropriate headers in the Express server.
   - Ensuring the tasks were correctly fetched and updated from MySQL required debugging of SQL queries to ensure proper data insertion, updating, and deletion.

### 3. **LocalStorage and Persistence:**
   - I implemented a feature to use **localStorage** as a temporary storage solution in the browser. This allows tasks to persist even when the page is reloaded or the app is restarted, improving user experience.
   - The main challenge was ensuring that the localStorage and the backend were both kept in sync. When a task is added or deleted, both the state and the localStorage are updated simultaneously.

### 4. **User Authentication (Future Considerations):**
   - I initially planned to implement user authentication, but this was not a requirement for this test. However, I left comments in the code about where user authentication (such as JWT tokens) could be added in the future for session management.

## Code Breakdown

### Front-End (React):
- The front-end uses **React** to render the list of tasks and handle interactions such as adding, editing, and deleting tasks. The application is built using functional components with hooks (`useState`, `useEffect`).
- The `useFetch` custom hook abstracts the API calls, making it easy to handle various HTTP methods (`GET`, `POST`, `PUT`, `DELETE`).

#### Key Code Snippets:

```javascript
// Fetch tasks from the back-end API
const handleFetchTasks = async () => {
  const fetchedTasks = await api.get('/tasks');
  setTasks(fetchedTasks);
};

// Add a new task
const handleAddTask = async () => {
  const newTask = { name: newTaskName, createdAt: new Date().toISOString() };
  const addedTask = await api.post('/tasks', newTask);
  setTasks((prevTasks) => [...prevTasks, addedTask]);
};
