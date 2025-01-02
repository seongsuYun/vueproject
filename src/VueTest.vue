<script setup>
import { ref, onMounted, reactive, computed, watch } from 'vue'
import childComp from './ChildTest.vue'

// 반응적인 상태의 속성
const count = ref(0)
const dataSetTmp = reactive([])
const titleClass = ref('type01')
const textValue = ref('')
const isDisplay = ref(true)
const hideCompleted = ref(false)
const rHText = ref(null)
const helloText = ref('안녕하세요. 저는 Props로 데이터를 넘길수 있어요')

let id = 0
const newTodo = ref('')
const todos = ref([
  {id: id++, text: '샘플1', done: true},
  {id: id++, text: '샘플2', done: true},
  {id: id++, text: '샘플3', done: false}
])

const addTodo = function() {
  todos.value.push({id: id++, text: newTodo.value})
  newTodo.value = ''
}

const removetodo = function(todo) {
  todos.value = todos.value.filter((item) => item !== todo)
}

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter((item) => !item.done) : todos.value
})

// 속성 값을 변경하고 업데이트 할 수 있는 함수.
function increment() {
  count.value++
}

const fetchData = async function(newValue) {
  console.log(`새로바뀐 값은 ${newValue}`);
}

watch(count, fetchData)



// 생명 주기 훅
onMounted(() => {
  console.log(`숫자 세기의 초기값은 ${ count.value } 입니다.`)

  let dummy = [
    {code : '01', text : '샘플1'},
    {code : '02', text : '샘플2'}
  ]

  dataSetTmp.push(dummy)

  rHText.value.textContent = "마운트된 후 값입니다.";
})

// const onInput = function(e) {
//   textValue.value = e.target.value;
// }
</script>

<template>
  <button @click="increment">숫자 세기: {{ count }}</button>
  <h1 :class="titleClass">기본연습중</h1>
  <p ref="rHText">기본연습중</p>

  <input v-model="textValue" placeholder="입력하세요." />
  <p v-if="isDisplay">{{textValue}}</p>


  <form @submit.prevent="addTodo">
      <input v-model="newTodo" />
      <button>할일추가</button>
  </form>
  <ul>
      <li v-for="item in filteredTodos" :key="item.id">
        <input type="checkbox" v-model="item.done" />
        <span :class="{done: item.done}">{{ item.text }}</span>
        <button @click="removetodo(item)">삭제하기</button>
      </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted">{{ hideCompleted ? '전체보이기' : '숨기기' }}</button>

  <childComp :msg="helloText"/>

</template>

<style>
.type01 {
  color: red;
}

.done {
  text-decoration: line-through;
}
</style>

!!!https://v3-docs.vuejs-korea.org/tutorial/#step-8여기 해야됨