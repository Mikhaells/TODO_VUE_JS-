<template>
  <nav class="navbar bg-body-tertiary">
  <div class="container">
    <a class="navbar-brand" href="#">Navbar</a>
  </div>
</nav>
  <main class="container">
    <form class="mt-5" v-on:submit.prevent="saveToDo">
      <div class="mb-3">
        <label for="input-todo" class="form-label">Add Activities</label>
        <input type="text" class="form-control" id="input-todo" aria-describedby="emailHelp" v-model="inputToDo"> 
      </div>
       
      <div class="mb-3 form-check">
        <input type="checkbox" class="form-check-input" id="reminder" v-model="reminderToDo">
        <label class="form-check-label" for="reminder">Remember Me</label>
      </div>
      <button type="submit" class="btn btn-primary">SIMPAN</button>
    </form>
    
    <section class="mt-5">
      <h4>To-Do List</h4>
      <ol class="list-group list-group-numbered">
          <li v-for="todo in todoList" :key="todo.id"
           class="list-group-item d-flex justify-content-between align-items-start">
            <div class="ms-2 me-auto">
              <div :class="{'fw-bold': todo.reminder}">{{ todo.text }}</div>
              {{ new Date(todo.createdAt).toLocaleTimeString() }}
            </div>
            <span class="badge bg-danger rounded-pill fs-5" role="button" v-on:click="deleteToDo(todo.id)">
              <i class="bi bi-x-lg"></i>
            </span>
          </li> 
      </ol>
    </section>


  </main>
</template>

<script>

import {v4 as uuidV4} from "uuid";

export default {
name:"App",
data(){
    return{
      inputToDo : "",
      reminderToDo : false,
      todoList:[]
    }
},
methods: {
    saveToDo(){
      const newTodo = {
        id:uuidV4(),
        text:this.inputToDo,
        reminder:this.reminderToDo,
        createdAt:new Date()
      };

      //this.todoList = this.todoList.push(newTodo);
      this.todoList = [newTodo, ... this.todoList] 
      localStorage.setItem('todo',JSON.stringify(this.todoList))
      this.inputToDo ='';
      this.reminderToDo = false; 
    },
    deleteToDo(id){
      console.log(id);
      const newToDos = this.todoList.filter((todo) => todo.id != id);
      this.todoList = newToDos;
      localStorage.setItem('todo',JSON.stringify(this.todoList))

    },  
  },
  mounted(){ 
      this.todoList = JSON.parse(localStorage.todo ||'[]') 
  }
};
</script>
 