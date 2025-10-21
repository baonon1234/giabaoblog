---
title: "React vs Vue: So sánh Frontend Frameworks"
date: 2025-10-19
draft: false
tags: ["javascript", "react", "vue", "frontend", "so-sánh", "lập-trình"]
categories: ["lập-trình"]
summary: "So sánh chi tiết giữa React và Vue.js cho frontend development, ưu nhược điểm và trường hợp sử dụng."
---

# React vs Vue: So sánh Frontend Frameworks

Chào các bạn! Hôm nay mình sẽ so sánh chi tiết giữa React và Vue.js - hai framework frontend phổ biến nhất hiện nay. Mỗi framework có thế mạnh riêng, hãy cùng tìm hiểu nhé! 😊

## 🎯 Tổng quan

### React
- **Developer**: Facebook (Meta)
- **Release**: 2013
- **Philosophy**: Library, component-based
- **Learning curve**: Moderate
- **Ecosystem**: Huge, mature
- **Job market**: Very high demand

### Vue.js
- **Developer**: Evan You
- **Release**: 2014
- **Philosophy**: Framework, progressive
- **Learning curve**: Easy
- **Ecosystem**: Growing, well-organized
- **Job market**: Growing demand

## 📊 So sánh chi tiết

### 1. Syntax và Template

#### React (JSX)
```jsx
import React, { useState, useEffect } from 'react';

function UserList() {
    const [users, setUsers] = useState([]);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        fetchUsers();
    }, []);
    
    const fetchUsers = async () => {
        try {
            const response = await fetch('/api/users');
            const data = await response.json();
            setUsers(data);
        } catch (error) {
            console.error('Error:', error);
        } finally {
            setLoading(false);
        }
    };
    
    const handleDelete = (id) => {
        setUsers(users.filter(user => user.id !== id));
    };
    
    if (loading) return <div>Loading...</div>;
    
    return (
        <div className="user-list">
            <h1>Danh sách người dùng</h1>
            {users.map(user => (
                <div key={user.id} className="user-card">
                    <h3>{user.name}</h3>
                    <p>Email: {user.email}</p>
                    <button onClick={() => handleDelete(user.id)}>
                        Xóa
                    </button>
                </div>
            ))}
        </div>
    );
}

export default UserList;
```

**Đặc điểm JSX:**
- **JavaScript trong HTML**: Cú pháp quen thuộc
- **Type safety**: Với TypeScript
- **Tooling**: Excellent IDE support
- **Learning curve**: Cần học JSX syntax

#### Vue.js (Template)
```vue
<template>
    <div class="user-list">
        <h1>Danh sách người dùng</h1>
        <div v-if="loading" class="loading">Loading...</div>
        <div v-else>
            <div 
                v-for="user in users" 
                :key="user.id" 
                class="user-card"
            >
                <h3>{{ user.name }}</h3>
                <p>Email: {{ user.email }}</p>
                <button @click="handleDelete(user.id)">
                    Xóa
                </button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: [],
            loading: true
        }
    },
    async mounted() {
        await this.fetchUsers();
    },
    methods: {
        async fetchUsers() {
            try {
                const response = await fetch('/api/users');
                this.users = await response.json();
            } catch (error) {
                console.error('Error:', error);
            } finally {
                this.loading = false;
            }
        },
        handleDelete(id) {
            this.users = this.users.filter(user => user.id !== id);
        }
    }
}
</script>

<style scoped>
.user-list {
    padding: 20px;
}

.user-card {
    border: 1px solid #ddd;
    padding: 15px;
    margin: 10px 0;
    border-radius: 5px;
}
</style>
```

**Đặc điểm Vue Template:**
- **HTML-like syntax**: Dễ học cho người mới
- **Directives**: v-if, v-for, v-model
- **Scoped CSS**: CSS chỉ áp dụng cho component
- **Learning curve**: Gentle, familiar

### 2. State Management

#### React với Hooks
```jsx
import React, { useState, useEffect, useReducer } from 'react';

// useState cho state đơn giản
function Counter() {
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </div>
    );
}

// useReducer cho state phức tạp
const initialState = { users: [], loading: false, error: null };

function userReducer(state, action) {
    switch (action.type) {
        case 'FETCH_START':
            return { ...state, loading: true, error: null };
        case 'FETCH_SUCCESS':
            return { ...state, loading: false, users: action.payload };
        case 'FETCH_ERROR':
            return { ...state, loading: false, error: action.payload };
        default:
            return state;
    }
}

function UserList() {
    const [state, dispatch] = useReducer(userReducer, initialState);
    
    useEffect(() => {
        dispatch({ type: 'FETCH_START' });
        fetchUsers()
            .then(users => dispatch({ type: 'FETCH_SUCCESS', payload: users }))
            .catch(error => dispatch({ type: 'FETCH_ERROR', payload: error.message }));
    }, []);
    
    return (
        <div>
            {state.loading && <div>Loading...</div>}
            {state.error && <div>Error: {state.error}</div>}
            {state.users.map(user => (
                <div key={user.id}>{user.name}</div>
            ))}
        </div>
    );
}
```

