## animation in Vue2 

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



