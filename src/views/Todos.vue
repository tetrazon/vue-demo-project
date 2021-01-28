<template>
    <div>
        <h2>Hello ololo</h2>
        <router-link to="/">Home</router-link>
        <hr>
        <AddTodo
                @add-todo="addTodo"
        ></AddTodo>
        <select v-model="filter">
            <option value="all">all</option>
            <option value="completed">completed</option>
            <option value="not-completed">not completed</option>

        </select>
        <hr>
        <Loader v-if="loading"/>
        <TodoList
                v-else-if="filteredTodos.length"
                v-bind:todos="filteredTodos"
                @remove-todo="removeTodo"
        />
        <p v-else>No todos!!</p>

    </div>
</template>

<script>

    import TodoList from '@/components/TodoList'
    import AddTodo from '@/components/AddTodo'
    import Loader from '@/components/Loader'
    export default {
        name: 'App',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        methods: {
            removeTodo(id) {
                console.log(id)
                this.todos = this.todos.filter(t => t.id !== id)
            },
            addTodo(todo) {
                this.todos.push(todo)
            }
        },
        mounted() {
            fetch('https://jsonplaceholder.typicode.com/todos?_limit=3')
                .then(response => response.json())
                .then(json => {
                    setTimeout(() => {
                        this.todos = json
                        this.loading = false
                    }, 1000)

                })
        },
        // watch:{
        //     filter(value) {
        //         console.log(value)
        //     }
        // },
        computed: {
            filteredTodos() {
                switch (this.filter) {
                    case 'all':
                        return this.todos
                    case 'completed':
                        return this.todos.filter(a => a.completed)
                    case 'not-completed':
                        return this.todos.filter(a => !a.completed)
                }
            }
        },
        components: {
            TodoList,
            AddTodo,
            Loader
        }
    }
</script>