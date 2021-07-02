<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
    <Footer />
  </div>
</template>
<script>
import Header from "./components/Header";
import Footer from "./components/Footer.vue";
import Tasks from "./components/Tasks.vue";
import AddTask from "./components/AddTask.vue";

export default {
  name: "App",
  components: {
    Header,
    Footer,
    Tasks,
    AddTask,
  },
  data() {
    return { tasks: [], showAddTask: false };
  },
  methods: {
    async addTask(task) {
      const res = await fetch("http://localhost:5000/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();
      this.tasks = [...this.tasks, data];
    },
    async toggleAddTask() {
      const taskToToggle = await this.fetchTask(id);
      const updateTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: json.stringify(updateTask),
      });
      const data = await res.json();
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: "DELETE",
        });
        res.status === 2000
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");

        this.tasks = data.filter((task) => task.id !== id);
      }
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: !task } : task
      );
    },
    async fetchTasks() {
      const response = await fetch("http://localhost:5000/tasks");
      const data = await response.json();
      return data;
    },
    async fetchTask() {
      const response = await fetch(`http://localhost:5000/task/${id}`);
      const data = await response.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style>
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 2px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
</style>
