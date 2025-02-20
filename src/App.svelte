<script>
  import { tasks } from './store.js';
  import Task from './lib/Task.svelte';

  let draggedTaskId = null;
  let newTask = ''; 

  function onDragStart(id) {
    draggedTaskId = id;
  }

  function onDragOver(event) {
    event.preventDefault();
  }

  function onDrop(targetId) {
    tasks.update(t => {
      const draggedIndex = t.findIndex(task => task.id === draggedTaskId);
      const targetIndex = t.findIndex(task => task.id === targetId);

      if (draggedIndex !== -1 && targetIndex !== -1 && draggedIndex !== targetIndex) {
        const updatedTasks = [...t];
        const [draggedItem] = updatedTasks.splice(draggedIndex, 1);
        updatedTasks.splice(targetIndex, 0, draggedItem);
        return updatedTasks;
      }
      return t;
    });

    draggedTaskId = null;
  }

  function addTask() {
    if (newTask.trim()) {
      tasks.update(t => [...t, { id: Date.now(), text: newTask, completed: false }]);
      newTask = ''; 
    }
  }

  function deleteAllTasks() {
    tasks.set([]);
  }
</script>

<main>
  <h1>Daily</h1>
  <div class="task-bar">
    <input type="text" bind:value={newTask} on:keypress={(e) => e.key === 'Enter' && addTask()} />
    <button on:click={addTask}>Add</button>
    <button on:click={deleteAllTasks}>Delete All</button>
  </div>

  <div class="board" on:dragover={onDragOver} on:drop={() => onDrop(null)}>
    {#each $tasks as task (task.id)}
      <Task {task} onDragStart={onDragStart} onDragOver={onDragOver} onDrop={onDrop} />
    {/each}
  </div>
</main>

<style>
  .task-bar {
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    margin-bottom: 1rem;
  }

  .task-bar input, .task-bar button {
    padding: 0.75rem 1rem;
    font-size: 1rem;
    border-radius: 8px;
    border: none;
    background-color: #444;
    color: white;
    transition: background-color 0.3s ease;
  }

  .task-bar input:focus, .task-bar button:hover {
    background-color: #555;
  }

  .board {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); 
    grid-gap: 1rem; 
    width: 90vw;
    height: 70vh; 
    margin: 0 auto;
    background: #333;
    border-radius: 12px;
    box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
    padding: 1rem;
    overflow-y: auto;
  }
</style>