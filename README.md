# 前端快速入门 - 一天学习计划

> 适合有后端经验(Python/Go)的开发者,目标是快速掌握 Vue 3 + 组件库开发

## 📚 一天学习计划

### 上午 (3-4小时) - 理论学习

#### 第一阶段: HTML/CSS/TypeScript 最小必要知识 (1.5小时)

**HTML - 只需了解这些标签:**
```html
<div>, <span>, <button>, <input>, <form>, <a>, <img>, <ul>, <li>
```

**CSS - 核心概念:**
- 盒模型: margin、padding、border
- Flex布局: 理解 `display: flex`, `justify-content`, `align-items`
- 类选择器: `.class-name`

**TypeScript - 核心语法 (直接用 TS,跳过 JS):**

```typescript
// 1. 基本类型声明
const name: string = 'Vue'
let count: number = 0
const isActive: boolean = true
const numbers: number[] = [1, 2, 3]

// 2. 接口定义 (类似 Go struct 或 Python dataclass)
interface User {
  id: number
  name: string
  age?: number  // ? 表示可选
}

interface Todo {
  id: number
  title: string
  completed: boolean
  category: 'work' | 'life' | 'study'  // 字面量类型
}

// 3. 类型别名
type Status = 'pending' | 'loading' | 'success' | 'error'
type ApiResponse<T> = {
  code: number
  data: T
  message: string
}

// 4. 函数类型 (箭头函数,类似 Python lambda)
const add = (a: number, b: number): number => a + b

// async 函数 (类似 Python asyncio)
async function fetchData(): Promise<User[]> {
  const response = await fetch('/api/users')
  return response.json()
}

// 5. 解构赋值 (类似 Python unpacking)
const { name, age } = user  // 对象解构
const [first, second] = array  // 数组解构

// 6. 泛型 (类似 Go/Python 泛型)
function useState<T>(initial: T): [T, (value: T) => void] {
  // ...
}
const [count, setCount] = useState<number>(0)

// 7. 模板字符串
const message = `Hello ${name}`

// 8. 可选链和空值合并
const userAge = user?.profile?.age ?? 18  // 如果为 null/undefined 则用 18
```

**TypeScript 与后端语言对比:**

| 概念 | TypeScript | Python | Go |
|------|------------|--------|-----|
| 类型注解 | `name: string` | `name: str` | `name string` |
| 接口 | `interface User {}` | `@dataclass` | `type User struct {}` |
| 可选字段 | `age?: number` | `age: Optional[int]` | `*int` |
| 泛型 | `Array<T>` | `List[T]` | `[]T` |
| 联合类型 | `string \| number` | `Union[str, int]` | 用 interface |
| Promise | `Promise<T>` | `Awaitable[T]` | `chan T` |

**学习资源:**
- TypeScript 官方文档: https://www.typescriptlang.org/zh/docs/
- TypeScript 入门教程: https://ts.xcatliu.com/

---

#### 第二阶段: Vue 3 + TypeScript 核心概念 (2-3小时)

**1. 响应式基础 (带 TypeScript)**
```vue
<script setup lang="ts">
import { ref, reactive, computed } from 'vue'

// ref: 基本类型响应式 (带类型)
const count = ref<number>(0)
count.value++ // 修改需要 .value

// reactive: 对象响应式
interface User {
  name: string
  age: number
}

const user = reactive<User>({
  name: 'John',
  age: 25
})
user.name = 'Jane' // 直接修改

// computed: 计算属性 (类似 Python @property)
const doubleCount = computed<number>(() => count.value * 2)
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

**3. 组件通信 (TypeScript 版本)**
```vue
<!-- 父组件 Parent.vue -->
<script setup lang="ts">
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const parentMsg = ref<string>('Hello from parent')

const handleUpdate = (data: string) => {
  console.log('Received:', data)
}
</script>

<template>
  <ChildComponent
    :message="parentMsg"
    @update="handleUpdate"
  />
</template>

<!-- 子组件 ChildComponent.vue -->
<script setup lang="ts">
// Props 定义 (推荐方式)
interface Props {
  message: string
  count?: number  // 可选
}

const props = defineProps<Props>()

// 或者使用 withDefaults 设置默认值
const props = withDefaults(defineProps<Props>(), {
  count: 0
})

// Emit 定义
interface Emits {
  update: [data: string]  // 参数类型
  delete: [id: number]
}

const emit = defineEmits<Emits>()

