<template>
  <TodoList></TodoList>
  <!-- <TransitionGroup name="list" tag="ul" class="list">
    <li v-for="item in items" :key="item">
      {{ item }}
    </li>
    <button @click="add()">add</button>
    <button @click="remove()">remove</button>
  </TransitionGroup> -->
</template>

<script setup lang="ts">
// import { ref } from 'vue';
import TodoList from './components/TodoList.vue';

// const items = ref([1, 2, 3, 4, 5])
// function add() {
//   items.value.push(4)
//   items.value.push(4)
//   items.value.push(4)
//   items.value.push(4)
//   items.value.push(4)
// }
// function remove() {
//   items.value.splice(-1, 1)
// }

</script>

<style scoped>
/* .list {
  display: inline-block;

}

.list-move,
.list-enter-active,
.list-leave-active {
  transition: all 0.2s ease;
}

.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translate(30px);
}

.list-leave-active {
  position: absolute;
} */
</style>
