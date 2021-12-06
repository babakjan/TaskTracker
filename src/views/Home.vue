<template>
  <AddTask v-show="showAddTask" @add-task="addTask"/>
  <Tasks
      @delete-task="deleteTask" :tasks="tasks"
      @toggle-reminder="toggleReminder"
  />
</template>

<script>
import Tasks from "@/components/Tasks";
import AddTask from "@/components/AddTask";

export default {
  name: "Home.vue",
  components: {
    Tasks,
    AddTask,
  },
  props: {
    showAddTask: Boolean
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async addTask(newTask) {
      const response = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(newTask)
      })

      const data = await response.json()

      this.tasks = [...this.tasks, data]

    },
    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const response = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        })

        if (response.status === 200) {
          this.tasks = this.tasks.filter((task) => task.id !== id)
        } else {
          alert('Error deleting task')
        }
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder }

      const response = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updatedTask)
      })

      const data = await response.json()

      this.tasks = this.tasks.map((task) => {
        if (task.id === id) {
          return { ...task, reminder: data.reminder }
        } else {
          return task
        }
      })
    },
    async fetchTasks() {
      const response = await fetch('api/tasks')
      const data = await response.json()
      return data
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`)
      const data = await response.json()
      return data
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>

<style scoped>

</style>