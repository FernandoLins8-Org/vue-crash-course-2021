<template>
  <AddTask 
    v-show="showAddTask"
    @add-task="addTask" 
  />

  <Tasks 
    @toggle-reminder="toggleReminder" 
    @delete-task="deleteTask" 
    :tasks="tasks" 
  />
</template>

<script>
import Tasks from '../components/Tasks.vue'
import AddTask from '../components/AddTask.vue'

export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
  },
  components: {
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: []
    }
  },
  methods: {
    async fetchTasks() {
      const res = await fetch('http://localhost:5000/tasks', {
        method: 'GET'
      })
      const tasks = await res.json()
      return tasks
    },
    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'GET'
      })
      const task = await res.json()
      return task
    },
    async addTask(task) {
      const res = await fetch('http://localhost:5000/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(task)
      })
      const newTask = await res.json()
      this.tasks = [...this.tasks, newTask]
    },
    async deleteTask(id) {
      if(confirm('Are you sure?')) {
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {
          method: 'DELETE'
        })
        
        if(res.status === 200) {
          this.tasks = this.tasks.filter(task => task.id !== id)
        }
      }
    },
    async toggleReminder(id) {
      const task = await this.fetchTask(id)
      await fetch(`http://localhost:5000/tasks/${id}`, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          'reminder': !task.reminder
        })
      })
      this.tasks = await this.fetchTasks()
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>