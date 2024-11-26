<template>
  <div class="todo-app">
    <h1>To-Do List App</h1>

    <!-- Input for new tasks-->
    <div class="input-section">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Ajouter une nouvelle tâche"
      />
      <button @click="addTask">Ajouter</button>
    </div>

    <!-- List the tasks -->
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <!-- Affichage des tâches -->
        <span
          :class="{ completed: task.completed }"
          @dblclick="editTask(index)"
        >
          {{ task.text }}
        </span>

        <!-- Modifier une tâche -->
        <input
          v-if="task.editing"
          v-model="task.text"
          @keyup.enter="stopEditing(index)"
          @blur="stopEditing(index)"
        />

        <!-- Boutons d'actions -->
        <div class="actions">
          <button @click="toggleComplete(index)">
            {{ task.completed ? "Annuler" : "Terminer" }}
          </button>
          <button @click="deleteTask(index)">Supprimer</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script >
import { ref, watch } from "vue";

export default {
  name: "TodoList",
  setup() {
    const newTask = ref(""); // Champ pour la nouvelle tâche
    const tasks = ref(JSON.parse(localStorage.getItem("tasks")) || []); // Tâches

    const addTask = () => {
      if (newTask.value.trim()) {
        tasks.value.push({ text: newTask.value, completed: false, editing: false });
        newTask.value = "";
        saveTasks();
      }
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
      saveTasks();
    };

    const toggleComplete = (index) => {
      tasks.value[index].completed = !tasks.value[index].completed;
      saveTasks();
    };

    const editTask = (index) => {
      tasks.value[index].editing = true;
    };

    const stopEditing = (index) => {
      tasks.value[index].editing = false;
      saveTasks();
    };

    const saveTasks = () => {
      localStorage.setItem("tasks", JSON.stringify(tasks.value));
    };

    // sauvegarde dès que tasks change
    watch(tasks, saveTasks, { deep: true });

    return {
      newTask,
      tasks,
      addTask,
      deleteTask,
      toggleComplete,
      editTask,
      stopEditing,
    };
  },
};
</script>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: auto;
  text-align: center;
}
.input-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}
.input-section input {
  padding:25px;
}
input[type="text"] {
  padding: 5px;
  margin-right: 10px;
  flex: 1;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
.completed {
  text-decoration: line-through;
  color: gray;
}
.actions button {
  margin-left: 10px;
}
</style>

