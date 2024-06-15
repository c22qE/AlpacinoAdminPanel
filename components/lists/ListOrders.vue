<template>
  <section class="section">
    <div class="grid is-mobile">
      <a href="#" @click="getOrders()">Обновить</a>

      <!-- <div
        v-for="(order, index) in orders.slice().reverse()"
        :key="index"
        class="cell"
      >
        <OrderCard
          v-if="order"
          :id="index"
          :title="order.title"
          :price="order.price"
          :image="order.image"
          icon="cellphone-link"
        >
          <b class="has-text-grey"> Нажмите чтобы посмотреть </b>
        </OrderCard>
      </div> -->
    </div>

    <audio
      id="sound"
      src="/event_new_order.mp3"
      type="audio/mpeg"
      class="hidden"
    ></audio>
  </section>
</template>

<script>
// import OrderCard from './OrderCard.vue'

export default {
  name: 'ListOrders',
  // components: { OrderCard },
  data() {
    return {
      orders: [],
    }
  },
  mounted() {
    this.getOrders()
    this.interval = setInterval(() => {
      this.getOrders()
    }, 1000)
  },
  beforeDestroy() {
    clearInterval(this.interval)
  },
  methods: {
    async getOrders() {
      await this.$axios
        .get('/orders')

        .then((response) => {
          const lastResponse = response.data
          // Проверяем если данные новые, то играем звук
          if (lastResponse !== response.data) {
            this.orders = response.data
            this.playSound()
          } else {
            this.orders = lastResponse
          }
        })
        .catch((error) => {
          throw error
        })
      // console.log(this.orders)
    },
    async getProducts() {
      await this.$axios
        .get('/products')
        // .get('/orders')

        .then((response) => {
          this.products = response.data
        })
        .catch((error) => {
          throw error
        })
      // console.log(this.orders)
    },
    playSound() {
      const audio = document.getElementById('sound')
      audio.play()
    },
  },
}
</script>