const sendToParent = () => {
  emit('update', 'data from child')
}
</script>
```

**4. 生命周期钩子**
```typescript
import { onMounted, onUnmounted } from 'vue'

onMounted(() => {
  console.log('组件挂载完成')
  // 常用于: API 请求、DOM 操作、定时器
})

onUnmounted(() => {
  console.log('组件卸载')
  // 常用于: 清理定时器、取消订阅
})
```

**5. Vue 3 常用组合式 API 类型**
```typescript
import type { Ref, ComputedRef } from 'vue'

// Ref 类型
const count: Ref<number> = ref(0)

// ComputedRef 类型
const doubled: ComputedRef<number> = computed(() => count.value * 2)

// 响应式对象类型推断
interface Todo {
  id: number
  title: string
}
const todos: Ref<Todo[]> = ref([])
```

**Vue 3 官方文档:**
- 快速开始: https://cn.vuejs.org/guide/quick-start.html
- 组合式API: https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html

---

#### 第三阶段: Vue Router 和 Pinia + TypeScript (30分钟)

**Vue Router (路由 + TypeScript):**
```typescript
// router/index.ts
import { createRouter, createWebHistory, type RouteRecordRaw } from 'vue-router'
import Home from '@/views/Home.vue'
import TodoList from '@/views/TodoList.vue'
import Weather from '@/views/Weather.vue'

