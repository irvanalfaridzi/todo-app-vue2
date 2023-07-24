<template>
    <div id="todoApp" class="bg-slate-800 h-screen w-screen mx-auto px-20 py-5 font-mono">
        <h1 class="text-gray-300 text-4xl font-bold text-center pt-8">Todo App</h1>
        <form v-on:submit.prevent="addTodo" class="flex flex-col mt-10 w-auto">
            <label class="text-base font-semibold text-gray-300 mb-1">New Todo</label>
            <input v-model="todoInput" autocomplete="off" class="mb-4 h-12 shadow-none outline-none px-3 border-2 border-gray-300 rounded-md bg-slate-800 text-gray-300" type="text" name="new-todo">
            <button class="h-12 shadow-none outline-none px-3 border-2 border-blue-300 rounded-md bg-blue-300 text-black font-bold text-lg">Add Todo</button>
        </form>
        

        <h3 class="mt-10 text-2xl text-gray-300 font-semibold border-b-2 border-gray-300">Todo List</h3>
        <ul class="mt-7">
            <li v-for="(todo, index) in todoList" v-bind:key="index" class="mb-4 h-14 shadow-none outline-none px-5 border-2 border-gray-300 rounded-md bg-slate-800 text-gray-300 flex justify-between items-center">
                <span v-on:click="doneTodo(todo)" :class="todo.done ? 'line-through' : 'no-underline'">{{ todo.content }}</span>
                <button v-on:click="removeTodo(index)" class="h-8 shadow-none outline-none px-3 border-2 border-blue-300 rounded-md bg-blue-300 text-black font-semibold text-base">Remove</button>
            </li>
            <label class="flex justify-center text-gray-600" v-show="todoList.length == 0">Empty List.</label>
        </ul>
    </div>
</template>

<script>
export default {
    data(){
        return{
            todoList: [],
            todoInput: null
        }
    },
    mounted() {
    if (localStorage.getItem('todo')) {
      try {
        let todoTemp = JSON.parse(localStorage.getItem('todo'));
        this.todoList = todoTemp;
      } catch(e) {
        localStorage.removeItem('todo');
      }
    } else {
        this.todoList = this.defaultTodo
    }
  },
    methods: {
        addTodo(){
            if (!this.todoInput) {
                return;
            }

            this.todoList.push({
                content: this.todoInput,
                done: false,
            });
            this.todoInput = '';
            this.saveTodo();
        },
        removeTodo(index) {
            this.todoList.splice(index, 1);
            this.saveTodo();
        },
        doneTodo(todo){
            todo.done = !todo.done;
            this.saveTodo();
        },
        saveTodo() {
            const parsed = JSON.stringify(this.todoList);
            localStorage.setItem('todo', parsed);
        }
    }
}
</script>