<template>
  <div class="container">
    <h2>{{ msg }}</h2>
    <form @submit.prevent="addTodo">
      <input type="text" placeholder="type your todo list" v-model="todo">
    </form>
    <div class="todo-list" v-for="(todo,index) in todoItems">
      <input type="checkbox" v-model="todo.complated" @click="todoComplated(todo)">
      <div class="todo-label">
         <div class="todo-item" :class="{complated: todo.complated}" v-if="!todo.edit" @dblclick="editTodo(todo)">{{todo.title}}</div>
         <input class="todo-edit-item" type="text" v-if="todo.edit" v-model="todo.title" @keyup.enter="editDone(todo)" @blur="editDone(todo)" @keyup.esc="escEdit(todo)" v-focus>
      </div>
      <p>lorefsdf</p>
      <button @click="dltTodo(index)">&times</button>
    </div>

    <div class="all-checked">
      <input type="checkbox" id="all-checked"> <label for="all-checked" @click="allChecked(todo)">all checked</label>
    </div>
    <div class="clear" @click="clearAll">clear all</div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Vue.js App',
      todo: '',
      beforeEditCache: '',
      idforTodo: 1,
      edit: false,
      todoItems: [{
        id: 1,
        title: 'this is me',
        complated: false,
        edit: false
      },
      {
        id: 1,
        title: 'this is u',
        complated: false,
        edit: false
      }]

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
    addTodo(){
      if(this.todo.trim()){

        this.todoItems.push({
          id: this.idforTodo,
          title: this.todo,
          complate: false,
          edit: false
        })
      }
      this.todo = ''
      this.idforTodo ++
    },
    dltTodo(index){
      this.todoItems.splice(index, 1)
    },
    editTodo(todo){
      todo.edit = true
      this.beforeEditCache = todo.title
    },
    editDone(todo){
      if(todo.title.trim()){
        todo.edit = false
      }else{
        todo.title = this.beforeEditCache
        todo.edit = false
      }
    },
    escEdit(todo){
      todo.title = this.beforeEditCache
      todo.edit = false
    },
    todoComplated(todo){
      // if(todo.complated == false){
      //   todo
      // }
      todo.complated = !todo.complated
    },
    allChecked(){
      for (var i = 0; i < this.todoItems.length; i++) {
        console.log(this.todoItems[i].title)

        this.todoItems[i].complated = !this.todoItems[i].complated
      }
    },
    clearAll(){
      this.todoItems = []
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.todo-list{
  padding-top: 5px;
  padding-bottom: 5px;
  display: flex;
  justify-content: space-between;

}
/*input[type="checkbox"]{
  display: inline-block;
  margin-right: 10px;
  margin-top: 5px;
}*/
.todo-label{
  /*display: flex;*/
  /*justify-content: space-between;*/
}
.complated{
  text-decoration: line-through;
}
</style>