const routes: RouteRecordRaw[] = [
  {
    path: '/',
    name: 'home',
    component: Home
  },
  {
    path: '/todos',
    name: 'todos',
    component: TodoList
  },
  {
    path: '/weather',
    name: 'weather',
    component: Weather
  }
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

**在组件中使用路由:**
```typescript
import { useRouter, useRoute } from 'vue-router'

const router = useRouter()
const route = useRoute()

// 编程式导航
router.push('/todos')
router.push({ name: 'todos', params: { id: 123 } })

// 获取当前路由信息
console.log(route.path)  // '/todos'
console.log(route.params.id)
```

**Pinia (状态管理 + TypeScript):**
```typescript
// stores/todo.ts
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'

// 定义 Todo 接口
export interface Todo {
  id: number
  title: string
  completed: boolean
  category: 'work' | 'life' | 'study'
  createdAt: string
}

export const useTodoStore = defineStore('todo', () => {
  // 状态 (带类型)
  const todos = ref<Todo[]>([])
  const loading = ref<boolean>(false)

  // 计算属性
  const totalCount = computed<number>(() => todos.value.length)

  const completedTodos = computed<Todo[]>(() =>
    todos.value.filter(t => t.completed)
  )

  // 方法 (带类型)
  const addTodo = (todo: Omit<Todo, 'id' | 'createdAt'>): void => {
    todos.value.push({
      ...todo,
      id: Date.now(),
      createdAt: new Date().toISOString()
    })
  }

  const deleteTodo = (id: number): void => {
    todos.value = todos.value.filter(t => t.id !== id)
  }

  const toggleTodo = (id: number): void => {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      todo.completed = !todo.completed
    }
  }

  // 异步方法
  const fetchTodos = async (): Promise<void> => {
    loading.value = true
    try {
      // API 请求...
      const response = await fetch('/api/todos')
      const data: Todo[] = await response.json()
      todos.value = data
    } finally {
      loading.value = false
    }
  }

  return {
    // 状态
    todos,
    loading,
    // 计算属性
    totalCount,
    completedTodos,
    // 方法
    addTodo,
    deleteTodo,
    toggleTodo,
    fetchTodos
  }
})
```

**在组件中使用 Store:**
```vue
<script setup lang="ts">
import { useTodoStore } from '@/stores/todo'
import type { Todo } from '@/stores/todo'

const todoStore = useTodoStore()

// 直接使用状态和方法
console.log(todoStore.todos)
console.log(todoStore.totalCount)

// 添加 todo
const newTodo: Omit<Todo, 'id' | 'createdAt'> = {
  title: 'Learn Vue',
  completed: false,
  category: 'study'
}
todoStore.addTodo(newTodo)
</script>
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
  "框架": "Vue 3 (Composition API) + TypeScript",
  "路由": "Vue Router 4",
  "状态管理": "Pinia",
  "UI组件库": "Element Plus",
  "HTTP请求": "Axios",
  "构建工具": "Vite",
  "类型检查": "TypeScript 5+"
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
│   │   └── todo.ts      # ✅ TypeScript
│   ├── router/          # 路由配置
│   │   └── index.ts     # ✅ TypeScript
│   ├── types/           # 类型定义
│   │   ├── todo.ts
│   │   └── api.ts
│   ├── utils/           # 工具函数
│   │   └── storage.ts   # ✅ TypeScript
│   ├── App.vue          # 根组件
│   └── main.ts          # ✅ 入口文件 (TypeScript)
├── package.json
├── tsconfig.json        # ✅ TypeScript 配置
└── vite.config.ts       # ✅ TypeScript
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
# ✅ TypeScript  ⭐ 必选
# ❌ JSX
# ❌ Vitest (可选)
# ✅ ESLint

cd learn-fe-soso-biubiu
npm install
```

#### 2. 安装 Element Plus

```bash
npm install element-plus
npm install @element-plus/icons-vue
```

在 `main.ts` 中引入:
```typescript
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

#### LocalStorage 封装 (`utils/storage.ts`)

```typescript
// 泛型封装,类型安全
export const storage = {
  get<T>(key: string): T | null {
    const value = localStorage.getItem(key)
    if (!value) return null

    try {
      return JSON.parse(value) as T
    } catch {
      return value as T
    }
  },

  set<T>(key: string, value: T): void {
    localStorage.setItem(key, JSON.stringify(value))
  },

  remove(key: string): void {
    localStorage.removeItem(key)
  },

  clear(): void {
    localStorage.clear()
  }
}

// 使用示例
import type { Todo } from '@/types/todo'

const todos = storage.get<Todo[]>('todos') ?? []
storage.set('todos', todos)
```

#### 类型定义 (`types/todo.ts`)

```typescript
export interface Todo {
  id: number
  title: string
  completed: boolean
  category: TodoCategory
  createdAt: string
  dueDate?: string  // 可选: 截止日期
}

export type TodoCategory = 'work' | 'life' | 'study'

export interface TodoFormData {
  title: string
  category: TodoCategory
  dueDate?: string
}
```

#### Todo Store (`stores/todo.ts`)

```typescript
import { defineStore } from 'pinia'
import { ref, computed } from 'vue'
import { storage } from '@/utils/storage'
import type { Todo, TodoFormData } from '@/types/todo'

export const useTodoStore = defineStore('todo', () => {
  // 状态 (带类型)
  const todos = ref<Todo[]>(storage.get<Todo[]>('todos') ?? [])
  const loading = ref<boolean>(false)

  // 计算属性
  const totalCount = computed<number>(() => todos.value.length)

  const completedCount = computed<number>(() =>
    todos.value.filter(t => t.completed).length
  )

  const pendingCount = computed<number>(() =>
    totalCount.value - completedCount.value
  )

  const todosByCategory = computed(() => {
    return {
      work: todos.value.filter(t => t.category === 'work'),
      life: todos.value.filter(t => t.category === 'life'),
      study: todos.value.filter(t => t.category === 'study')
    }
  })

  // 方法 (带类型)
  const addTodo = (formData: TodoFormData): void => {
    const newTodo: Todo = {
      id: Date.now(),
      title: formData.title,
      category: formData.category,
      completed: false,
      createdAt: new Date().toISOString(),
      dueDate: formData.dueDate
    }
    todos.value.push(newTodo)
    saveTodos()
  }

  const deleteTodo = (id: number): void => {
    todos.value = todos.value.filter(t => t.id !== id)
    saveTodos()
  }

  const toggleTodo = (id: number): void => {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      todo.completed = !todo.completed
      saveTodos()
    }
  }

  const updateTodo = (id: number, updates: Partial<Todo>): void => {
    const todo = todos.value.find(t => t.id === id)
    if (todo) {
      Object.assign(todo, updates)
      saveTodos()
    }
  }

  const saveTodos = (): void => {
    storage.set('todos', todos.value)
  }

  return {
    // 状态
    todos,
    loading,
    // 计算属性
    totalCount,
    completedCount,
    pendingCount,
    todosByCategory,
    // 方法
    addTodo,
    deleteTodo,
    toggleTodo,
    updateTodo
  }
})
```

#### 天气 API 类型定义 (`types/api.ts`)

```typescript
// 天气 API 响应类型
export interface WeatherApiResponse {
  errcode: number
  errmsg: string
  city: string
  wea: string        // 天气状况
  tem: string        // 温度
  tem_day: string    // 白天温度
  tem_night: string  // 夜间温度
  win: string        // 风向
  win_speed: string  // 风速
  air_tips: string   // 空气提示
  humidity: string   // 湿度
}

