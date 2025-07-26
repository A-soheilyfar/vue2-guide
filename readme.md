# راه‌های ایجاد پروژه Vue 2
1. استفاده از Vue CLI (توصیه می‌شه)
نصب Vue CLI:
```bash
npm install -g @vue/cli
# یا
yarn global add @vue/cli
```
## ایجاد پروژه جدید:
```bash
vue create my-vue2-project
```

وقتی این دستور رو اجرا کنی، گزینه‌هایی نمایش داده می‌شه:
```bash

? Please pick a preset:
❯ Default ([Vue 2] babel, eslint)
  Default ([Vue 3] babel, eslint)  
  Manually select features

```

اگر میخواین router رو هم در همین مرحله اضافه کنید از گزینه manual برید جلو
گزینه ۱ رو انتخاب کن
یا Manual Configuration:
اگر Manually select features رو انتخاب کنی:

‍‍‍```bash
? Check the features needed for your project:
 ◉ Choose Vue version
 ◉ Babel
 ◯ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
 ◯ Vuex
 ◉ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing```

بعد:

Vue version: 2.x رو انتخاب کن
Router: اگه می‌خوای routing داشته باشی
CSS Pre-processor: Sass/SCSS توصیه می‌شه
Linter: ESLint + Prettier

2 - استفاده از Vite (سریع‌تر)
```bash
npm create vue@2 my-vue2-project
# یا
yarn create vue@2 my-vue2-project
```

4. شروع Development Server

```bash
cd my-vue2-project
npm run serve
# یا
yarn serve
```
پروژه روی http://localhost:8080 اجرا می‌شه.


برای تغییر پورت اچرایی در فایل vue.config.js این کد ها رو قرار بدید
```javascript
const { defineConfig } = require('@vue/cli-service');

module.exports = defineConfig({
  transpileDependencies: true,

  devServer: {
    // شماره پورت را اینجا مشخص کنید
    port: 3100, 
    client: {
      // این آدرس را هم مطابق با پورت جدید آپدیت کنید
      webSocketURL: 'ws://localhost:3100/ws', 
    },
  },
});
```

# Vue 2 Router 🚧
برای Vue 2 حتماً از Vue Router نسخه 3 استفاده کنید
- `<router-view>` جایی است که کامپوننت‌های مختلف نمایش داده می‌شوند
- `<router-link>` برای ایجاد لینک‌های navigation استفاده می‌شود
- Route Guards برای کنترل دسترسی و احراز هویت بسیار مفیدند

### 1. ایجاد فایل router
ابتدا فایل router/index.js ایجاد کنید:
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '@/views/Home.vue'
import About from '@/views/About.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    component: About
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router

```

### 2. تنظیم در main.js
```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'

Vue.config.productionTip = false

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```
### 3. اضافه کردن router-view در App.vue

```javascript

<template>
  <div id="app">
    <nav>
      <router-link to="/">خانه</router-link>
      <router-link to="/about">درباره ما</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
  margin-right: 10px;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style><template>
  <div id="app">
    <nav>
      <router-link to="/">خانه</router-link>
      <router-link to="/about">درباره ما</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
  margin-right: 10px;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>

```

## انواع Route ها
- Route های ساده
```javascript
const routes = [
  { path: '/', component: Home },
  { path: '/users', component: Users },
  { path: '/contact', component: Contact }
]
```
- Route های پارامتری (Dynamic Routes)
```javascript
const routes = [
  // پارامتر :id
  { path: '/user/:id', component: User },
  
  // چند پارامتر
  { path: '/user/:id/post/:postId', component: UserPost },
  
  // پارامتر اختیاری
  { path: '/user/:id?', component: User }
]
```

دسترسی به پارامترها در کامپوننت:
```vue
<template>
  <div>
    <h1>User ID: {{ $route.params.id }}</h1>
  </div>
</template>

<script>
export default {
  name: 'User',
  created() {
    console.log('User ID:', this.$route.params.id)
  },
  watch: {
    '$route'(to, from) {
      // واکنش به تغییر route
      console.log('Route changed from', from.path, 'to', to.path)
    }
  }
}
</script>
```
### Nested Routes (Route های تو در تو)
```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      // /user/:id/profile
      { path: 'profile', component: UserProfile },
      
      // /user/:id/posts
      { path: 'posts', component: UserPosts },
      
      // route خالی یعنی /user/:id
      { path: '', component: UserHome }
    ]
  }
]
```
کامپوننت والد باید `<router-view>` داشته باشد:

```vue
<template>
  <div class="user">
    <h2>User {{ $route.params.id }}</h2>
    <router-view></router-view>
  </div>
