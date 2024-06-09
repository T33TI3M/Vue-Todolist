<template>
  <h1>{{ title }}</h1>
  <div>
    <div>You Have {{ allTasks }} {{ allTasks > 1 ? "tasks" : "task" }} at the moment</div>
    <input
      type="text"
      v-model="newTask"
      placeholder="Add a new task"
      @keyup.enter="addTask"
    />
  </div>

  <button @click="addTask" :disabled="newTask.length < 1">Add Task</button>

  <div v-if="newTask.length > 0">
    <h3>New Task preview</h3>
    <p>{{ newTask }}</p>
  </div>

  <ul>
    <li
      v-for="(task, index) in lastest"
      :key="task.id"
      @click="finishTask(task)"
      :class="{ strikeout: task.finished }"
    >
      {{ index + 1 }}. {{ task.name }}
      <div class="button" v-if="task.finished">
        <button @click="deleteTask(task.id)">Delete task</button>
      </div>
      <div class="button" v-else-if="!task.finished">
        <button>Edit</button>
      </div>
      <div class="button" v-else>
        <button>No Button</button>
      </div>
    </li>
  </ul>
</template>

<script lang="ts">
import { defineComponent } from "vue";

type Task = {
  id: number;
  name: string;
  finished: boolean;
};
type Model = {
  title: string;
  newTask: string;
  tasks: Task[];
};

export default defineComponent({
  // Data of vue
  data(): Model {
    return {
      title: "hello vue",
      newTask: "",
      tasks: [
        { id: 1, name: "vue 1", finished: false },
        { id: 2, name: "vue 2", finished: true },
        { id: 3, name: "vue 3", finished: false },
      ],
    };
  },

  // Method of Vue
  methods: {
    addTask() {
      if (this.newTask.length < 1) return;
      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        finished: false,
      });
      this.newTask = "";
    },
    finishTask(task: Task) {
      task.finished = !task.finished;
    },
    deleteTask(taskID: number) {
      this.tasks = this.tasks.filter((prevTask) => {
        return prevTask.id !== taskID;
      });
    },
    
  },

  computed: {
    allTasks(): number {
      return this.tasks.length;
    },
    lastest(): Task[] {
      return [...this.tasks].reverse();
    },
  },
  

});
</script>

<style>
.strikeout {
  text-decoration: line-through;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
