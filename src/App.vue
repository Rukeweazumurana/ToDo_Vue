<template>
  <div id="app">
    <h1>My Vue Todo Application</h1>
    <form @submit.prevent="addTodo">
      <input type="text" v-model="newTodo" placeholder="Enter Task" />
      <button type="submit">Add Task</button>
    </form>
    <table>
      <tr>
        <th style="width:64%">Task</th>
        <th style="width:12%">Delete</th>
        <th style="width:12%">Edit</th>
        <th style="width:12%">Save</th>
      </tr>
      <tr v-for="(todo, index) in todos" :key="index">
        <td><span v-if="!todo.isEditing">{{ todo.title }}</span>
          <input v-if="todo.isEditing" type="text" v-model="todo.title" /></td>
        <td><button @click="removeTodo(index)"><img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRcoiDAZqEvV4GDpMyNDsFgObegB_YHajx92Q&usqp=CAU" width="16px"/></button></td>
        <td><button @click="editTodo(index)"><img src="data:image/png;base64,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" width="16px"/></button></td>
        <td><button @click="saveTodo(index)"><img src="https://www.svgrepo.com/show/76243/save-icon.svg" width="16px"/></button></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      editingIndex: -1,
    };
  },
  methods: {
    addTodo() {
      this.todos.push({ title: this.newTodo, isEditing: false });
      this.newTodo = '';
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    editTodo(index) {
      this.todos[index].isEditing = true;
      this.editingIndex = index;
    },
    saveTodo(index) {
      this.todos[index].isEditing = false;
      this.editingIndex = -1;
    },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: -200px;
}
h1 {
  color: #000;
  font-size: 24px;
}
form input {
  height: 32px;
  width: 300px;
}
form button {
  background-color: #5783db;
  color: #fff;
  text-decoration: bold;
  border: 1px solid #e3e3e3;
  border-radius: 0px;
}
table{
  margin-top:16px;
}
table, tr, th, td{
  border:1px solid #e3e3e3;
  border-collapse:collapse;
}
table button{
  border-radius:0px;
  width:100%;
  height:100%;
}
</style>
