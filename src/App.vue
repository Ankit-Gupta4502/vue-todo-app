
<script>
import HelloWorld from './components/HelloWorld.vue'
import { toRaw,onMounted } from 'vue';
export default {
  data() {
    return {
      todosList: [],
      todo: '',
      todos: [],
      edit: false,
      editId: '',
    }
  },

  components:{
    HelloWorld
  },

  methods: {
    addTodo() {
      this.todos = [...this.todos, { name: this.todo, id: Math.random() }]
      this.todo = ''
    },
    removeTodo(id) {
      this.todos = this.todos.filter((item) => item.id !== id)
    },
    editItem(id) {
      const editText = this.todos.find((item) => item.id === id)
      this.todo = editText.name
      this.edit = true
      this.editId = id
    },

    addEditeditem() {
      this.todos = this.todos.map((item) => {
        if (item.id === this.editId) {
          return { ...item, name: this.todo }
        }
        return item
      })
      this.edit = false
    },
  },

  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos')
      .then((res) => res.json())
      .then((res) => {
        this.todosList = res
      })
      .then((res) => console.log(toRaw(this.todosList), 'todoslist'))
  },
}
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!"  />
    </div>
  </header>

  <main>
    <div class="add-todo-container" >
      <input type="text" v-model="todo" class="todo-input" />
      <button class="btn" @click="(index) => addTodo(index)" v-if="!this.edit">Add</button>
      <button class="btn" v-else v-on:click="addEditeditem">Edit</button>
    </div>

    <div class="todo-list-container">
      <span v-for="(item, index) in todos" class="flex justify-between" :key="index">
        <span>
          {{ item.name }}
        </span>

        <span>
          <button class="btn" @click="(e) => editItem(item.id,)">Edit</button>
        </span>

        <span>
          <button class="btn" @click="() => removeTodo(item.id)">Remove</button>
        </span>
      </span>
    </div>

    <router-link to="/about">

      <h5>About</h5>
    </router-link>

    <router-view></router-view>

  </main>
</template>



<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

main {
  width: 50%;
}

.flex {
  display: flex;
}

.mb-3 {
  margin-bottom: 1.5rem;
}

.justify-between {
  justify-content: space-between;
}

.todo-list-container {
  margin-top: 2rem;
}

span {
  display: block;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.todo-input {
  padding: 0.7rem;
  border: 1px solid hsla(160, 100%, 37%, 1);
  width: 100%;
}

.add-todo-container {
  display: flex;
  align-items: stretch;
}

.btn {
  background-color: hsla(160, 100%, 37%, 1);
  color: #fff;
  padding-inline: 1rem;
  border: none;
}

.todo-input:focus {
  outline: none;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
