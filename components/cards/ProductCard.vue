<template>
  <div class="column">
    <div class="card">
      <header class="card-header">
        <p class="card-header-title has-text-grey">
          {{ category }}
        </p>
      </header>

      <!-- <a :href="'/products/' + id"> -->
      <div class="card-content">
        <div class="media">
          <div class="media-left">
            <figure class="image column">
              <img
                class="is-vcentered"
                style="object-fit: cover; max-height: 120px"
                :src="baseURL + image"
                alt="#"
              />
            </figure>
          </div>
          <div class="media-content">
            <p class="title is-4">{{ name }}</p>
            <p class="subtitle is-6">{{ description }}</p>
            <p class="subtitle is-5 has-text-grey">{{ price }} руб.</p>
          </div>
          <button class="card-header-icon" @click="deleteProductById(id)">
            <span class="icon" aria-hidden="true">
              <font-awesome-icon :icon="['fas', 'trash']" />
            </span>
          </button>
        </div>
      </div>
      <!-- </a> -->

      <footer class="card-footer">
        <a href="#" class="card-footer-item">Изменить</a>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProductCard',

  props: {
    id: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    description: {
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
    category: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      baseURL:
        this.$axios.defaults.baseURL + 'static/' ||
        'http://localhost:3000/static/',
    }
  },
  methods: {
    deleteProductById(id) {
      this.$emit('delete-product', id)
    },
  },
}
</script>
