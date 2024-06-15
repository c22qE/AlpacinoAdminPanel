<template>
  <div>
    <!-- Dialog create product -->
    <CreateDialog
      :show="isDialog"
      :title="dialogTitle"
      @create="createProduct"
      @cancel="closeDialog"
    >
      <ProductForm
        v-model="product"
        :categories="categories"
        :sizes="sizes"
        @input="updateProduct"
      ></ProductForm>
    </CreateDialog>

    <section class="section">
      <h2 class="title is-3 has-text-grey">Продукты</h2>
      <!-- Buttons -->
      <div class="buttons has-text-centered">
        <div class="button is-danger is-medium" @click="deleteProducts()">
          Удалить все продукты
        </div>
        <div class="button is-success is-medium" @click="openDialog()">
          Создать новый продукт
        </div>
      </div>

      <list-products
        :products="products"
        :categories="categories"
        @delete-product="deleteProductById"
      ></list-products>

      <!-- Error message -->
      <div v-if="isError" class="column">
        <h2 class="subtitle is-3 has-text-danger">Произошла ошибка</h2>
        <div class="mt-2 button is-primary" @click="getProducts()">
          Обновить страницу
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import ListProducts from '@/components/lists/ListProducts.vue'
import CreateDialog from '@/components/modals/CreateDialog.vue'
import ProductForm from '@/components/forms/ProductForm.vue'

export default {
  name: 'ProductsPage',
  components: {
    ListProducts,
    CreateDialog,
    ProductForm,
  },
  data() {
    return {
      dialogTitle: 'Создание продукта',
      isDialog: false,
      isError: false,

      products: [],
      categories: [],
      sizes: [],

      product: {
        name: '',
        description: '',
        price: 0,
        image: null,
        category: null,
        sizes: [],
      },
    }
  },

  head: {
    title: 'Админ панель ALPACINO ',
  },

  mounted() {
    this.getProductsAndCategories()
    this.getSizes()
  },

  methods: {
    async createProduct() {
      const formData = new FormData()

      formData.append('name', this.product.name)
      formData.append('description', this.product.description)
      formData.append('price', this.product.price)
      formData.append('img', this.product.image)
      formData.append('category', this.product.category)
      formData.append('sizes', this.product.sizes)

      await this.$axios
        .post('/products', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        })
        .then((response) => {
          this.getProductsAndCategories()
          this.closeDialog()
        })
        .catch((error) => {
          throw error
        })
    },

    async getProductsAndCategories() {
      await this.$axios
        .get('/products')

        .then((response) => {
          this.isError = false

          this.products = response.data.products
          this.categories = response.data.categories

          this.product.category = this.categories[0].name
        })
        .catch((error) => {
          // throw error
          this.isError = true
          // eslint-disable-next-line no-console
          console.log('Похоже, что сервер недоступен: ' + error)
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

    async deleteProducts() {
      await this.$axios
        .delete('/products')
        .then((response) => {
          this.products = []
        })
        .catch((error) => {
          throw error
        })
    },

    async deleteProductById(id, index) {
      await this.$axios
        .delete('/products/' + id)
        .then((response) => {
          // Удаляем продукт из массива, для отображение исчезновения
          const updatedProducts = this.products.filter(
            (product) => product.id !== id
          )
          this.products = updatedProducts
        })
        .catch((error) => {
          throw error
        })
    },

    // Dialog
    openDialog() {
      this.isDialog = true
    },

    closeDialog() {
      this.isDialog = false
    },

    // Events
    updateProduct(updatedProduct) {
      this.product = updatedProduct
    },
  },
}
</script>
