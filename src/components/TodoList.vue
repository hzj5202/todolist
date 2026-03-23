<template>
    <div class="todo-container">
        <h2>我的待办</h2>

        <!-- 添加任务 -->
        <div class="input-group">
            <input v-model="newTodo" @keyup.enter="addTodo" placeholder="输入新任务..." class="todo-input" />
            <button @click="addTodo" class="addbtn">添加</button>
        </div>
        <TransitionGroup name="list" tag="ul" class="list">
            <!-- 任务列表 -->
            <li v-for="(todo) in filteredTodos" :key="todo.id" :class="{ completed: todo.completed }"
                @click="toggleComplete(todo.id)" class="todo-item">
                <span>{{ todo.text }}</span>
                <button @click.stop="removeTodo(todo.id)" class="delete-btn">删除</button>
            </li>

            <!-- 操作按钮 -->
            <div class="actions">
                <p>共{{ todos.length }}项任务，完成了{{ todos.length - remainingCount }}项,还有{{ remainingCount }}项未完成</p>
                <button @click="clearCompleted">清除已完成</button>
                <div>
                    <button @click="filter = 'all'">全部</button>
                    <button @click="filter = 'completed'">已完成</button>
                    <button @click="filter = 'active'">未完成</button>
                </div>
            </div>
        </TransitionGroup>    
    </div>
</template>

<script setup lang="ts">
import { computed, ref, watchEffect } from 'vue';

//初始化任务数组
const todos = ref(loadFromLocalStorage())
// 过滤状态
const filter = ref('all') //'all'|'completed'|'active'

// 返回过滤后列表
const filteredTodos = computed(() => {
    switch (filter.value) {
        case 'completed':
            return todos.value.filter(todo => todo.completed)
        case 'active':
            return todos.value.filter(todo => !todo.completed)
        default:
            return todos.value
    }
})

// 新增任务
const newTodo = ref('')
function addTodo() {
    const text = newTodo.value.trim()
    if (text === '') return
    todos.value.push({
        id: Date.now(),
        text,
        completed: false
    })
    newTodo.value = ''
}
// 切换状态
function toggleComplete(id) {
    const todo = todos.value.find(item => item.id === id)
    todo.completed = !todo.completed
}
// 删除任务
function removeTodo(id) {
    todos.value = todos.value.filter(item => item.id !== id)
    // todos.value.splice(index,1)
}

//清除已完成
function clearCompleted() {
    todos.value = todos.value.filter(todo => !todo.completed)
}

//监听变化保存到localstorage
watchEffect(() => {
    saveToLocalStorage(todos.value)
})

// 计算未完成
const remainingCount = computed(() => {
    return todos.value.filter(todo => !todo.completed).length
})

// 本地存储
function saveToLocalStorage(data) {
    localStorage.setItem('todos', JSON.stringify(data))
}

function loadFromLocalStorage() {
    const data = localStorage.getItem('todos')
    return data ? JSON.parse(data) : []
}

</script>

<style scoped>
.todo-container {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    font-family: Arial;
}

.input-group {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

.todo-input {
    flex: 1;
    padding: 8px;
    font-size: 16px;
}

.addbtn {
    padding: 8px 12px;
    background-color: rgb(240, 249, 235);
    color: rgb(103, 194, 58);
    border: 0.5px solid rgb(179, 225, 157);
    cursor: pointer;
    border-radius: 4px;
}

.addbtn:hover {
    background-color: rgb(103, 194, 58);
    border-color: rgb(103, 194, 58);
    color: white;
}

.todo-item {
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    position: relative;
    display: flex;
    justify-content: space-between;
    min-width: 100px;
}
.todo-item span{
    max-width: 300px;   
    overflow-wrap: anywhere;
}

.todo-item.completed {
    background-color: #8080801A;
}

.todo-item.completed span {
    text-decoration: line-through;
    color: gray;
}

.delete-btn {
    background-color: rgb(254, 240, 240);
    color: rgb(245, 108, 108);
    border: 0.5px solid rgb(250, 182, 182);
    border-radius: 4px;
    cursor: pointer;
    width: 50px;
    height: 25px;
    margin: 10px;
}

.delete-btn:hover {
    background-color: rgb(245, 108, 108);
    border-color: rgb(245, 108, 108);
    color: white;
}


.actions {
    margin-top: 20px;
}

.actions button {
    padding: 8px 12px;
    margin: 5px;
    cursor: pointer;
    background-color: rgb(236, 245, 255);
    color: rgb(64, 158, 255);
    border: 0.5px solid rgb(160, 207, 255);
    border-radius: 4px;
}

.actions button:hover {
    background-color: rgb(64, 158, 255);
    border-color: rgb(64, 158, 255);
    color: white;
}

.list{
    max-width: 400px;
    padding: 10px;
    margin: 0 auto;
    position: relative;
}

.list-move,
.list-enter-active,
.list-leave-active {
  transition: all .2s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translate(30px);
}

.list-leave-active {
  position: absolute;
  width: 100%;
}
</style>