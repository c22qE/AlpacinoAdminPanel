<template>
  <div>
    <CreateDialog
      :show="isDialog"
      :title="isEdit ? dialogTitleEdit : dialogTitle"
      :is-edit="isEdit"
      @create="createCategory"
      @update="updateCategory"
      @cancel="closeDialog"
    >
      <CategoryForm
        :id="category.id"
        :name="category.name"
        :is-edit="isEdit"
        @onCategory="setCategory"
      ></CategoryForm>
    </CreateDialog>

    <section class="section">
      <h2 class="title is-3 has-text-grey">Категорий продуктов</h2>
      <!-- Buttons -->
      <div class="buttons has-text-centered">
        <div class="button is-danger is-medium" @click="deleteCategories()">
          Удалить все категорий
        </div>
        <div class="button is-success is-medium" @click="openDialog()">
          Создать новую категорию продуктов
        </div>
      </div>

      <!-- Список категории -->
      <list-categories
        :categories="categories"
        @update-category="openDialogEdit"
        @delete-category="deleteCategoryById"
      ></list-categories>

      <!-- Error message -->
      <div v-if="isError" class="column">
        <h2 class="subtitle is-3 has-text-danger">Произошла ошибка</h2>
        <div class="mt-2 button is-primary" @click="getCategories()">
          Обновить страницу
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import CreateDialog from '@/components/modals/CreateDialog.vue'
import ListCategories from '@/components/lists/ListCategories.vue'
import CategoryForm from '@/components/forms/CategoryForm.vue'

export default {
  name: 'CategoriesPage',
  components: {
    CreateDialog,
    ListCategories,
    CategoryForm,
  },
  data() {
    return {
      dialogTitle: 'Создание категорий продукта',
      dialogTitleEdit: 'Редактирование категорий продукта',

      isDialog: false,
      isEdit: false, // редактирование категории
      isError: false,

      categories: [],

      category: {
        id: null,
        name: '',
      },
    }
  },

  head: {
    title: 'Админ панель ALPACINO ',
  },

  mounted() {
    this.getCategories()
  },

  methods: {
    async createCategory() {
      await this.$axios
        .post('/categories', this.category)
        .then((response) => {
          this.closeDialog()
          this.getCategories()
        })
        .catch((error) => {
          throw error
        })
    },

    async updateCategory() {
      await this.$axios
        .put('/categories/' + this.category.id, { name: this.category.name })
        .then((response) => {
          this.closeDialog()
          this.getCategories()
        })
        .catch((error) => {
          throw error
        })
    },

    async getCategories() {
      await this.$axios
        .get('/categories')

        .then((response) => {
          this.isError = false
          this.categories = response.data
        })
        .catch((error) => {
          // throw error
          this.isError = true
          // eslint-disable-next-line no-console
          console.log('Похоже, что сервер недоступен: ' + error)
        })
    },

    async deleteCategories() {
      await this.$axios
        .delete('/categories')
        .then((response) => {
          this.categories = []
        })
        .catch((error) => {
          throw error
        })
    },

    async deleteCategoryById(id) {
      await this.$axios
        .delete('/categories/' + id)
        .then((response) => {
          // Удаляем категорию из массива, для отображение исчезновения
          const updatedCategories = this.categories.filter(
            (category) => category.id !== id
          )
          this.categories = updatedCategories
        })
        .catch((error) => {
          throw error
        })
    },

    // Dialog

    // update
    openDialogEdit(category) {
      this.isEdit = true
      this.isDialog = true

      this.category = category
    },

    openDialog() {
      this.isDialog = true
    },
    closeDialog() {
      this.isDialog = false
      this.isEdit = false
    },

    // Events
    setCategory(category) {
      this.category = category
    },
  },
}
</script>
