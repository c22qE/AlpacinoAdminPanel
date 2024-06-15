<template>
  <div>
    <div class="mb-4 file is-centered is-boxed is-medium is-primary has-name">
      <label class="file-label">
        <input
          ref="imageFile"
          class="file-input"
          type="file"
          name="resume"
          @change="emitInput"
        />
        <span class="file-cta">
          <span class="file-icon">
            <font-awesome-icon :icon="['fas', 'image']" />
          </span>
          <span class="file-label"> Выберите изображение продукта</span>
          <p class="is-small is-danger">с размером не меньше 900х600px</p>
        </span>

        <div v-if="product.image" class="file-name" style="max-width: 100%">
          Выбрано: {{ product.image.name }}
        </div>
        <div v-else class="file-name" style="max-width: 100%">
          Не выбранно...
        </div>
      </label>
    </div>
    <div class="field">
      <label class="label">Категория продукта</label>
      <div class="control">
        <div class="select">
          <select v-model="product.category" @change="emitInput">
            <!-- get catogories -->
            <option v-for="category in categories" :key="category.id">
              {{ category.name }}
            </option>
          </select>
        </div>
      </div>
    </div>
    <div class="field">
      <label class="label">Название продукта</label>
      <div class="control">
        <input
          v-model="product.name"
          class="input"
          type="text"
          maxlength="40"
          placeholder="Например: Грибная"
          @input="emitInput"
        />
      </div>
    </div>

    <div class="field">
      <label class="label">Описание</label>
      <textarea
        v-model="product.description"
        class="textarea"
        placeholder="Например: Шампиньоны, моцарелла, помидоры"
        rows="3"
        maxlength="255"
        @input="emitInput"
      ></textarea>
    </div>
    <div class="field">
      <h3 class="label">Цена продукта</h3>
      <div class="control">
        <input
          v-model="product.price"
          class="input"
          type="number"
          maxlength="5"
          placeholder="Например: 500"
          @input="emitInput"
        />
      </div>
    </div>

    <div class="field">
      <label class="label">Опции продукта (размеры, литры)</label>
      <div class="field is-grouped is-grouped-multiline">
        <div
          v-for="size in sizes"
          :key="size.id"
          class="control"
          @click="toggleTag(size.id)"
        >
          <div class="tags has-addons">
            <a
              class="tag is-medium"
              :class="{ 'is-info': product.sizes.includes(size.id) }"
              >{{ size.name }}</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    categories: {
      type: Set,
      default: Set,
    },
    sizes: {
      type: Set,
      default: Set,
    },
  },
  data() {
    return {
      product: {
        name: '',
        description: '',
        price: 0,
        image: null,
        category: this.categories[0].name,
        sizes: [],
      },

      handler() {
        this.$emit('input', this.product)
      },
    }
  },
  methods: {
    setFile() {
      const file = this.$refs.imageFile.files[0]

      if (!file) return

      this.product.image = file
      this.$emit('image-selected', this.product)
      this.$emit('input', this.product)
    },

    toggleTag(tagId) {
      if (!this.product.sizes.includes(tagId)) {
        this.product.sizes.push(tagId)
      } else {
        const index = this.product.sizes.indexOf(tagId)
        this.product.sizes.splice(index, 1)
      }
      this.emitInput()
    },

    emitInput() {
      this.setFile()
      this.$emit('input', this.product)
    },
  },
}
</script>
