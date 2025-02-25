<script>
    export let task;
    export let onDragStart;
    export let onDragOver;
    export let onDrop;
  
    import { tasks } from "../store.js";
    import { fade } from 'svelte/transition';
    import axios from 'axios';
  
    let isDragging = false;
  
    // async function classifyTask(taskText) {
    //   try {
    //     const response = await axios.post('http://localhost:5000/classify', { taskText });
    //     tasks.update(t => 
    //       t.map(item => item.id === task.id ? { ...item, category: response.data.category } : item)
    //     );
    //   } catch (error) {
    //     console.error("Error en la API:", error);
    //     tasks.update(t => 
    //       t.map(item => item.id === task.id ? { ...item, category: "Desconocido" } : item)
    //     );
    //   }
    // }
  
    // if (!task.category) {
    //   classifyTask(task.text);  
    // }
  
    function toggleComplete() {
      tasks.update(t => 
        t.map(item => item.id === task.id ? { ...item, completed: !item.completed } : item)
      );
    }
  
    function deleteTask() {
      tasks.update(t => t.filter(item => item.id !== task.id));
    }
  </script>
  
  <div class="task-card"
      draggable="true"
      on:dragstart={() => { isDragging = true; onDragStart(task.id); }}
      on:dragend={() => isDragging = false}
      on:dragover={onDragOver}
      on:drop={() => onDrop(task.id)}
      on:click={toggleComplete}
      class:completed={task.completed}
      transition:fade
  >
      <p>{task.text} - <strong>{task.category}</strong></p>
      <button class="delete-button" on:click={deleteTask}>X</button>
  </div>
  
  <!-- <style>
      .task-card {
          display: flex;
          flex-direction: column; 
          justify-content: center; 
          align-items: center;
          position: relative;
          border-radius: 12px;
          background: #222;
          color: white;
          box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
          cursor: grab;
          font-size: 0.875rem;
          width: 10rem;
          height: 10rem;
          transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
          aspect-ratio: 1; 
      }

      .task-card p{
        margin: 10%;
      }
  
      .task-card:active {
          cursor: grabbing;
      }
  
      .task-card.completed {
          text-decoration: line-through;
          background: #555;
          color: #bbb;
          transform: scale(0.95);
          transition: transform 0.2s ease-in-out;
      }
  
      .task-card:hover {
          box-shadow: 2px 4px 20px rgba(0, 0, 0, 0.2);
          transform: scale(1.05);
      }
  
      .task-card:focus {
          outline: 2px solid #888;
      }
  
      .task-card[draggable=true] {
          opacity: 1;
          transition: opacity 0.2s ease-in-out;
      }
  
      .task-card[draggable=true]:active {
          opacity: 0.5;
      }
  
      .delete-button {
          position: absolute;
          top: 0.5rem;
          right: 0.5rem;
          background: #e74c3c;
          color: white;
          border: none;
          border-radius: 8px;
          padding: 0.25rem 0.5rem;
          cursor: pointer;
          font-size: 0.875rem;
          transition: background-color 0.3s ease;
      }
  
      .delete-button:hover {
          background: #c0392b;
      }
  </style> -->