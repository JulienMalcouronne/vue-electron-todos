
<template>

<div>
  <el-row>
    <el-col :span="12" :offset="9" style="width: 100%">
      <h1>Todos</h1>
    </el-col>
  </el-row>
</div>
</template>

<script lang="ts">
export default {
  name: 'TodoList',
  inheritAttrs: false,
  customOptions: {}
}
</script>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { ElMessage } from 'element-plus'
import axios from 'axios'

interface Todo {
  id?: number | string;
  title: string;
  completed: boolean;
}

const todos = ref<Todo[]>([])

onMounted(async () => {
  await loadTodos()
})

// eslint-disable-next-line @typescript-eslint/explicit-function-return-type
const loadTodos = async () => {
  try {
    const response = await axios.get('http:localhost:8000/todos')
    todos.value = response.data;

  } catch (error) {
    ElMessage.error(error.message)
  }
}

// eslint-disable-next-line @typescript-eslint/explicit-function-return-type
const createTodo = async (todo: Todo) => {
  try {
    await axios.post('http:localhost:8000/todos', {
      title: todo.title,
      completed: todo.completed
    })
    ElMessage({
      message: 'Todo successfully created',
      type: 'success'
    })
  } catch (error) {
    ElMessage.error(error.message)
  }
  // refacto with store
  await loadTodos();
}

const updateTodo = async (todo: Todo) => {
  try {
    await axios.put(`http:localhost:8000/todos/${todo.id}`, {
      title: todo.title,
      completed: todo.completed
    })
    ElMessage({
      message: 'Todo successfully created',
      type: 'success'
    })
  } catch (error) {
    ElMessage.error(error.message)
  }
  // refacto with store
  await loadTodos();
}

// eslint-disable-next-line @typescript-eslint/explicit-function-return-type
const deleteTodo = async (todo: Todo) => {

  try {
    await axios.delete(`http://localhost:8000/todos/${todo.id}`)
    ElMessage({
      message: 'Todo successfully deleted',
      type: 'success'
    })
  } catch (error) {
    ElMessage.error(error.message)
  }
  // refacto with store
  await loadTodos();
}

// eslint-disable-next-line @typescript-eslint/explicit-function-return-type
const cancelDelete = () => {
  ElMessage({
    message: 'Delete cancelled',
    type: 'info'
  })
}
</script>
