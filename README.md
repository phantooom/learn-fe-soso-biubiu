# 前端快速入门 - 一天学习计划

> 适合有后端经验(Python/Go)的开发者,目标是快速掌握 Vue 3 + 组件库开发

## 📚 一天学习计划

### 上午 (3-4小时) - 理论学习

#### 第一阶段: HTML/CSS/JS 最小必要知识 (1小时)

**HTML - 只需了解这些标签:**
```html
<div>, <span>, <button>, <input>, <form>, <a>, <img>, <ul>, <li>
```

**CSS - 核心概念:**
- 盒模型: margin、padding、border
- Flex布局: 理解 `display: flex`, `justify-content`, `align-items`
- 类选择器: `.class-name`

**JavaScript - 必备语法:**
```javascript
// 变量声明
const name = 'Vue'
let count = 0

// 箭头函数 (类似 Python lambda)
const add = (a, b) => a + b

// 解构赋值 (类似 Python unpacking)
const { name, age } = user
const [first, second] = array

// Promise 和 async/await (类似 Python asyncio)
async function fetchData() {
  const response = await fetch('/api/data')
  return response.json()
}

// 模板字符串
const message = `Hello ${name}`
```

**学习资源:**
- MDN 快速入门: https://developer.mozilla.org/zh-CN/docs/Learn

---

#### 第二阶段: Vue 3 核心概念 (2-3小时)

**1. 响应式基础**
```vue
<script setup>
import { ref, reactive, computed } from 'vue'

// ref: 基本类型响应式
const count = ref(0)
count.value++ // 修改需要 .value

// reactive: 对象响应式
const user = reactive({
  name: 'John',
  age: 25
})
user.name = 'Jane' // 直接修改

// computed: 计算属性 (类似 Python @property)
const doubleCount = computed(() => count.value * 2)
</script>

<template>
  <div>{{ count }} - {{ doubleCount }}</div>
</template>
```

**2. 模板语法**
```vue
<template>
  <!-- 条件渲染 (类似 Jinja2 {% if %}) -->
  <div v-if="isLogin">已登录</div>
  <div v-else>未登录</div>

  <!-- 列表渲染 (类似 {% for %}) -->
  <ul>
    <li v-for="item in items" :key="item.id">
      {{ item.name }}
    </li>
  </ul>

  <!-- 双向绑定 -->
  <input v-model="message" />

  <!-- 事件绑定 -->
  <button @click="handleClick">点击</button>

  <!-- 属性绑定 -->
  <img :src="imageUrl" />
</template>
```

**3. 组件通信**
```vue
<!-- 父组件 -->
<template>
  <ChildComponent
    :message="parentMsg"
    @update="handleUpdate"
  />
</template>

<!-- 子组件 -->
<script setup>
const props = defineProps({
  message: String
})

const emit = defineEmits(['update'])

const sendToParent = () => {
  emit('update', 'data from child')
}
</script>
```

**4. 生命周期钩子**
```javascript
import { onMounted, onUnmounted } from 'vue'

onMounted(() => {
  console.log('组件挂载完成')
})

onUnmounted(() => {
  console.log('组件卸载')
})
```

**Vue 3 官方文档:**
- 快速开始: https://cn.vuejs.org/guide/quick-start.html
- 组合式API: https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html

---

#### 第三阶段: Vue Router 和 Pinia (30分钟)

**Vue Router (路由):**
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  { path: '/', component: Home },
  { path: '/todos', component: TodoList },
  { path: '/weather', component: Weather }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})
