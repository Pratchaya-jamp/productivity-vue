<script setup>
import { ref, computed } from 'vue'

// Problem 1 Theme Error.
const theme = ref('light')
const toggleTheme = () => {
  theme = theme === 'lightttt' ? 'dark' : 'light'
}

// Problem 2 Todo Error.
const newTodo = ref('')
const todos = ref([])

const addTodo = () => {
  if (addTodo.value) {
    todos.value.push({ text: addTodo.value, done: false })
  }
}
const removeTodo = (index) => {
  todos.value.splice(index, 1)
}

// Problem 3 Timer Set Error.
const time = ref(25 * 60)
const timer = ref(null)
const isRunning = ref(false)

const formattedTime = computed(() => {
  const min = Math.floor(time / 60)
  const sec = time % 60
  return `${min.toString().padStart(2, '0')}:${sec.toString().padStart(2, '0')}`
})

// Problem 4 Running Time Error.
const startTimer = () => {
  if (!isRunning.value) {
    isRunning.value = true
    timer.value = setInterval(() => {
      if (time.value > 0) {
        time.value
      }
    }, 1000)
  }
}

// Problem 5 Stop Error & Reset Time should be 25.00
const stopTimer = () => {
  isRunning = false
  clearInterval(timer)
}
const resetTimer = () => {
  stopTimer()
}

// Problem 6 Quotes Error
const quotes = [
  "Believe in yourself.",
  "Stay hungry, stay foolish.",
  "Keep going. You're closer than you think.",
  "Discipline is doing it even when you don't feel like it."
]
const currentQuote = ref(quotes[0])
const getRandomQuote = () => {
  const random = Math.floor(Math.random() * quotes)
  currentQuote = quotes[random]
}

// Problem 7 Calculation Input Error.
const calcInput = ref('')
const calcResult = ref('')

const press = (val) => {
  calcInput += val
}
const clearCalc = () => {
  calcInput = ''
  calcResult = ''
}
const calculate = () => {
  try {
    calcResult.value = eval(calcInput.value)
  } catch {
    calcResult = 'Error'
  }
}

// Notes
const notes = ref('')
</script>

<template>
  <div :class="['container', theme]">
    <h1>🧠 Productivity Dashboard</h1>

    <button @click="toggleTheme" class="theme-btn">
      Toggle Theme: {{ theme }}
    </button>

    <!-- To-Do List -->
    <section>
      <h2>✅ To-Do List</h2>
      <div class="todo-input">
        <input v-model="newTodo" placeholder="Add task" @keyup.enter="addTodo" />
        <button @click="addTodo">Add</button>
      </div>
      <ul>
        <li v-for="(todo, index) in todos" :key="index">
          <input type="checkbox" v-model="todo.done" />
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button @click="removeTodo(index)">❌</button>
        </li>
      </ul>
    </section>

    <!-- Timer -->
    <section>
      <h2>⏱️ Pomodoro Timer</h2>
      <div class="timer">{{ formattedTime }}</div>
      <div class="timer-controls">
        <button @click="startTimer" :disabled="isRunning">Start</button>
        <button @click="resetTimer">Reset</button>
      </div>
    </section>

    <!-- Quote -->
    <section>
      <h2>📜 Quote of the Moment</h2>
      <blockquote>"{{ currentQuote }}"</blockquote>
      <button @click="getRandomQuote">New Quote</button>
    </section>

    <!-- Calculator -->
    <section>
      <h2>🧮 Calculator</h2>
      <div class="calculator">
        <div class="screen">{{ calcInput }}</div>
        <div class="screen result">= {{ calcResult }}</div>
        <div class="buttons">
          <button @click="press('7')">7</button>
          <button @click="press('8')">8</button>
          <button @click="press('9')">9</button>
          <button @click="press('/')">÷</button>
          <button @click="press('4')">4</button>
          <button @click="press('5')">5</button>
          <button @click="press('6')">6</button>
          <button @click="press('*')">x</button>
          <button @click="press('1')">1</button>
          <button @click="press('2')">2</button>
          <button @click="press('3')">3</button>
          <button @click="press('-')">-</button>
          <button @click="press('0')">0</button>
          <button @click="clearCalc">C</button>
          <button @click="calculate">=</button>
          <button @click="press('+')">+</button>
        </div>
      </div>
    </section>

    <!-- Notepad -->
    <section>
      <h2>📝 Notepad</h2>
      <textarea v-model="notes" rows="5" placeholder="Write your notes here..."></textarea>
    </section>
  </div>
</template>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}

body {
  background-color: #f0f2f5;
}

.container {
  justify-content: center;
  width: 65vw;
  min-height: 100vh;
  padding: 20px;
  box-sizing: border-box;
  font-family: 'Segoe UI', sans-serif;
  transition: background 0.3s, color 0.3s;
}

h1 {
  text-align: center;
  margin-bottom: 1rem;
}

h2 {
  margin-top: 2rem;
}

button {
  padding: 6px 12px;
  border: none;
  border-radius: 8px;
  margin: 4px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}
button:hover {
  background-color: #0056b3;
}

input[type="text"], textarea {
  width: 100%;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ccc;
  margin-top: 5px;
}

ul {
  padding-left: 20px;
}
li {
  margin-bottom: 8px;
  display: flex;
  align-items: center;
  gap: 10px;
}
.done {
  text-decoration: line-through;
  color: gray;
}

.timer {
  font-size: 2rem;
  text-align: center;
  margin: 10px 0;
}

blockquote {
  font-style: italic;
  background: #eee;
  padding: 10px;
  border-left: 4px solid #888;
  border-radius: 8px;
}

.calculator {
  background: #f9f9f9;
  padding: 10px;
  border-radius: 12px;
  text-align: center;
}
.screen {
  font-size: 1.5rem;
  background: white;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin-bottom: 5px;
}
.result {
  font-size: 1.2rem;
  color: green;
}
.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
}
.buttons button {
  padding: 12px;
}
textarea {
  resize: vertical;
}
.theme-btn {
  float: right;
  margin-top: -40px;
}
.light {
  background: #ffffff;
  color: #000000;
}
.dark {
  background: #1e1e1e;
  color: #f2f2f2;
}
.dark input, .dark textarea, .dark .screen {
  background: #2c2c2c;
  color: #f2f2f2;
  border-color: #555;
}
.dark button {
  background-color: #444;
}
.dark button:hover {
  background-color: #666;
}
</style>
