<template>
  <div>
    <h1>Todo List</h1>

    <!-- Display the Todo List -->
    <ul>
      <li v-for="(todo, index) in todos" :key="index">{{ todo.task }}</li>
    </ul>

    <!-- Form to Add a New Todo -->
    <h2>Add a new todo</h2>
    <input v-model="newTodo" placeholder="Enter new todo" />
    <button @click="addTodo">Submit</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      todos: [],     // Store the todo list
      newTodo: ''    // Store the new todo to be added
    };
  },
  mounted() {
    // Fetch the most up-to-date todo list when the component is mounted
    this.getTodos();
  },
  methods: {
    // Method to GET the list of todos using Axios
    async getTodos() {
      try {
        const response = await axios.get('https://6lzzhlthsj.execute-api.eu-north-1.amazonaws.com/dev');
        // If the response body is stringified JSON, parse it
        const data = JSON.parse(response.data.body);

        this.todos = data.items || [];  // Assuming the response structure is { "items": [...] }
      } catch (error) {
        console.error("Error fetching todos:", error);
      }
    },

    // Method to POST a new todo using Axios
    async addTodo() {
      try {
        // Send the new todo item to the backend
        const response = await axios.post('https://6lzzhlthsj.execute-api.eu-north-1.amazonaws.com/dev', {
          name: this.newTodo   // The name field matches your Lambda's expectation
        }, {
          headers: {
            'Content-Type': 'application/json'  // Ensure proper headers are sent
          }
        });

        if (response.status === 200) {  // Check for successful response
          this.newTodo = '';  // Reset the input field
          this.getTodos();    // Fetch the updated todo list after adding a new one
        } else {
          console.error('Error adding todo:', response);
        }
      } catch (error) {
        console.error('Error adding todo:', error);
      }
    }
  }
};
</script>
