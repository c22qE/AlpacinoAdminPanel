<template>
  <div v-if="show" :class="{ 'modal is-active': show }">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">{{ title }}</p>
        <button class="delete" aria-label="close" @click="cancel"></button>
      </header>
      <section class="modal-card-body">
        <slot></slot>
      </section>
      <footer class="modal-card-foot">
        <div class="buttons">
          <button v-if="!isEdit" class="button is-success" @click="create">
            Сохранить
          </button>
          <button v-if="isEdit" class="button is-success" @click="update">
            Изменить
          </button>
          <button class="button" @click="cancel">Отменить</button>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CreateDialog',
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      required: true,
    },
    isEdit: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    create() {
      this.$emit('create')
    },
    update() {
      this.$emit('update')
    },
    cancel() {
      this.$emit('cancel')
    },
  },
}
</script>
