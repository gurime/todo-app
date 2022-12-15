<script setup>
import { ref, onMounted, computed, watch } from "vue";

const todos = ref([]);
const name = ref("");

const input_content = ref("");
const input_category = ref(null);

const todos_asc = computed(() =>
  // eslint-disable-next-line vue/no-side-effects-in-computed-properties
  todos.value.sort((a, b) => {
    return a.createdAt - b.createdAt;
  })
);

watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  {
    deep: true,
  }
);

const addTodo = () => {
  if (input_content.value.trim() === "" || input_category.value === null) {
    return;
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime(),
  });
};

const removeTodo = (todo) => {
  todos.value = todos.value.filter((t) => t !== todo);
};

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todos.value = JSON.parse(localStorage.getItem("todos")) || [];
});
</script>











<template>
<div class='container' >
<div class="flex-title">
<h1 class="title">Hello</h1>
<input
type="text"
class="title-input"
id="name"
placeholder="Name here"
v-model="name"
/>
</div>


<div class="title-block">
<h1 class="title">
create a todo
</h1>
</div>

<!--start of hero-->
<div class="hero">
<form action="" style="background-color: transparent;">

<input 
type="text" 
name="content"
id="content"
v-model="input_content"
placeholder="e.g. pick up the kids"
class="input-todo">
</form>
</div>
<!--end of hero-->


<div class="title-block">

<h1 class="title">Pick a category</h1>
</div>

<form style="border-bottom:solid 1px; margin-bottom: 1rem;" id="new-todo-form" @submit.prevent="addTodo">
<div class="options-block">
<label>
<input
type="radio"
name="category"
id="category1"
value="business"
v-model="input_category"/>
<span class="bubble business"></span>
<div>Business</div>
</label>

<label>
<input
type="radio"
name="category"
id="category1"
value="personal"
v-model="input_category"/>
<span class="bubble personal"></span>
<div>Personal</div>
</label>

<label>
<input
type="radio"
name="category"
id="category2"
value="date"
v-model="input_category"/>
<span class="bubble date"></span>
<div>Date</div>
</label>

<label>
<input
type="radio"
name="category"
id="category3"
value="grocerys"
v-model="input_category"/>
<span class="bubble grocerys"></span>
<div>Grocerys</div>
</label>

<label>
<input
type="radio"
name="category"
id="category4"
value="bills"
v-model="input_category"/>
<span class="bubble bills"></span>
<div>Bills</div>
</label>

</div>

<div class="todo-btn-block">
<input class="todo-btn" type="submit" value="Add todo" />
</div>
</form>


<div class="title-block">
<h1 class="title">To do list</h1>
</div>
<div class="todo-list">
<div
v-for="todo in todos_asc"
:class="`todo-item ${todo.done && 'done'}`">




<div class="todo-content">
<input type="text" v-model="todo.content" />
</div>

<div class="actions">
<button class="delete" @click="removeTodo(todo)">Delete</button>
</div>
</div>
</div>

</div>
</template>

<style>
.flex-title{
display: flex;
place-items: center;
}


.title-input{
border: none;
font-size: 24px ;
letter-spacing: 2px;
padding: 1rem;

}


.title-block{
text-align: center;
 
}
.title{
font-weight: 300;
letter-spacing: 2px;
text-transform: capitalize;
}
.hero{
display: grid;
background: url('../assets/img/bg-home.jpg');
background-size: cover;
background-position: center;
height: 600px;
}


.input-todo{
padding: 1rem;
border-radius: 7px;
border: solid 1px;
width: 100%;
}

.options-block{
    display: grid;
place-items: center;
place-content: center;
grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
gap:0.5em;
padding: 0.8rem 0;
}

.options-block label{
display: flex;
flex-direction: column;
place-items: center;
place-content: center;
padding: 1.5rem;
cursor: pointer;
}




input[type="radio"] {
display: none;
}

.bubble::after {
content: "";
display: block;
opacity: 0;
width: 0px;
height: 0px;
background-color: #3A82EE;
border-radius: 50%;
transition: 0.2s ease-in-out;
}

.bubble.personal::after {
background-color: #00b7ff;
}
.bubble.date::after {
background-color: #ee3aa3;
}
.bubble.grocerys::after {
background-color: #3aee67;
}
.bubble.bills::after {
background-color: red;
}

input:checked ~ .bubble::after {
width: 10px;
height: 10px;
opacity: 1;
}

.todo-btn-block{
margin: auto;
display: grid;
place-content: center;
max-width: 968px;
}
.todo-btn{
background-color: #3A82EE;
border: none;
color: #fff;
padding: 10px;
border-radius: 7px;
cursor: pointer;
transition: 0.5s ease-in-out;
}

.todo-btn:hover{
opacity: 0.75;
}

.todo-list  {
margin: 1rem 0;
} 

.todo-item {
display: flex;
place-items: center;

padding: 1rem;
border-radius: 0.5rem;
box-shadow:0 1px 3px #0000001a;
margin-bottom: 1rem;
}

.todo-item label {
display: block;
margin-right: 1rem;
cursor: pointer;
}

.todo-content {
flex: 1 1 0%;}

.todo-content input{
  font-size: 1.125rem;
  border: none ;
}

.delete{
border: none;
background-color: red;
color: #fff;
padding: 10px;
border-radius: 7px;
cursor: pointer;
}

@media(max-width:768px){
.hero{
height: 240px;
}

.title{
font-size: 28px;
}

.flex-title{
flex-direction: column;
}

.title-input{
font-size: 16px;
text-align: center;
width: 100%;
}
}
@media(max-width:390px){
.hero{
height: 200px;
}
}
</style>