```

**Pinia (状态管理):**
```javascript
// stores/todo.js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useTodoStore = defineStore('todo', () => {
  const todos = ref([])

  const addTodo = (todo) => {
    todos.value.push(todo)
  }

  return { todos, addTodo }
})
```

---

### 下午 (3-4小时) - 实战作业

## 🎯 作业: Todo管理系统 + 天气查询

### 功能要求

#### 基础功能 (必做)

**1. Todo List 页面**
- ✅ 添加任务 (输入框 + 按钮)
- ✅ 删除任务
- ✅ 标记任务完成/未完成 (checkbox)
- ✅ 任务分类 (工作/生活/学习 - 下拉选择)
- ✅ 任务统计 (总数/已完成/未完成)
- ✅ 数据持久化 (使用 localStorage)

**2. 天气查询页面**
- ✅ 城市输入框 + 查询按钮
- ✅ 调用天气API显示结果
- ✅ 显示: 城市、温度、天气状况、天气建议
- ✅ 加载状态提示
- ✅ 错误处理

**3. 路由导航**
- ✅ 首页 `/` (欢迎信息 + 项目介绍)
- ✅ Todo页 `/todos`
- ✅ 天气页 `/weather`
- ✅ 顶部导航栏

#### 进阶功能 (选做)

- 🌟 Todo 支持编辑
- 🌟 Todo 添加截止日期
- 🌟 天气页面支持城市收藏
- 🌟 深色模式切换
- 🌟 响应式设计(移动端适配)
- 🌟 添加动画效果

---

### 技术栈

```json
{
  "框架": "Vue 3 (Composition API)",
  "路由": "Vue Router 4",
  "状态管理": "Pinia",
  "UI组件库": "Element Plus",
  "HTTP请求": "Axios",
  "构建工具": "Vite"
}
```

---

### 项目结构

```
learn-fe-soso-biubiu/
├── public/
├── src/
│   ├── assets/          # 静态资源
│   ├── components/      # 可复用组件
│   │   ├── TodoItem.vue
│   │   └── WeatherCard.vue
│   ├── views/           # 页面组件
│   │   ├── Home.vue
│   │   ├── TodoList.vue
│   │   └── Weather.vue
│   ├── stores/          # Pinia 状态管理
│   │   └── todo.js
│   ├── router/          # 路由配置
│   │   └── index.js
│   ├── utils/           # 工具函数
│   │   └── storage.js   # localStorage 封装
│   ├── App.vue          # 根组件
│   └── main.js          # 入口文件
├── package.json
└── vite.config.js
```

---

### 快速开始

#### 1. 创建项目

```bash
# 使用 Vue 官方脚手架
npm create vue@latest

# 选择配置:
# ✅ Vue Router
# ✅ Pinia
# ❌ TypeScript (先不用)
# ❌ JSX
# ❌ Vitest
# ✅ ESLint

cd learn-fe-soso-biubiu
npm install
```

#### 2. 安装 Element Plus

```bash
npm install element-plus
npm install @element-plus/icons-vue
```

在 `main.js` 中引入:
```javascript
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'
import router from './router'
import { createPinia } from 'pinia'

const app = createApp(App)
app.use(createPinia())
app.use(router)
app.use(ElementPlus)
app.mount('#app')
```

#### 3. 安装 Axios

```bash
npm install axios
```

#### 4. 启动开发服务器

```bash
npm run dev
```

---

### 天气API推荐

**免费天气API (无需注册):**

1. **天气API (推荐):**
   - URL: `https://www.tianqiapi.com/free/day`
   - 参数: `?appid=23035354&appsecret=8YvlPNrz&city=城市名`
   - 示例: https://www.tianqiapi.com/free/day?appid=23035354&appsecret=8YvlPNrz&city=北京

2. **或使用 OpenWeatherMap (需注册):**
   - 网站: https://openweathermap.org/api
   - 免费额度: 1000次/天

---

### 核心代码示例

#### LocalStorage 封装 (`utils/storage.js`)

```javascript
export const storage = {
  get(key) {
    const value = localStorage.getItem(key)
    try {
      return JSON.parse(value)
    } catch {
      return value
    }
  },

  set(key, value) {
    localStorage.setItem(key, JSON.stringify(value))
  },

  remove(key) {
    localStorage.removeItem(key)
  }
}
```

#### Todo Store (`stores/todo.js`)

```javascript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import { storage } from '@/utils/storage'

export const useTodoStore = defineStore('todo', () => {
  // 状态
  const todos = ref(storage.get('todos') || [])

  // 计算属性
  const totalCount = computed(() => todos.value.length)
  const completedCount = computed(() =>
    todos.value.filter(t => t.completed).length
  )

  // 方法
  const addTodo = (todo) => {
    todos.value.push({
      id: Date.now(),
      ...todo,
      completed: false,
      createdAt: new Date().toISOString()
    })
    saveTodos()
  }

  const deleteTodo = (id) => {
    todos.value = todos.value.filter(t => t.id !== id)
    saveTodos()
  }

  const toggleTodo = (id) => {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      todo.completed = !todo.completed
      saveTodos()
    }
  }

  const saveTodos = () => {
    storage.set('todos', todos.value)
  }

  return {
    todos,
    totalCount,
    completedCount,
    addTodo,
    deleteTodo,
    toggleTodo
  }
})
```

