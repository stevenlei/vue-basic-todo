<script>
export default {
  data() {
    return {
      todos: [
        {
          id: 1,
          title: 'do something',
          completed: true
        },
        {
          id: 2,
          title: 'do something else',
          completed: false
        }
      ],
    };
  },
  
};
</script>

<template>
  <div class="min-h-screen w-full bg-slate-100 flex justify-center items-center">
    <div class="bg-white flex-1 max-w-md rounded-xl shadow-xl overflow-hidden ring-4 ring-slate-100">
      <input type="text" class="bg-slate-600 text-white p-6 w-full outline-none text-3xl" placeholder="Type something...">
      <ul>
        <li :class="{'bg-slate-300': todo.completed, 'text-slate-400': todo.completed, 'hover:bg-blue-500': !todo.completed, 'hover:text-white': !todo.completed}" :key="todo.id" v-for="todo in todos" class="p-6 text-3xl transition border-b border-slate-100 bg-slate-200 text-slate-600 cursor-pointer">
          <span :class="{'line-through': todo.completed}">{{ todo.title }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>
