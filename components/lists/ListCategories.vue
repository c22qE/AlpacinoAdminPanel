<template>
  <div class="column">
    <div v-for="(category, index) in categories" :key="index">
      <CategoryCard
        :id="category.id"
        :name="category.name"
        @update-category="updateCategoryById(category)"
        @delete-category="deleteCategoryById"
      >
      </CategoryCard>
    </div>
  </div>
</template>

<script>
import CategoryCard from '@/components/cards/CategoryCard.vue'
export default {
  components: { CategoryCard },

  props: {
    categories: {
      type: Set,
      required: true,
    },
  },
  methods: {
    updateCategoryById(category) {
      this.$emit('update-category', category)
    },
    deleteCategoryById(id) {
      this.$emit('delete-category', id)
    },
  },
}
</script>
