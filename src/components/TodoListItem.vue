<script setup>
defineProps({
    id: Number,
    title: String,
    completed: Boolean
})

const emit = defineEmits(['todoItemDeleted', 'todoItemCompleted']);


function handleClick(){
  console.log('Clicked!');
  emit('todoItemDeleted');

}

function handleChange(event){
  emit('todoItemCompleted',event.target.checked);
}

</script>

<template>
     <tr>
        <td>{{id}}</td>
        <td>{{title}}</td>
        <td>
            <input type="checkbox" :checked="completed" @change="handleChange"/>
        </td>
        <td>
            <button @click="handleClick" class="button">Delete</button>
        </td>
        </tr>
   
</template>

<style scoped>

button:hover{
           background-color: #868bad;


}
.button{
background: #6c7ae0;
color: white;
border-radius: 5%;
cursor: pointer;
border: none;

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
tr:nth-child(even) td {
  background: #f8f6ff;
}

</style>