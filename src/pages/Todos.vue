<template>
  <q-page class="column fit content-center"  style="width: 50%;">
  <q-skeleton type="text" v-if="isLoading" square width="30%" height="6em"/>
  <form @submit.prevent='createTodo' v-else>
    <q-input dark
      type='text'
      v-model='newTodo.text'
      placeholder='New Todo'
      >
    </q-input>
  </form>
  <q-skeleton type="text" v-if="isLoading" square width="30%" height="6em"/>
  <q-list dark v-else>
    <todo
      v-for='todo in todos'
      :key='todo.id'
      :todo='todo'
      @change='saveTodo'
      class='my-1'
    />
  </q-list>
  <q-skeleton type="text" v-if="isLoading" square width="30%" height="6em"/>
  <div v-else>
    <p v-if='todos.length'>{{ done }} / {{ total }} complete</p>
    <q-button dark
      @click='deleteCompleted'
      v-if='done'
    >
      Clear completed
    </q-button>
  </div>
</q-page>
</template>

<script>
import axios from 'axios'
import { v4 as uuidv4 } from 'uuid'

import Todo from 'components/Todo'
export default {
  components: { Todo },
  data () {
    return {
      isLoading: true,
      newTodo: {},
      todos: [],
      pendingOps: []
    }
  },
  computed: {
    isSaving () {
      return this.pendingOps.length > 0
    },
    done () {
      return this.todos.filter(todo => todo.isDone).length
    },
    remaining () {
      return this.todos.filter(todo => !todo.isDone).length
    },
    total () {
      return this.todos.length
    }
  },
  created () {
    axios.get('/api/todos').then(({ data }) => {
      this.isLoading = false
      this.todos = data
    })
  },
  methods: {
    async createTodo () {
      const operationId = uuidv4()
      this.pendingOps.push(operationId)
      let todo = { ...this.newTodo }
      const index = this.todos.length
      this.$set(this.todos, index, todo)
      this.newTodo = {}
      todo = await axios
        .post('/api/todos', { data: todo })
        .then(json => json.data)
      // Update client side cache with record from server
      this.$set(this.todos, index, todo)
      this.pendingOps = this.pendingOps.filter(id => id !== operationId)
    },
    async saveTodo (todo) {
      const operationId = uuidv4()
      this.pendingOps.push(operationId)
      const index = this.todos.findIndex(t => t.id === todo.id)
      this.$set(this.todos, index, todo)
      await axios.patch(`/api/todos/${todo.id}`, { data: todo })
      this.pendingOps = this.pendingOps.filter(id => id !== operationId)
    },
    async deleteCompleted () {
      const operationId = uuidv4()
      this.pendingOps.push(operationId)
      const completedTodos = this.todos.filter(t => t.isDone)
      const remainingTodos = this.todos.filter(t => !t.isDone)
      this.todos = remainingTodos
      await Promise.all(
        completedTodos.map(todo => axios.delete(`/api/todos/${todo.id}`))
      )
      this.pendingOps = this.pendingOps.filter(id => id !== operationId)
    }
  }
}
</script>

<style lang='css' scoped></style>