#### Vue.js với Composition API
```vue
<template>
    <div>
        <p>Count: {{ count }}</p>
        <button @click="increment">Increment</button>
        
        <div v-if="loading">Loading...</div>
        <div v-else-if="error">Error: {{ error }}</div>
        <div v-else>
            <div v-for="user in users" :key="user.id">
                {{ user.name }}
            </div>
        </div>
    </div>
</template>

<script>
import { ref, reactive, onMounted } from 'vue';

export default {
    setup() {
        // Reactive state
        const count = ref(0);
        const users = ref([]);
        const loading = ref(false);
        const error = ref(null);
        
        // Methods
        const increment = () => {
            count.value++;
        };
        
        const fetchUsers = async () => {
            loading.value = true;
            error.value = null;
            try {
                const response = await fetch('/api/users');
                users.value = await response.json();
            } catch (err) {
                error.value = err.message;
            } finally {
                loading.value = false;
            }
        };
        
        // Lifecycle
        onMounted(() => {
            fetchUsers();
        });
        
        return {
            count,
            users,
            loading,
            error,
            increment
        };
    }
}
</script>
```

### 3. Component Communication

#### React Props và Context
```jsx
// Parent Component
import React, { createContext, useContext, useState } from 'react';

const UserContext = createContext();

function App() {
    const [users, setUsers] = useState([]);
    
    return (
        <UserContext.Provider value={{ users, setUsers }}>
            <UserList />
            <UserForm />
        </UserContext.Provider>
    );
}

// Child Component
function UserList() {
    const { users } = useContext(UserContext);
    
    return (
        <div>
            {users.map(user => (
                <UserCard key={user.id} user={user} />
            ))}
        </div>
    );
}

// Props drilling
function UserCard({ user }) {
    return (
        <div>
            <h3>{user.name}</h3>
            <p>{user.email}</p>
        </div>
    );
}
```

#### Vue Props và Provide/Inject
```vue
<!-- Parent Component -->
<template>
    <div>
        <UserList :users="users" @user-selected="handleUserSelected" />
        <UserForm @user-added="handleUserAdded" />
    </div>
</template>

<script>
export default {
    data() {
        return {
            users: []
        }
    },
    provide() {
        return {
            users: this.users,
            addUser: this.addUser
        }
    },
    methods: {
        handleUserSelected(user) {
            console.log('Selected user:', user);
        },
        handleUserAdded(user) {
            this.users.push(user);
        }
    }
}
</script>

<!-- Child Component -->
<template>
    <div>
        <div 
            v-for="user in users" 
            :key="user.id"
            @click="$emit('user-selected', user)"
        >
            <h3>{{ user.name }}</h3>
            <p>{{ user.email }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        users: {
            type: Array,
            required: true
        }
    },
    inject: ['addUser']
}
</script>
```

### 4. Routing

#### React Router
```jsx
import React from 'react';
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
    return (
        <BrowserRouter>
            <nav>
                <Link to="/">Home</Link>
                <Link to="/users">Users</Link>
                <Link to="/about">About</Link>
            </nav>
            
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/users" element={<UserList />} />
                <Route path="/users/:id" element={<UserDetail />} />
                <Route path="/about" element={<About />} />
                <Route path="*" element={<NotFound />} />
            </Routes>
        </BrowserRouter>
    );
}

// Route component
function UserDetail() {
    const { id } = useParams();
    const [user, setUser] = useState(null);
    
    useEffect(() => {
        fetchUser(id).then(setUser);
    }, [id]);
    
    if (!user) return <div>Loading...</div>;
    
    return (
        <div>
            <h1>{user.name}</h1>
            <p>Email: {user.email}</p>
        </div>
    );
}
```

#### Vue Router
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../views/Home.vue';
import UserList from '../views/UserList.vue';
import UserDetail from '../views/UserDetail.vue';

const routes = [
    { path: '/', name: 'Home', component: Home },
    { path: '/users', name: 'UserList', component: UserList },
    { path: '/users/:id', name: 'UserDetail', component: UserDetail },
    { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound }
];

