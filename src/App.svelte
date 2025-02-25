<script>
  import { tasks } from './store.js';
  import Task from './lib/Task.svelte';
  import TaskBar from './lib/TaskBar.svelte';

  let draggedTaskId = null;

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
</script>

<main>
  <h1>Daily</h1>

  <TaskBar/>

  <div class="board" on:dragover={onDragOver} on:drop={() => onDrop(null)}>
    {#each $tasks as task (task.id)}
      <Task {task} onDragStart={onDragStart} onDragOver={onDragOver} onDrop={onDrop} />
    {/each}
  </div>
</main>

<style>
  .board {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); 
    grid-gap: 1rem; 
    width: 90vw;
    height: 70vh; 
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 2rem;
    background: #333;
    border-radius: 12px;
    box-shadow: 2px 2px 10px #555;
    padding: 1rem;
    overflow-y: auto;
  }
</style>