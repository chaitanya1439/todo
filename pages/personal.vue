<template>
    <head>
        <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0" />
    </head>
    <div class="chai">
        <header class="chai-header">
            <a href="/" class="material-symbols-outlined" style="text-decoration: none;">arrow_back</a>
            <h1 class="chai-title">Personal</h1>
            <!-- Task Input -->
            <input v-model="newTaskText" placeholder="Add new task" text="text" style="width: 300px;" />
            <!-- Add Task Button -->
            <button @click="addTask" class="chai-add">+</button>
        </header>
        <aside class="chai-sidebar">
            <div class="chai-progress">
                <h2 class="chai-progress-title">Progress</h2>
                <div class="chai-progress-bar">
                    <!-- Progress Bar Fill -->
                    <div class="chai-progress-bar-fill" :style="{ width: progress + '%' }"></div>
                </div>
                <!-- Progress Text -->
                <p class="chai-progress-text">{{ progress }}% completed</p>
            </div>
            <div class="chai-activity">
                <h2 class="chai-activity-title">Activity</h2>
                <ul class="chai-activity-list">
                    <!-- Tasks List -->
                    <li v-for="task in tasks" :key="task.id" class="chai-activity-item">
                        {{ task.text }} - {{ task.status }}
                        <!-- Example of status change buttons -->
                        <button @click="updateTaskStatus(task.id, 'In Progress')">Start</button>
                        <button @click="updateTaskStatus(task.id, 'Done')">Done</button>
                        <button class="material-symbols-outline" @click="deleteTask(task.id)">close</button>
                    </li>
                </ul>
            </div>
        </aside>
        <main class="chai-main">
            <div class="chai-column chai-column-ready">
                <h2 class="chai-column-title">Ready</h2>
                <ul class="chai-column-list">
                    <!-- Tasks Ready -->
                    <li v-for="task in readyTasks" :key="task.id">
                        {{ task.text }}
                    </li>
                </ul>
            </div>
            <div class="chai-column chai-column-in-progress">
                <h2 class="chai-column-title">In Progress</h2>
                <ul class="chai-column-list">
                    <!-- Tasks In Progress -->
                    <li v-for="task in inProgressTasks" :key="task.id">
                        {{ task.text }}
                    </li>
                </ul>
            </div>
            <div class="chai-column chai-column-done">
                <h2 class="chai-column-title">Done</h2>
                <ul class="chai-column-list">
                    <!-- Tasks Done -->
                    <li v-for="task in doneTasks" :key="task.id">
                        {{ task.text }}
                    </li>
                </ul>
            </div>
        </main>
    </div>
</template>
<script lang="ts">
import { defineComponent, ref, computed } from 'vue';

interface Task {
  id: number;
  text: string;
  status: 'Ready' | 'In Progress' | 'Done' | 'Delete';
}

export default defineComponent({
  name: 'PersonalPage',
  setup() {
    const newTaskText = ref('');
    const tasks = ref<Task[]>([]);

    const addTask = () => {
      if (newTaskText.value.trim()) {
        tasks.value.push({
          id: Date.now(),
          text: newTaskText.value,
          status: 'Ready'
        });
        newTaskText.value = '';
      }
    };

    const deleteTask = (taskId: number) => {
      tasks.value = tasks.value.filter(task => task.id !== taskId);
    };

    const progress = computed(() => {
      const completedTasks = tasks.value.filter(task => task.status === 'Done').length;
      const totalTasks = tasks.value.length;
      return totalTasks > 0 ? Math.round((completedTasks / totalTasks) * 100) : 0;
    });

    const readyTasks = computed(() => tasks.value.filter(task => task.status === 'Ready'));
    const inProgressTasks = computed(() => tasks.value.filter(task => task.status === 'In Progress'));
    const doneTasks = computed(() => tasks.value.filter(task => task.status === 'Done'));

    const updateTaskStatus = (taskId: number, status: Task['status']) => {
      const taskIndex = tasks.value.findIndex(task => task.id === taskId);
      if (taskIndex !== -1) {
        tasks.value[taskIndex].status = status;
      }
    };

    return {
      newTaskText,
      tasks,
      addTask,
      deleteTask,
      progress,
      readyTasks,
      inProgressTasks,
      doneTasks,
      updateTaskStatus
    };
  }
});
</script>
<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

.chai {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: 80px 1fr;
    grid-template-areas:
        "header header"
        "sidebar main";
    height: 100vh;
    font-family: Arial, sans-serif;
    color: white;
    background-color: #222;
}

.chai-header {
    grid-area: header;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;
    border-bottom: 1px solid #333;
}

.chai-title {
    font-size: 24px;
    font-weight: bold;
}

.chai-add {
    width: 120px;
    height: 40px;
    border: none;
    border-radius: 20px;
    background-color: #28a745;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
}

.chai-sidebar {
    grid-area: sidebar;
    display: flex;
    flex-direction: column;
    padding: 20px;
    border-right: 1px solid #333;
}

.chai-progress {
    margin-bottom: 20px;
}

.chai-progress-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.chai-progress-bar {
    width: 100%;
    height: 20px;
    border: 1px solid #333;
    border-radius: 10px;
    overflow: hidden;
}

.chai-progress-bar-fill {
    width: 0%;
    height: 100%;
    background-color: #28a745;
    transition: width 0.5s;
}

.chai-progress-text {
    font-size: 14px;
    margin-top: 10px;
}

.chai-activity {
    flex: 1;
    overflow-y: auto;
}

.chai-activity-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 10px;
}

.chai-activity-list {
    list-style: none;
}

.chai-activity-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.chai-activity-icon {
    width: 20px;
    height: 20px;
    margin-right: 10px;
    border-radius: 50%;
    background-color: #28a745;
}

.chai-activity-text {
    font-size: 14px;
}

.chai-main {
    grid-area: main;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 20px;
    padding: 20px;
}

.chai-column {
    height: 100%;
    overflow-y: auto;
    background-color: #333;
    border-radius: 10px;
}

.chai-column-title {
    font-size: 18px;
    font-weight: bold;
    padding: 10px;
    border-bottom: 1px solid #444;
}

.chai-column-list {
    list-style: none;
}

.chai-task {
    display: flex;
    flex-direction: column;
    padding: 10px;
    border-bottom: 1px solid #444;
}

.chai-task-text {
    font-size: 16px;
    margin-bottom: 10px;
}

.chai-task-info {
    display: flex;
    align-items: center;
    justify-content: space-between;
}

.chai-task-date {
    font-size: 14px;
    color: #aaa;
}

.chai-task-delete {
    width: 20px;
    height: 20px;
    border: none;
    border-radius: 50%;
    background-color: #dc3545;
    color: white;
    font-size: 12px;
    cursor: pointer;
}
</style>