// 统一 API 响应格式
export interface ApiResponse<T> {
  code: number
  data: T
  message: string
}
```

#### 天气查询示例 (`views/Weather.vue`)

```vue
<script setup lang="ts">
import { ref } from 'vue'
import axios, { type AxiosError } from 'axios'
import { ElMessage } from 'element-plus'
import type { WeatherApiResponse } from '@/types/api'

const city = ref<string>('')
const weather = ref<WeatherApiResponse | null>(null)
const loading = ref<boolean>(false)

const fetchWeather = async (): Promise<void> => {
  if (!city.value.trim()) {
    ElMessage.warning('请输入城市名')
    return
  }

  loading.value = true
  try {
    const { data } = await axios.get<WeatherApiResponse>(
      'https://www.tianqiapi.com/free/day',
      {
        params: {
          appid: '23035354',
          appsecret: '8YvlPNrz',
          city: city.value
        }
      }
    )

    if (data.errcode === 0) {
      weather.value = data
      ElMessage.success('查询成功')
    } else {
      ElMessage.error(`查询失败: ${data.errmsg}`)
    }
  } catch (error) {
    const axiosError = error as AxiosError
    console.error('Weather API Error:', axiosError)
    ElMessage.error('网络错误,请稍后重试')
  } finally {
    loading.value = false
  }
}

const resetWeather = (): void => {
  city.value = ''
  weather.value = null
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
- **TypeScript**: https://www.typescriptlang.org/zh/docs/
- **Vue 3**: https://cn.vuejs.org/
- **Vue + TypeScript**: https://cn.vuejs.org/guide/typescript/overview.html
- **Vue Router**: https://router.vuejs.org/zh/
- **Pinia**: https://pinia.vuejs.org/zh/
- **Element Plus**: https://element-plus.org/zh-CN/

### TypeScript 入门推荐
- TypeScript 入门教程: https://ts.xcatliu.com/
- TypeScript Deep Dive (中文版): https://jkchao.github.io/typescript-book-chinese/

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

**TypeScript 基础:**
- [ ] 理解基本类型注解 (`string`, `number`, `boolean`)
- [ ] 会定义接口 (`interface`) 和类型别名 (`type`)
- [ ] 理解泛型的基本用法 (`Array<T>`, `Promise<T>`)
- [ ] 会使用联合类型和可选属性
- [ ] 理解 `Omit`, `Pick`, `Partial` 等工具类型

**Vue 3 + TypeScript:**
- [ ] 理解 `ref<T>` 和 `reactive<T>` 的类型标注
- [ ] 会使用 `defineProps<Props>()` 定义组件属性
- [ ] 会使用 `defineEmits<Emits>()` 定义事件
- [ ] 会使用 `v-if`, `v-for`, `v-model`
- [ ] 会使用生命周期钩子 `onMounted`

**Vue Router + TypeScript:**
- [ ] 能配置类型安全的路由表 (`RouteRecordRaw[]`)
- [ ] 使用 `useRouter()` 和 `useRoute()` 进行导航
- [ ] 理解路由跳转和参数传递

**Pinia + TypeScript:**
- [ ] 会创建类型安全的 store
- [ ] 在组件中正确使用 store 的状态和方法
- [ ] 理解响应式状态和计算属性的类型推断

**Element Plus:**
- [ ] 能查阅文档使用任意组件
- [ ] 会使用表单组件 (input, select, button)
- [ ] 会使用反馈组件 (message, loading)

**工程化:**
- [ ] 理解组件化思想
- [ ] 会拆分和复用组件
- [ ] 理解 TypeScript 项目目录结构
- [ ] 会使用 VSCode 的 TypeScript 智能提示

---

## 🎓 进阶学习建议

完成作业后,可以继续学习:

1. **TypeScript 高级类型** - 泛型、工具类型、类型体操
2. **组合式函数 (Composables)** - 逻辑复用和封装
3. **Vue 3 性能优化** - 虚拟列表、懒加载、代码分割
4. **Vite 构建优化** - 打包优化、环境变量
5. **Vue DevTools** - 调试工具(支持 TypeScript)
6. **单元测试** - Vitest + Vue Testing Library
7. **E2E 测试** - Playwright/Cypress

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
