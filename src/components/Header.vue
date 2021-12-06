<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
        v-show="homePage"
        @btn-click="$emit('toggle-add-task')"
        :text="showAddTask ? 'Close' : 'Add Task'"
        :color="showAddTask ? 'red' : 'green'"
        class="btn"
    />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: {
      type: String,
      default: 'Hello Word'
    },
    showAddTask: Boolean
  },
  components: {
    Button
  },
  emits: ['toggle-add-task'],
  computed: {
    homePage() {
      return this.$route.path === '/'
    }
  }
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