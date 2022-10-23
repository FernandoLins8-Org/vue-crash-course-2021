<template>
  <header >
    <h1>{{title}}</h1>
    <Button 
      v-show="homePage"
      @btn-click="$emit('toggle-add-task-form')"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'coral' : 'green'"
    />
  </header>
</template>

<script>
  import Button from './Button.vue'

  export default {
    name: 'Header',
    props: {
      title: {
        type: String,
        default: 'Hello World',
      },
      showAddTask: {
        type: Boolean
      }
    },
    components: {
      Button
    },
    computed: {
      homePage() {
        if(this.$route.path === '/') {
          return true
        } else {
          return false
        }
      }
    },
    emits: ['toggle-add-task-form']
  }
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>