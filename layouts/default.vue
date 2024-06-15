<template>
  <div>
    <section class="main-content columns">
      <aside class="column is-2 section">
        <div class="card-image has-text-centered">
          <figure class="image is-inline-block">
            <img src="~assets/logo.png" />
          </figure>
        </div>
        <p class="menu-label is-hidden-touch">Меню</p>
        <ul class="menu-list">
          <li v-for="(item, key) of items" :key="key">
            <NuxtLink :to="item.to" exact-active-class="is-active">
              <font-awesome-icon :icon="item.icon" /> {{ item.title }}
            </NuxtLink>
          </li>
        </ul>
      </aside>

      <div class="container column is-10">
        <Nuxt />
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      items: [
        {
          title: 'Заказы',
          icon: ['fas', 'utensils'],
          to: { name: 'index' },
        },
        {
          title: 'Продукты',
          icon: ['fas', 'pizza-slice'],
          to: { name: 'products' },
        },
        {
          title: 'Категории продуктов',
          icon: ['fas', 'poll-h'],
          to: { name: 'categories' },
        },
        {
          title: 'Опций продуктов (размеры)',
          icon: ['fas', 'ruler'],
          to: { name: 'options' },
        },
        {
          title: 'Платежи',
          icon: ['fas', 'credit-card'],
          to: { name: 'payments' },
        },
        {
          title: 'Статистика',
          icon: ['fas', 'chart-area'],
          to: { name: 'stats' },
        },
        {
          title: 'Техническая помощь',
          icon: ['fas', 'code'],
          to: { name: 'help' },
        },
      ],
    }
  },
  head: {
    title: 'Админ панель ALPACINO ',
  },
}
</script>
