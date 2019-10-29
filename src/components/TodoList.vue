<template>
  <div>
      <input type="text" class="todo-input" placeholder="Add to the Wish List" v-model="newTodo" @keyup.enter="addTodo">
      <!-- <div v-for="todo in todos" :key="todo.id" class="todo-item"> -->  
      <div v-for="(todo, index) in todos" :key="todo.id" class="todo-item">
          <div class="todo-item-left">
              <input type="checkbox" v-model="todo.completed">
              <!--below, adding small feature [adding a class conditionally] to bind-->
              <!--adding another class to existing class, replacing the need for a ternary operator-->
          <div v-if="!todo.editing" @click="editTodo(todo)" class="todo-item-label" :class="{ completed : todo.completed }">{{ todo.title }}</div>
          <!--v-else conditional hides option to edit below-->
          <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>
          </div>
          <!-- 'x' icon to remove from list-->
          <div class="remove-item" @click="removeTodo(index)">
              &times;
          </div>
      </div>
    <div class="extra container">
        <div><label><input type="checkbox"> Check All</label></div>
        <div>{{ remaining }} items left</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
      return {
          newTodo: '',
          idForTodo: 3,
          beforeEditCache: '',
          todos: [
              {
                  'id': 1,
                  'title': 'Finish Vue Screencast',
                  'completed': false,
                  'editing': false,
              },
              {
                  'id': 2,
                  'title': 'Take over the world',
                  'completed': false,
                  'editing': false,
              },
              {
                  'id': 3,
                  'title': 'Or just playing in the sandbox',
                  'completed': false,
                  'editing': false,
              },
              {
                  'id': 4,
                  'title': 'Skilled enough to build a castle',
                  'completed': false,
                  'editing': false,
              },
          ]
      }
    },
    // could write all methods in computed, but mainly for composing new data derived from existing data.
    // rules: should not mutate data, not accept parameters, must always return something
    computed: {
        remaining () {
            return this.todos.filter(todo => !todo.completed)
        }

    },
    // below, is a custom directive
    directives: {
        focus: {
            inserted: function (el) {
                el.focus()
            }
        }
    },
      methods: {
          addTodo() {
              if(this.newTodo.trim().length == 0) {
                  return
              }
              this.todos.push({
                  id: this.idForTodo,
                  title: this.newTodo,
                  completed: false,
              })
              this.newTodo = ''
              this.idForTodo++
              //alert('adding')
          },
          // this method is editing the STATE of the object 'todo'
          editTodo(todo) {
              this.beforeEditCache = todo.title
              //alert('Editing is working')
              todo.editing = true
          },
          doneEdit(todo) {
              // below, CONDITIONAL doesn't allow for empty strings
              if(todo.title.trim().length == '') {
                  // SETTING title to the cache 
                  todo.title = this.beforeEditCache
              }
              //alert('Editing is working')
              todo.editing = false
          },
          cancelEdit(todo) {
              todo.title = this.beforeEditCache
              todo.editing = false
          },
          removeTodo(index) {
              this.todos.splice(index, 1)
          },
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

        &:focus {
            outline: 0;
        }
    }
    .todo-item {
        margin-bottom: 12px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    .remove-item {
        cursor: pointer;
        margin-left: 14px;
        &:hover {
            color: black;
        }
    }
    .todo-item-left {
        display: flex;
        align-items: center;
    }
    .todo-item-label {
        padding: 10px;
        border: 1px solid white;
        margin-left: 12px;
    }

    .todo-item-edit {
        font-size: 24px;
        color: #2c3e50;
        margin-left: 12px;
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        font-family: 'Avenir', Helvetica, Arial, sans-serif;

        &:focus {
            outline: none;
        }
    }

    .completed {
        text-decoration: line-through;
        color: grey;
    }

    .extra-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 14px;
        margin-bottom: 14px;
    }

    button {
        font-size: 14px;
        background-color: white;
        appearance: none;

        &:hover {
            background: lightgreen;
        }
        &:focus {
            outline: none;            
        }        
    }

    .active {
        background-color: lightgreen;
    }

</style>