</template>
```

## Navigation (جابجایی)
- استفاده از router-link
```vue
<template>
  <div>
    <!-- لینک ساده -->
    <router-link to="/users">کاربران</router-link>
    
    <!-- لینک با پارامتر -->
    <router-link :to="`/user/${userId}`">پروفایل کاربر</router-link>
    
    <!-- لینک با object -->
    <router-link :to="{ name: 'User', params: { id: userId }}">
      پروفایل کاربر
    </router-link>
    
    <!-- لینک با query parameters -->
    <router-link :to="{ path: '/users', query: { page: 2 }}">
      صفحه 2 کاربران
    </router-link>
  </div>
</template>
```
- Navigation برنامه‌ای (Programmatic Navigation)
```javascript
// در methods کامپوننت
methods: {
  goToUser(userId) {
    // با path
    this.$router.push(`/user/${userId}`)
    
    // با object
    this.$router.push({ name: 'User', params: { id: userId }})
    
    // با query
    this.$router.push({ path: '/users', query: { page: 2 }})
  },
  
  goBack() {
    this.$router.go(-1)
  },
  
  replaceRoute() {
    // جایگزین کردن به جای اضافه کردن به history
    this.$router.replace('/new-page')
  }
}
```


# animation in Vue2 

این رفتار مربوط به سیستم Transition Classes در Vue.js است. وقتی شما از `<transition>` با نام مشخص استفاده می‌کنید، Vue به صورت خودکار کلاس‌های CSS مخصوص انیمیشن را اضافه و حذف می‌کند.
چگونگی کار Transition Classes در Vue:
وقتی شما می‌نویسید:

```vue
<transition name="alert-in">
```

Vue به صورت خودکار این کلاس‌ها را در مراحل مختلف انیمیشن اضافه می‌کند:
برای ورود (Enter):

- `alert-in-enter` - حالت شروع
- `alert-in-enter-active `- در طول کل مرحله ورود (اینجا animation تعریف می‌شود)
- `alert-in-enter-to` - حالت پایان

برای خروج (Leave):

- `alert-in-leave` - حالت شروع خروج
- `alert-in-leave-active` - در طول کل مرحله خروج (اینجا animation تعریف می‌شود)
- `alert-in-leave-to` - حالت پایان خروج

چرا -enter-active و -leave-active؟
این کلاس‌ها در طول کل مدت انیمیشن روی المان باقی می‌مانند. به همین دلیل برای تعریف `animation` یا `transition` از آنها استفاده می‌شود.

```css
.alert-in-enter-active {
    animation: bounce-in .3s; /* در طول ورود */
}
.alert-in-leave-active {
    animation: bounce-in .3s reverse; /* در طول خروج */
}
```
شما نیازی به تعریف دستی این کلاس‌ها در HTML ندارید - Vue خودش آنها را مدیریت می‌کند!



### `<transition>`

- برای انیمیت کردن یک المنت استفاده می‌شه
- فقط یک child element می‌تونه داشته باشه
- برای show/hide یا تغییر حالت یک کامپوننت

```vue

<transition name="fade">
  <p v-if="show">این متن نمایش داده می‌شه</p>
</transition>

```

### `<transition-group>`
- برای انیمیت کردن لیست از المنت‌ها استفاده می‌شه
- می‌تونه چندین child element داشته باشه
- برای لیست‌هایی که المنت‌ها اضافه/حذف/جابجا می‌شن

```vue
<transition-group name="list">
  <li v-for="item in items" :key="item.id">
    {{ item.name }}
  </li>
</transition-group>

```
نکات مهم:

`<transition-group>` به صورت پیش‌فرض یک `<span>` رندر می‌کنه
- می‌تونی با `tag="ul"` تگ رو تغییر بدی
- حتماً `key` برای هر المنت در `<transition-group>` تعریف کن
- `<transition-group>` انیمیشن‌های move هم پشتیبانی می‌کنه (وقتی المنت‌ها جابجا می‌شن)



مثال :
```html
<transition enter-active-class="animate__animated animate__bounceIn">
  <p class="alert" v-if="errors.has('skill')">{{ errors.first('skill') }}</p>
</transition>
```
```html
<transition-group name="list" enter-active-class="animate__animated animate__bounceInRight">
  <li v-for="(data,index) in Skills" :key="index">{{ data.skill}}</li>
</transition-group>
```

