<template>
  <div>
    <CreateDialog
      :show="isDialog"
      :title="isEdit ? dialogTitleEdit : dialogTitle"
      :is-edit="isEdit"
      @create="createSize"
      @update="updateSize"
      @cancel="closeDialog"
    >
      <SizeForm
        :id="size.id"
        :name="size.name"
        :is-edit="isEdit"
        @onSize="setSize"
      ></SizeForm>
    </CreateDialog>

    <section class="section">
      <h2 class="title is-3 has-text-grey">
        Опции продуктов <br />
        (размеры пиццы, сока т.п.)
      </h2>
      <!-- Buttons -->
      <div class="buttons has-text-centered">
        <div class="button is-danger is-medium" @click="deleteSizes()">
          Удалить все опции
        </div>
        <div class="button is-success is-medium" @click="openDialog()">
          Создать новую опцию продукта
        </div>
      </div>

      <!-- Список опции -->
      <list-sizes
        :sizes="sizes"
        @update-size="openDialogEdit"
        @delete-size="deleteSizeById"
      ></list-sizes>

      <!-- Error message -->
      <div v-if="isError" class="column">
        <h2 class="subtitle is-3 has-text-danger">Произошла ошибка</h2>
        <div class="mt-2 button is-primary" @click="getSizes()">
          Обновить страницу
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import CreateDialog from '@/components/modals/CreateDialog.vue'
import ListSizes from '@/components/lists/ListSizes.vue'
import SizeForm from '@/components/forms/SizeForm.vue'

export default {
  name: 'OptionsPage',
  components: {
    CreateDialog,
    ListSizes,
    SizeForm,
  },
  data() {
    return {
      dialogTitle: 'Создание опции продукта',
      dialogTitleEdit: 'Редактирование опции продукта',

      isDialog: false,
      isEdit: false, // редактирование опции
      isError: false,

      sizes: [],

      size: {
        id: null,
        name: '',
      },
    }
  },

  head: {
    title: 'Админ панель ALPACINO ',
  },

  mounted() {
    this.getSizes()
  },

  methods: {
    async createSize() {
      await this.$axios
        .post('/sizes', this.size)
        .then((response) => {
          this.closeDialog()
          this.getSizes()
        })
        .catch((error) => {
          throw error
        })
    },

    async updateSize() {
      await this.$axios
        .put('/sizes/' + this.size.id, { name: this.size.name })
        .then((response) => {
          this.closeDialog()
          this.getSizes()
        })
        .catch((error) => {
          throw error
        })
    },

    async getSizes() {
      await this.$axios
        .get('/sizes')

        .then((response) => {
          this.isError = false
          this.sizes = response.data
        })
        .catch((error) => {
          // throw error
          this.isError = true
          // eslint-disable-next-line no-console
          console.log('Похоже, что сервер недоступен: ' + error)
        })
    },

    async deleteSizes() {
      await this.$axios
        .delete('/sizes')
        .then((response) => {
          this.sizes = []
        })
        .catch((error) => {
          throw error
        })
    },

    async deleteSizeById(id) {
      await this.$axios
        .delete('/sizes/' + id)
        .then((response) => {
          // Удаляем опцию из массива, для отображение исчезновения
          const updatedSizes = this.sizes.filter((size) => size.id !== id)
          this.sizes = updatedSizes
        })
        .catch((error) => {
          throw error
        })
    },

    // Dialog

    // update
    openDialogEdit(size) {
      this.isEdit = true
      this.isDialog = true

      this.size = size
    },

    openDialog() {
      this.isDialog = true
    },
    closeDialog() {
      this.isDialog = false
      this.isEdit = false
    },

    // Events
    setSize(size) {
      this.size = size
    },
  },
}
</script>
