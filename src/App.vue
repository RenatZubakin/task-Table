<template>
  <div id="app">
    <h1>Задание</h1>
    <TaskTable/>
  </div>
</template>

<script>
import TaskTable from '@/components/TaskTable'
export default {
  name: 'App',
  components: {
    TaskTable
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
