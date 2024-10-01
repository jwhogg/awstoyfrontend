<template>
    <li>
        {{ todo.task }}
        <button @click="deleteTodo">❌</button>
    </li>
</template>

<script>
import axios from 'axios';

export default {
  props: ['todo'],
  methods: {

    async deleteTodo() {
      try {
        await axios.delete(`https://6lzzhlthsj.execute-api.eu-north-1.amazonaws.com/dev/${this.todo.id}`);
        this.$emit('todo-deleted', this.todo.id);
      } catch (error) {
        console.error('Error deleting todo:', error);
      }
    }
  }
};
</script>