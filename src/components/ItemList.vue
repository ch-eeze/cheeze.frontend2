<script setup lang="ts">
import { ref } from 'vue'
import { computed } from 'vue'

interface Item {
  name: string
  price: number
}

const items = ref<Item[]>([
  { name: 'たまご', price: 100 },
  { name: 'りんご', price: 160 }
])
const newItemName = ref('')
const newItemPrice = ref(0)

const addItem = () => {
    if (newItemName.value!='') {
      items.value.push({ name: newItemName.value, price: newItemPrice.value })    
    }
  newItemName.value = ''
  newItemPrice.value = 0
}

interface Task {
    name: string
    deadline: string
    status: boolean
}

const tasks = ref<Task[]>([
    { name: '数学のレポート', deadline: '20日', status: false}
])
const newTaskName = ref('')
const newTaskDeadline = ref('')

const addTask = () => {
    if (newTaskName.value !='') {
        tasks.value.push({ name: newTaskName.value, deadline: newTaskDeadline.value, status: false})
    }
  newTaskName.value = ''
  newTaskDeadline.value = ''
}

const undoneTasks = computed(() => tasks.value.filter(task => task.status === false))
const doneTasks = computed(() => tasks.value.filter(task => task.status === true))


const finishTask = (taskName: string) => {
  // タスク名がtaskNameのタスクを完了済みにする
  tasks.value = tasks.value.map((task) => {
    if (task.name === taskName) {
      return {
        ...task,
        status: true
      }
    }
    return task
  })}
</script>

<template>
    <div>
      <div>ItemList</div>
      <ul>
        <li v-for="item in items" :key="item.name" :class="{ over500: item.price >= 500 }">
            <div>名前: {{  item.name  }}</div>
            <div>{{ item.price  }} 円</div>
            <div v-if="item.price >= 1000">高額商品</div>
        </li>
      </ul>
      <div>
        <label>
            名前
              <input v-model="newItemName" type="text" />
        </label>
        <label>
            価格
              <input v-model="newItemPrice" type="number" />
        </label>
        <button @click="addItem">追加</button>
      </div>
      <div>TodoList</div>
      <div>Undone</div>
        <ul>
            <li v-for="task in undoneTasks" :key="task.name">
                <div><button @click="finishTask(task.name)"></button>タスク名: {{ task.name }}</div>
                <div>締切: {{ task.deadline }}まで</div>
            </li>
        </ul>
      <div>Done</div>
        <ul>
            <li v-for="task in doneTasks" :key="task.name">
                <div><input type="checkbox" checked>タスク名: {{ task.name }}</div>
            </li>
        </ul>
      <div>
        <label>
            タスク名
              <input v-model="newTaskName" type="string" />
        </label>
        <label>
            締切
              <input v-model="newTaskDeadline" type="stirng" />
        </label>
        <button @click="addTask">Add</button>
      </div>
    </div>
</template>

<style>
.over500 {
  color: red;
}
</style>