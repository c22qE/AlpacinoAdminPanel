<template>
  <div class="column">
    <div v-for="(size, index) in sizes" :key="index">
      <SizeCard
        :id="size.id"
        :name="size.name"
        @update-size="updateSizeById(size)"
        @delete-size="deleteSizeById"
      >
      </SizeCard>
    </div>
  </div>
</template>

<script>
import SizeCard from '@/components/cards/SizeCard.vue'
export default {
  components: { SizeCard },

  props: {
    sizes: {
      type: Set,
      required: true,
    },
  },
  methods: {
    updateSizeById(size) {
      this.$emit('update-size', size)
    },
    deleteSizeById(id) {
      this.$emit('delete-size', id)
    },
  },
}
</script>
