<template>
  <div id="app">
   <TodoHeader></TodoHeader>
   <TodoInput></TodoInput>
   <TodoList></TodoList>
   <TodoFooter></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoFooter from './components/TodoFooter.vue'
import TodoInput from './components/TodoInput.vue'
import TodoList from './components/TodoList.vue'


export default {
  name: 'app',
  components: {
    'TodoHeader' : TodoHeader,
    'TodoList' : TodoList,
    'TodoInput' : TodoInput,
    'TodoFooter' : TodoFooter 
  }
}
</script>

<style>

</style>