#### 天气查询示例 (`views/Weather.vue`)

```vue
<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { ElMessage } from 'element-plus'

const city = ref('')
const weather = ref(null)
const loading = ref(false)

const fetchWeather = async () => {
  if (!city.value) {
    ElMessage.warning('请输入城市名')
    return
  }

  loading.value = true
  try {
    const { data } = await axios.get('https://www.tianqiapi.com/free/day', {
      params: {
        appid: '23035354',
        appsecret: '8YvlPNrz',
        city: city.value
      }
    })

    if (data.errcode === 0) {
      weather.value = data
    } else {
      ElMessage.error('查询失败: ' + data.errmsg)
    }
  } catch (error) {
    ElMessage.error('网络错误')
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="weather-page">
    <el-card>
      <el-input
        v-model="city"
        placeholder="输入城市名"
        @keyup.enter="fetchWeather"
      >
        <template #append>
          <el-button
            :loading="loading"
            @click="fetchWeather"
          >
            查询
          </el-button>
        </template>
      </el-input>
    </el-card>

    <el-card v-if="weather" class="weather-result">
      <h2>{{ weather.city }}</h2>
      <p>天气: {{ weather.wea }}</p>
      <p>温度: {{ weather.tem }}℃</p>
      <p>提示: {{ weather.air_tips }}</p>
    </el-card>
  </div>
</template>

<style scoped>
.weather-page {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.weather-result {
  margin-top: 20px;
}
</style>
```

---

## 📖 学习资源

### 官方文档
- Vue 3: https://cn.vuejs.org/
- Vue Router: https://router.vuejs.org/zh/
- Pinia: https://pinia.vuejs.org/zh/
- Element Plus: https://element-plus.org/zh-CN/

### 视频教程 (可选)
- B站搜索: "Vue3 快速入门"
- 推荐: 尚硅谷/黑马程序员 Vue3 教程

### 常用 Element Plus 组件
- `el-button` - 按钮
- `el-input` - 输入框
- `el-select` - 下拉选择
- `el-checkbox` - 复选框
- `el-card` - 卡片容器
- `el-table` - 表格
- `el-message` - 消息提示
- `el-dialog` - 对话框

组件文档: https://element-plus.org/zh-CN/component/button.html

---

## ✅ 学习检查清单

完成作业后,检查是否掌握以下知识点:

**Vue 基础:**
- [ ] 理解 `ref` 和 `reactive` 的区别
- [ ] 会使用 `v-if`, `v-for`, `v-model`
- [ ] 理解组件的 props 和 emit
- [ ] 会使用生命周期钩子 `onMounted`

**Vue Router:**
- [ ] 能配置路由表
- [ ] 使用 `<router-link>` 和 `<router-view>`
- [ ] 理解路由跳转 `router.push()`

**Pinia:**
- [ ] 会创建 store
- [ ] 在组件中使用 store 的状态和方法
- [ ] 理解响应式状态的概念

**Element Plus:**
- [ ] 能查阅文档使用任意组件
- [ ] 会使用表单组件 (input, select, button)
- [ ] 会使用反馈组件 (message, loading)

**工程化:**
- [ ] 理解组件化思想
- [ ] 会拆分和复用组件
- [ ] 理解项目目录结构

---

## 🎓 进阶学习建议

完成作业后,可以继续学习:

1. **TypeScript + Vue** - 类型安全
2. **组合式函数 (Composables)** - 逻辑复用
3. **Vite 构建优化** - 性能优化
4. **Vue DevTools** - 调试工具
5. **测试** - Vitest + Vue Testing Library

---

## 💡 提示

1. **遇到问题先查官方文档** - Vue/Element Plus 文档都很详细
2. **善用 Element Plus** - 不要自己写 CSS,直接用组件
3. **多看示例代码** - Element Plus 每个组件都有示例
4. **Chrome DevTools** - F12 调试,查看网络请求
5. **不要背语法** - 多写多练,用到什么查什么

---

## 🚀 作业提交

完成后建议:
1. 提交代码到 GitHub
2. 部署到 Vercel/Netlify (免费)
3. 写一篇学习总结

**Good Luck! 加油!** 🎉
