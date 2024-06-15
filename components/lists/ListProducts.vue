<template>
  <div class="columns">
    <!-- Список продуктов 1 колонка -->
    <div class="column">
      <div v-for="(product, index) in products" :key="index">
        <ProductCard
          v-if="index % 2 === 0 && product"
          :id="product.id"
          :name="product.name"
          :description="product.description"
          :price="product.price"
          :image="product.img"
          :category="product.categoryId"
          @delete-product="deleteProductById"
        >
        </ProductCard>
      </div>
    </div>
    <!-- Список продуктов 2 колонка -->
    <div class="column">
      <div v-for="(product, index) in products" :key="index">
        <ProductCard
          v-if="index % 2 !== 0 && product"
          :id="product.id"
          :name="product.name"
          :description="product.description"
          :price="product.price"
          :image="product.img"
          :category="product.categoryId"
          @delete-product="deleteProductById"
        >
        </ProductCard>
      </div>
    </div>
  </div>
</template>

<script>
import ProductCard from '@/components/cards/ProductCard.vue'

export default {
  name: 'ListProducts',
  components: { ProductCard },
  props: {
    products: {
      type: Array,
      required: true,
    },
    categories: {
      type: Array,
      required: true,
    },
  },
  methods: {
    deleteProductById(id) {
      this.$emit('delete-product', id)
    },
  },
}
</script>
