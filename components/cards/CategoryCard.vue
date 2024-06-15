<template>
  <div class="card mb-4">
    <header class="card-header">
      <p class="card-header-title">{{ name }}</p>
      <button class="card-header-icon" @click="updateCategoryById(id, name)">
        <span class="icon">
          <font-awesome-icon :icon="['fas', 'pencil-alt']" />
        </span>
      </button>
      <button class="card-header-icon" @click="deleteCategoryById(id)">
        <span class="icon">
          <font-awesome-icon :icon="['fas', 'trash']" />
        </span>
      </button>
    </header>
  </div>
</template>

<script>
export default {
  name: 'CategoryCard',

  props: {
    id: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
  },
  methods: {
    updateCategoryById(id, name) {
      this.$emit('update-category', id, name)
    },
    deleteCategoryById(id) {
      this.$emit('delete-category', id)
    },
  },
}
</script>
