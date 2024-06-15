<template>
  <div>
    <div class="field">
      <label class="label">Название категорий продуктов</label>
      <div class="control">
        <input
          v-if="isEdit"
          v-model="category.name"
          class="input"
          type="text"
          @input="onCategory"
        />
        <input
          v-else
          v-model="category.name"
          class="input"
          type="text"
          placeholder="Например: Пицца"
          @input="onCategory"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },

    isEdit: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      category: {
        id: this.id,
        name: this.name.trim(),
      },
    }
  },
  methods: {
    onCategory() {
      this.$emit('onCategory', this.category)
    },
  },
}
</script>
