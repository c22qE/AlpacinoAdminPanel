<template>
  <div>
    <div class="field">
      <label class="label">Название опции продукта</label>
      <div class="control">
        <input
          v-if="isEdit"
          v-model="size.name"
          class="input"
          type="text"
          maxlength="5"
          @input="onSize"
        />
        <input
          v-else
          v-model="size.name"
          class="input"
          type="text"
          maxlength="5"
          placeholder="Например: 25см"
          @input="onSize"
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
      size: {
        id: this.id,
        name: this.name.trim(),
      },
    }
  },
  methods: {
    onSize() {
      this.$emit('onSize', this.size)
    },
  },
}
</script>
