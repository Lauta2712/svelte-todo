<script>
    import { tasks } from '../store';
    let newTask = ''; 

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

<div class="task-bar">
    <input type="text" bind:value={newTask} on:keypress={(e) => e.key === 'Enter' && addTask()} />
    <button on:click={addTask}>➕</button>
    <button class="delete-all-button" on:click={deleteAllTasks}>🗑</button>
</div>

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
  
  .delete-all-button:hover{
    background-color: #c0392b;
  }
</style>