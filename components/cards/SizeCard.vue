<template>
  <div class="card mb-4">
    <header class="card-header">
      <p class="card-header-title">{{ name }}</p>
      <button class="card-header-icon" @click="updateSizeById(id, name)">
        <span class="icon">
          <font-awesome-icon :icon="['fas', 'pencil-alt']" />
        </span>
      </button>
      <button class="card-header-icon" @click="deleteSizeById(id)">
        <span class="icon">
          <font-awesome-icon :icon="['fas', 'trash']" />
        </span>
      </button>
    </header>
  </div>
</template>

<script>
export default {
  name: 'SizeCard',

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
    updateSizeById(id, name) {
      this.$emit('update-size', id, name)
    },
    deleteSizeById(id) {
      this.$emit('delete-size', id)
    },
  },
}
</script>
