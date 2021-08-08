<template>
  <div>
      <input type="text" placeholder="Add a To Do work" class="input-field" v-model="newWork" @keyup.enter="addNewWork" @click="isShowing ^= true">
      <div class="new-input" v-show="isShowing" >
        {{newWork}}
      </div>
        <div v-for="(todo, index) in todosFilter" :key="todo.id" class="newWork-items">
            <div class="newWork-items-left">
                <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editing" @dblclick="editWork(todo)" class="newWork-label" :class="{ completed : todo.completed}" >  {{todo.title}} </div>
          <input v-else class="edit" type="text" v-model="todo.title" @blur="done(todo)" @keyup.enter="done(todo)" v-focus @keyup.esc="cancel(todo)">
            </div>
          <div class="remove" @click="removeWork(index)">
              &times;
          </div>
        </div>
        <div class="second">
        <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAll">Check All</label></div>
        <div>{{remaining}} items remaining</div>
        </div>
        <div class="second">
        <div>
            <button :class="{active: filter == 'all'}" @click="filter = 'all'">All</button>
            <button :class="{active: filter == 'active'}" @click="filter ='active'">Active</button>
            <button :class="{active: filter == 'completed'}" @click="filter ='completed'">Completed</button>
        </div>
        <div>
            <button v-if="showClear" @click="clearCompleted">Clear Completed</button>
        </div>
        </div>
  </div>
</template>

<script>
export default {
  name: 'todo',
  data (){
      return {
          newWork : '',
          todoid : 3,
          beforeEdit : '',
          filter : 'all',
          isShowing: true,
          todos : [
              {
                  'id' : 1,
                  'title' : 'Wake Up Early',
                  'completed' : false,
                  'editing' : false,
              },
              {
                  'id' : 2,
                  'title' : 'Exercise',
                  'completed' : false,
                  'editing' : false,
              },

          ]
      }
  },
  computed: {
      remaining() {
          return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining(){
          return this.remaining != 0
      },
      todosFilter(){
          if(this.filter == 'all'){
              return this.todos
          }else if(this.filter == 'active'){
              return this.todos.filter(todo => !todo.completed)
          }else if(this.filter == 'completed'){
              return this.todos.filter(todo => todo.completed)
          }
          return this.todos
      },
      showClear(){
          return this.todos.filter(todo => todo.completed).length > 0
      }
  },
  directives: {
      focus: {
          inserted: function(el){
              el.focus()
          }
      }
  },
  methods: {
      addNewWork(){
          if(this.newWork.trim().length == 0){
              return
          }

          this.todos.push({
              id : this.todoid,
              title : this.newWork,
              completed : false,
          })

          this.newWork = ''
          this.todoid++
      },
      newInput(){

      },
       editWork(todo){
          todo.editing = true
          this.beforeEdit = todo.title
      },
      removeWork(index){
          this.todos.splice(index, 1)
      },
      done(todo){
          if(todo.title.trim() == ''){
              todo.title = this.beforeEdit
          }
          todo.editing = false
      },
      cancel(todo){
          todo.editing = false
          todo.title = this.beforeEdit
      },
      checkAll(){
          this.todos.forEach((todo) => todo.completed = event.target.checked)
      },
      clearCompleted(){
          this.todos = this.todos.filter(todo => !todo.completed)
      }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

.input-field{
    width: 100%;
    padding: 10px 18px;
    font-size: 18px;
}
.input-field:focus{
    outline: 0;
}
.newWork-items{
    display: flex;
    justify-content: space-between;
    margin-bottom: 12px;
    align-items: center;
}
.remove{
    cursor: pointer;
    margin-left: 14px;
}
.remove:hover{
    color: red;
}
.newWork-items-left{
    display: flex;
    align-items: center;
}
.newWork-label{
    padding: 10px;
    border: 1px solid white;
    margin-left: 12px;
}
.edit{
    font-size: 20px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    font-family: Arial, Helvetica, sans-serif;
}
.edit:focus{
    outline: none;
}
.completed{
    text-decoration: line-through;
    color: grey;
}
.second{
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 16px;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 14px;
}
button{
    font-size: 14px;
    background-color: white;
    appearance: none;
}
button:hover{
    background-color: lightgreen;
}
button:focus{
    outline: none;
}
.active{
    background-color: lightgreen;
}
.new-input{
    width:100%;
    height: 40px;
    border: 1px solid black;
    border-top : none;
    display: block;
    padding: 10px 18px;
}
</style>