const router = createRouter({
    history: createWebHistory(),
    routes
});

export default router;
```

```vue
<!-- App.vue -->
<template>
    <div id="app">
        <nav>
            <router-link to="/">Home</router-link>
            <router-link to="/users">Users</router-link>
            <router-link to="/about">About</router-link>
        </nav>
        
        <router-view />
    </div>
</template>
```

```vue
<!-- UserDetail.vue -->
<template>
    <div v-if="loading">Loading...</div>
    <div v-else-if="user">
        <h1>{{ user.name }}</h1>
        <p>Email: {{ user.email }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            user: null,
            loading: true
        }
    },
    async created() {
        const id = this.$route.params.id;
        this.user = await this.fetchUser(id);
        this.loading = false;
    },
    methods: {
        async fetchUser(id) {
            const response = await fetch(`/api/users/${id}`);
            return await response.json();
        }
    }
}
</script>
```

### 5. State Management Libraries

#### React với Redux
```javascript
// store/userSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUsers = createAsyncThunk(
    'users/fetchUsers',
    async () => {
        const response = await fetch('/api/users');
        return await response.json();
    }
);

const userSlice = createSlice({
    name: 'users',
    initialState: {
        users: [],
        loading: false,
        error: null
    },
    reducers: {
        addUser: (state, action) => {
            state.users.push(action.payload);
        },
        removeUser: (state, action) => {
            state.users = state.users.filter(user => user.id !== action.payload);
        }
    },
    extraReducers: (builder) => {
        builder
            .addCase(fetchUsers.pending, (state) => {
                state.loading = true;
            })
            .addCase(fetchUsers.fulfilled, (state, action) => {
                state.loading = false;
                state.users = action.payload;
            })
            .addCase(fetchUsers.rejected, (state, action) => {
                state.loading = false;
                state.error = action.error.message;
            });
    }
});

export const { addUser, removeUser } = userSlice.actions;
export default userSlice.reducer;
```

```jsx
// Component sử dụng Redux
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchUsers, addUser } from '../store/userSlice';

function UserList() {
    const { users, loading, error } = useSelector(state => state.users);
    const dispatch = useDispatch();
    
    useEffect(() => {
        dispatch(fetchUsers());
    }, [dispatch]);
    
    const handleAddUser = (userData) => {
        dispatch(addUser(userData));
    };
    
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;
    
    return (
        <div>
            {users.map(user => (
                <div key={user.id}>{user.name}</div>
            ))}
        </div>
    );
}
```

#### Vue với Pinia
```javascript
// stores/userStore.js
import { defineStore } from 'pinia';

export const useUserStore = defineStore('users', {
    state: () => ({
        users: [],
        loading: false,
        error: null
    }),
    
    getters: {
        userCount: (state) => state.users.length,
        activeUsers: (state) => state.users.filter(user => user.active)
    },
    
    actions: {
        async fetchUsers() {
            this.loading = true;
            this.error = null;
            try {
                const response = await fetch('/api/users');
                this.users = await response.json();
            } catch (error) {
                this.error = error.message;
            } finally {
                this.loading = false;
            }
        },
        
        addUser(user) {
            this.users.push(user);
        },
        
        removeUser(id) {
            this.users = this.users.filter(user => user.id !== id);
        }
    }
});
```

```vue
<!-- Component sử dụng Pinia -->
<template>
    <div>
        <div v-if="userStore.loading">Loading...</div>
        <div v-else-if="userStore.error">Error: {{ userStore.error }}</div>
        <div v-else>
            <div v-for="user in userStore.users" :key="user.id">
                {{ user.name }}
            </div>
        </div>
    </div>
</template>

<script>
import { useUserStore } from '../stores/userStore';

export default {
    setup() {
        const userStore = useUserStore();
        
        // Fetch users on component mount
        userStore.fetchUsers();
        
        return {
            userStore
        };
    }
}
</script>
```

## 🚀 Performance Comparison

### Bundle Size
- **React**: ~42KB (gzipped)
- **Vue**: ~34KB (gzipped)
- **React + Router**: ~45KB
- **Vue + Router**: ~37KB

### Runtime Performance
```javascript
// React - Virtual DOM diffing
function UserList({ users }) {
    return (
        <div>
            {users.map(user => (
                <UserCard key={user.id} user={user} />
            ))}
        </div>
    );
}

// Vue - Template compilation
// Vue template được compile thành render function
// Tối ưu hóa tốt hơn cho static content
```

### Memory Usage
- **React**: Higher memory usage
- **Vue**: Lower memory usage
- **React**: More re-renders
- **Vue**: Fewer re-renders

## 🛠️ Development Experience

### React Development
```jsx
// Hot reload với Create React App
import React from 'react';
import { hot } from 'react-hot-loader';

