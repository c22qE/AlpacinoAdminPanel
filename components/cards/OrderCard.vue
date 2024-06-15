<template>
  <div class="column">
    <a href="/orders/1">
      <div class="card">
        <header class="card-header">
          <p class="card-header-title has-text-grey">
            {{ title }}
          </p>
        </header>
        <div class="card-content">
          <div class="content has-text-centered">
            <!-- номер заказа id -->
            <!-- дата заказа -->
            <!-- дата и время доставки -->
            <!-- Название товара -->
            <!-- Количество -->
            <!-- Кто заказал -->
            <!-- Номер телефона -->
            <!-- Адрес -->
            <!-- Доставка - Есть/нету, клиент заберёт -->
            <!-- Цена заказа -->
            <!-- Статус заказа - готов/не готов -->

            <!-- <div class="card-image has-text-centered">
              <figure class="image is-inline-block">
                <img :src="image" />
              </figure>
            </div> -->
          </div>
        </div>
        <footer class="card-footer">
          <div class="card-footer-item">
            <span>
              <slot />
            </span>
          </div>
        </footer>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: 'OrderCard',
  props: {
    id: {
      type: Number,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    category: {
      type: String,
      required: true,
    },
    price: {
      type: Number,
      required: true,
    },
    image: {
      type: String,
      required: true,
    },
  },
}
</script>
