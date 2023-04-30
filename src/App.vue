<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form @submit.prevent="addToDo">
          <h4>What's on your ToDo List</h4>
          <input type="text" placeholder="e.g. make a Video" v-model="input_content" />
          <h4>Pick a Category</h4>
          <div class="options">
            <label>
              <input type="radio" name="category" value="business" v-model="input_category"/>
              <span class="bubble business"></span>
              <div>Business</div>
            </label>

            <label>
              <input type="radio" name="category" value="personal" v-model="input_category"/>
              <span class="bubble personal"></span>
              <div>Personal</div>
            </label>

          </div>

          <input type="submit" value="Add todo"/>
      </form>
    </section>

    <section class="todo-list">
      <h3>ToDO List</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :key="todo.content" :class="`todo-item ${todo.done && 'done'}`">
            <label>
              <input type="checkbox" v-model="todo.done" />
              <span :class="`bubble ${todo.category}`"></span>
            </label>
            
            <div class="todo-content">
              <input type="text" v-model="todo.content" />
            </div>

            <div class="actions">
              <button class="delete" @click="removeTodo(todo)">Remove</button>
            </div>
        </div>
      </div>
    </section>

    {{ todos_asc }}
  </main>
</template>

<script>
  export default({

    computed:{
      todos_asc(){
        return this.sortData();
      }
    },
    methods:{
      sortData(){
        return this.todos.sort((a,b) => {
          return a.createdAt - b.createdAt
        })
      },

      addToDo(){
        if(this.input_category === '' || this.input_category == null)
        {
          return;
        }

        this.todos.push({
          content: this.input_content,
          category: this.input_category,
          done: false,
          createdAt: new Date().getTime()
        })

        console.log(this.todos)

        this.input_content = '';
        this.input_category = null;
      },

      removeTodo(todo){
        this.todos = this.todos.filter(t => t != todo);
      }


    },

    watch:{
      name(newName){
        localStorage.setItem('name',newName);
      },

      todos:{
        handler(newTodos){
          localStorage.setItem('todos',JSON.stringify(newTodos));
        }, deep:true
      }
    },

    mounted(){
      this.name = localStorage.getItem('name');
      this.todos = JSON.parse(localStorage.getItem('todos')) || []
    },
    
    data:function(){
      return{
        todos :[],
        name: '',
        input_content:'',
        input_category: null
      }
    }
  })
</script>

<style>
</style>