function App() {
    return <div>Hello World</div>;
}

export default hot(module)(App);
```

**Development features:**
- **Create React App**: Zero-config setup
- **Next.js**: Full-stack React framework
- **Gatsby**: Static site generator
- **React DevTools**: Excellent debugging
- **TypeScript**: First-class support

### Vue Development
```javascript
// Vue CLI
vue create my-project
vue add router
vue add vuex
```

**Development features:**
- **Vue CLI**: Command-line interface
- **Nuxt.js**: Full-stack Vue framework
- **Vite**: Fast build tool
- **Vue DevTools**: Great debugging
- **TypeScript**: Good support

## 📈 Learning Curve

### React Learning Path
1. **JavaScript ES6+** (2-4 weeks)
2. **JSX Syntax** (1-2 weeks)
3. **Components & Props** (1-2 weeks)
4. **State & Hooks** (2-3 weeks)
5. **Routing** (1 week)
6. **State Management** (2-3 weeks)
7. **Testing** (2-3 weeks)

**Total: 11-18 weeks**

### Vue Learning Path
1. **JavaScript ES6+** (2-4 weeks)
2. **Vue Template** (1 week)
3. **Components & Props** (1 week)
4. **Reactivity** (1-2 weeks)
5. **Routing** (1 week)
6. **State Management** (1-2 weeks)
7. **Testing** (1-2 weeks)

**Total: 8-13 weeks**

## 💼 Job Market

### React Jobs
- **Demand**: Very high
- **Salary**: $70,000 - $150,000/year
- **Companies**: Facebook, Netflix, Airbnb, Uber
- **Skills**: React, Redux, TypeScript, Testing

### Vue Jobs
- **Demand**: Growing
- **Salary**: $60,000 - $130,000/year
- **Companies**: Alibaba, GitLab, Adobe, Nintendo
- **Skills**: Vue, Vuex, TypeScript, Testing

## 🎯 Khi nào nên chọn React?

### Chọn React khi:
- **Large applications**: Ứng dụng lớn, phức tạp
- **Team experience**: Team đã quen với React
- **Ecosystem**: Cần ecosystem phong phú
- **Performance**: Cần performance cao
- **Job market**: Tập trung vào job opportunities

### Ví dụ dự án phù hợp:
- **Social media**: Facebook, Twitter
- **E-commerce**: Amazon, Shopify
- **SaaS platforms**: Slack, Notion
- **Mobile apps**: React Native

## 🌟 Khi nào nên chọn Vue?

### Chọn Vue khi:
- **Small to medium apps**: Ứng dụng vừa và nhỏ
- **Learning curve**: Cần học nhanh
- **Team new to frameworks**: Team mới với frameworks
- **Rapid prototyping**: Prototype nhanh
- **Progressive adoption**: Tích hợp từ từ

### Ví dụ dự án phù hợp:
- **Admin dashboards**: Quản trị hệ thống
- **Marketing websites**: Website marketing
- **Internal tools**: Công cụ nội bộ
- **Prototypes**: MVP, demo

## 🔮 Tương lai

### React Roadmap
- **Concurrent Features**: React 18+
- **Server Components**: RSC
- **Suspense**: Better loading states
- **Streaming**: Progressive rendering

### Vue Roadmap
- **Vue 3**: Composition API
- **Vite**: Faster development
- **TypeScript**: Better support
- **SSR**: Server-side rendering

## 🎯 Kết luận

### React phù hợp khi:
- Xây dựng ứng dụng lớn, phức tạp
- Cần ecosystem phong phú
- Team có kinh nghiệm JavaScript
- Tập trung vào job opportunities
- Cần performance cao

### Vue phù hợp khi:
- Xây dựng ứng dụng vừa và nhỏ
- Cần learning curve dễ dàng
- Team mới với frontend frameworks
- Cần development nhanh
- Progressive adoption

**Lời khuyên:**
- Học cả hai để có nhiều lựa chọn
- Bắt đầu với Vue nếu mới học
- Chuyển sang React khi cần scale
- Thực hành với project thực tế
- Theo dõi trends và cập nhật

Chúc các bạn chọn được framework phù hợp! Nếu có câu hỏi gì, hãy comment bên dưới nhé! 😊

---

*Bài viết được viết dựa trên kinh nghiệm thực tế của tác giả. So sánh có thể thay đổi tùy theo phiên bản và use case cụ thể.*
