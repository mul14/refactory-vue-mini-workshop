<template>
  <div id="app">
    <div>
      Search <input type="text" v-model="search">
    </div>

    <ul>
      <li v-for="task in filteredTasks">
        <div v-if="editId !== task.id">
          {{ task.name }}
        </div>
        <div v-if="editId === task.id">
          <input type="text" v-model="task.name">
        </div>
        <div v-if="editId !== task.id">
          <button @click="edit(task)">Edit</button>
        </div>
        <div v-else>
          <button @click="save(task)">Save</button>
          <button @click="cancel(task)">Cancel</button>
        </div>
        <button @click="remove(task)">&times;</button>
      </li>
    </ul>
    <div>
      <input type="text" v-model="newTask">
      <button @click="add">Add new task</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',

  mounted() {
    this.fetchingTasksData()
  },

  data () {
    return {
      newTask: '',
      search: '',
      originalTask: '',
      editId: null,
      tasks: []
    }
  },

  computed: {
    filteredTasks() {
      return this.tasks.filter(task => {
        return task.name.toLowerCase().indexOf(
          this.search.toLowerCase()
        ) > -1
      })
    }
  },

  methods: {
    add() {
      this.tasks.push({ name: this.newTask })
      this.newTask = ''
    },

    edit(task) {
      this.originalTask = task.name
      this.editId = task.id
    },

    save(task) {
      this.originalTask = ''
      this.editId = null
    },

    cancel(task) {
      task.name = this.originalTask
      this.originalTask = ''
      this.editId = null
    },

    remove(task) {
      const index = this.tasks.indexOf(task)
      this.tasks.splice(index, 1)
    },

    async fetchingTasksData() {
      const res = await axios.get('http://localhost:3000/tasks')
      this.tasks = res.data
    }
  }
}
</script>

<style>

</style>
