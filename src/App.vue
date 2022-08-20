<script>
export default {
  data() {
    return {
      newItem: "",
      todos: [],
      editingItemId: false,
    };
  },
  methods: {
    clickTodo(id) {
      const todos = [...this.todos];

      for (let todo of todos) {
        if (todo.id === id) {
          todo.completed = !todo.completed;
        }
      }

      this.todos = todos;
    },
    addItem() {
      const todos = [...this.todos];
      
      if (this.editingItemId) {
        const todo = todos.find((todo) => todo.id === this.editingItemId);
        todo.title = this.newItem;
        this.editingItemId = null;

        this.todos = todos;
        this.newItem = "";
      } else {
        const lastTodo = todos[todos.length - 1];
        const id = lastTodo ? lastTodo.id + 1 : 1;
        todos.push({
          id,
          title: this.newItem,
          completed: false,
        });
        this.newItem = "";
        this.todos = todos;
      }
    },
    removeItem(id) {
      const todos = [...this.todos];
      const filteredTodos = todos.filter((todo) => todo.id !== id);
      this.todos = filteredTodos;
    },
    editItem(id) {
      const todos = [...this.todos];
      const filteredTodos = todos.filter((todo) => todo.id === id);
      this.newItem = filteredTodos[0].title;
      this.editingItemId = id;
    },
  },
  mounted() {
    this.todos = localStorage.getItem("todos")
      ? JSON.parse(localStorage.getItem("todos"))
      : [];
  },
  watch: {
    todos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
  }
};
</script>

<template>
  <div class="min-h-screen w-full bg-slate-100 flex justify-center items-center">
    <div class="bg-white flex-1 max-w-md rounded-xl shadow-xl overflow-hidden ring-4 ring-slate-100">
      <input
        type="text"
        class="bg-slate-600 text-white p-6 w-full outline-none text-3xl"
        placeholder="Type something..."
        v-model="newItem"
        @keydown.enter="addItem"
      />
      <ul>
        <li
          @click.exact="clickTodo(todo.id)"
          @click.shift="removeItem(todo.id)"
          @contextmenu.prevent="editItem(todo.id)"
          :class="{
            'bg-slate-300': todo.completed,
            'text-slate-400': todo.completed,
            'hover:bg-blue-500': !todo.completed,
            'hover:text-white': !todo.completed,
          }"
          :key="todo.id"
          v-for="todo in todos"
          class="p-6 text-3xl transition border-b border-slate-100 bg-slate-200 text-slate-600 cursor-pointer"
        >
          <span :class="{ 'line-through': todo.completed }">{{ todo.title }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>
