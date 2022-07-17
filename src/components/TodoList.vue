<script setup>
import { ref } from "@vue/reactivity";
import TodoListItem from "./TodoListItem.vue"

const response = await fetch('https://jsonplaceholder.typicode.com/todos');
let todos = await response.json();
todos=todos.slice(0,10);

const reactiveTodos=ref(todos);
const newTitle = ref("");


function handleToDoItemDeleted(todoItemId){
  console.log(`item deleted: ${todoItemId}`);
  reactiveTodos.value = reactiveTodos.value.filter(todo => todo.id!==todoItemId);
  console.log(reactiveTodos.value);
}

function handleToDoItemCompleted(todoItemId, completed){
  reactiveTodos.value = reactiveTodos.value.map(todo => {
    if(todo.id === todoItemId){
      return {
        ...todo,
        completed: completed
      }
    }else{
      return todo;
    }
  })
  console.log(`item completed: ${todoItemId} >> ${completed}`);
}

function handeleAddNewTodo(newTodoTitle){
  const getLastId = reactiveTodos.value.slice(-1)[0].id;
  const newId = getLastId+1;
  console.log(getLastId, newId);
  console.log(newTodoTitle);
  console.log(todos);

  reactiveTodos.value.push({userId: 1, id: newId, title: newTodoTitle, completed: false});
  console.log(reactiveTodos.value);
  reactiveTodos.value = reactiveTodos.value.filter(todo => todo);

}

</script>  

<template>   
<!-- <div>
   <div v-for="todo of todos" :key="todo.key" >
   <div  class="grid">
   <div class="grid">{{todo.id}}</div>
    <div class="grid">{{todo.title}}</div>
    <div class="grid">
    <input type="checkbox" :checked="todo.completed"/>
    </div>
     <div class="grid">
        <button>Delete</button>
    </div>
   
   </div> </div>
      
</div>  -->
<div>
  <input type="text"  v-model="newTitle">
  <p>New title is: {{newTitle}}</p>
  <button type="submit" @click="handeleAddNewTodo(newTitle)">Add todo item</button>
    <table>
    <thead>
        <tr>
        <th>ID</th>
        <th>Title</th>
        <th>Checked</th>
        <th>Options</th>

        </tr>
    </thead>
    <tbody>
        <TodoListItem v-for="todo of reactiveTodos" :key="todo.id" :id="todo.id" :title="todo.title" :completed="todo.completed"
        @todo-item-deleted="handleToDoItemDeleted(todo.id)"
        @todo-item-completed="completed => handleToDoItemCompleted(todo.id, completed)"
        />
    </tbody>
    </table>
</div>
</template>

<style scoped>



table {
  display: grid;
  border-collapse: collapse;
  min-width: 100%;
  grid-template-columns: 
    minmax(150px, 1fr)
    minmax(150px, 1.67fr)
    minmax(150px, 1fr)
    minmax(150px, 1fr);
}

thead,
tbody,
tr {
  display: contents;
}

th,
td {
  padding: 15px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

th {
  position: sticky;
  top: 0;
  background: #6c7ae0;
  text-align: left;
  font-weight: normal;
  font-size: 1.1rem;
  color: white;
  
}


td {
  padding-top: 10px;
  padding-bottom: 10px;
  color: #808080;
}


</style>