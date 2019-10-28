<template>
  <div>
      <input type="text" class="todo-input" placeholder="What Needs to be Done" v-model="newTodo">
      <!-- <div v-for="todo in todos" :key="todo.id" class="todo-item"> -->  
      <div v-for="todo in todos" :key="todo.id" class="todo-item">
          {{ todo.title }}
      </div>    
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
      return {
          newTodo: '',
          todos: [
              {
                  'id': 1,
                  'title': 'Finish Vue Screencast',
                  'completed': false,

              },
              {
                  'id': 2,
                  'title': 'Take over the world',
                  'completed': false,

              }
          ]
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
    .todo-input {
        width: 100%;
        padding: 10px 18px;
        font-size: 18px;
        margin-bottom: 16px;
    }

</style>
