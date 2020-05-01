<template>
  <q-item dark clickable v-ripple>
   <q-checkbox dark color="primary"
    v-model="localTodo.isDone" @change="save">
  </q-checkbox>
    <q-input dark color="primary"
        v-model="localTodo.text"
        placeholder="New Todo"
        @focus="isFocused = true"
        @blur="save"
      />
    </q-item>
</template>

<script>
export default {
  props: ['todo'],
  data () {
    return {
      localTodo: { ...this.todo },
      isFocused: false
    }
  },
  watch: {
    todo: {
      handler () {
        this.localTodo = { ...this.todo }
      },
      deep: true
    }
  },
  computed: {
    hasChanges () {
      return (
        this.localTodo.text !== this.todo.text ||
        this.localTodo.isDone !== this.todo.isDone
      )
    }
  },
  methods: {
    async save () {
      this.isFocused = false
      if (this.hasChanges) {
        this.$emit('change', this.localTodo)
      }
    }
  }
}
</script>
