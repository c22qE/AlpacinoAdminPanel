<template>
  <section class="section">
    <h2 class="title is-3 has-text-grey">Заказы</h2>
    <list-orders></list-orders>
  </section>
</template>

<script>
import ListOrders from '~/components/lists/ListOrders'

export default {
  name: 'IndexPage',
  components: {
    ListOrders,
  },
  head: {
    title: 'Админ панель ALPACINO ',
  },
}
</script>
