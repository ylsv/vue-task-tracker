<template>
  <AddTask v-show="showAddTask" @add-task="addTask" />
  <Tasks :tasks='tasks' @delete-task="deleteTask" @toggle-reminder="toggleReminder"/>
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
    AddTask,
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async fetchTasks() {
      try {
        const res = await fetch('api/tasks')
        const data = await res.json()
        this.tasks = data
      } catch(e) {
        return e
      }
    },
    async fetchTask(id) {
      try {
        const res = await fetch(`api/tasks/${id}`)
        const data = await res.json()
        return data
      } catch(e) {
        return e
      }
    },
    async deleteTask(taskId) {
      if (confirm('Are you sure?')){
        const res = await fetch(`api/tasks/${taskId}`, {
          method: 'DELETE',
        })
        if (res.status !== 200) console.log('delete error')
        await this.fetchTasks()
      }
    },
    async toggleReminder(taskId) {
      const taskToToggle = await this.fetchTask(taskId)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      await fetch(`api/tasks/${taskId}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })

      await this.fetchTasks()
    },
    async addTask(newTask) {
      await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(newTask)
      })
      await this.fetchTasks()
    },
  },
  async created() {
    await this.fetchTasks()
  }
}
</script>