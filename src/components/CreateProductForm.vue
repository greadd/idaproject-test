<template>
  <div class="wrapper">
    <h2 class="form-title">Добавление товара</h2>
    <form class="form" novalidate @submit.prevent="createProduct">
      <BaseInput
        id="name"
        v-model="newProduct.title.value"
        label="Наименование товара"
        placeholder="Введите наименование товара"
        :required="true"
        :invalid="newProduct.title.touched && titleInvalid"
        @blur="newProduct.title.touched = true"
      />
      <BaseTextarea
        id="description"
        v-model="newProduct.description.value"
        label="Описание товара"
        placeholder="Введите описание товара"
        :invalid="newProduct.description.touched && descriptionInvalid"
        @blur="newProduct.description.touched = true"
      />

      <BaseInput
        id="image"
        v-model="newProduct.image.value"
        label="Ссылка на изображение товара"
        placeholder="Введите ссылку"
        :required="true"
        :invalid="newProduct.image.touched && imageInvalid"
        @blur="newProduct.image.touched = true"
      />
      <BaseInput
        id="price"
        :value="formattedPrice"
        label="Цена товара"
        placeholder="Введите цену"
        :required="true"
        :invalid="newProduct.price.touched && priceInvalid"
        @blur="newProduct.price.touched = true"
        @input="handlePriceInput"
      />
      <button :disabled="formInvalid" type="submit">Добавить товар</button>
    </form>
  </div>
</template>

<script>
import BaseInput from './UI/BaseInput.vue'
import BaseTextarea from './UI/BaseTextarea.vue'
import { required } from '@/validators.js'

export default {
  components: {
    BaseInput,
    BaseTextarea
  },
  data() {
    return {
      newProduct: {
        title: {
          value: '',
          touched: false,
          validators: [required]
        },
        description: {
          value: '',
          touched: false,
          validators: []
        },
        image: {
          value: '',
          touched: false,
          validators: [required]
        },
        price: {
          value: null,
          touched: false,
          validators: [required]
        }
      }
    }
  },
  computed: {
    formattedPrice() {
      return (
        this.newProduct.price.value &&
        this.newProduct.price.value.toLocaleString()
      )
    },
    formInvalid() {
      return (
        this.titleInvalid ||
        this.descriptionInvalid ||
        this.imageInvalid ||
        this.priceInvalid
      )
    },
    titleInvalid() {
      return this.anyError('title')
    },
    descriptionInvalid() {
      return this.anyError('description')
    },
    imageInvalid() {
      return this.anyError('image')
    },
    priceInvalid() {
      return this.anyError('price')
    }
  },

  methods: {
    handlePriceInput(newValue) {
      this.newProduct.price.value = parseInt(newValue.replace(/\D/g, ''))
    },
    createProduct() {
      console.log(1)
      const product = {
        id: Date.now().toString(),
        title: this.newProduct.title.value,
        description: this.newProduct.description.value,
        image: this.newProduct.image.value,
        price: this.newProduct.price.value
      }
      this.$emit('create', product)
      this.resetForm()
    },
    anyError(field) {
      return this.newProduct[field].validators.some(
        (validator) => !validator(this.newProduct[field].value)
      )
    },
    resetForm() {
      this.newProduct = {
        title: {
          value: '',
          touched: false,
          validators: [required]
        },
        description: {
          value: '',
          touched: false,
          validators: []
        },
        image: {
          value: '',
          touched: false,
          validators: [required]
        },
        price: {
          value: null,
          touched: false,
          validators: [required]
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.wrapper {
  flex: 0 0 calc(25% - 8px);
  margin-right: 16px;
}

.form-title {
  font-size: 28px;
  line-height: 35px;
  font-weight: 600;
  margin-bottom: 16px;
}

.form {
  padding: 24px;
  background-color: $bg-element;
  box-shadow: $shadow-block;
  border-radius: $radius-default;
}

button {
  background-color: $bg-btn;
  box-shadow: $shadow-input;
  border-radius: $radius-higher;
  font-family: 'Inter', sans-serif;
  width: 100%;
  font-size: 12px;
  line-height: 15px;
  font-weight: 600;
  text-align: center;
  border: none;
  outline: none;
  color: #ffffff;
  padding: 10px 0;
  transition: all 0.3s;
  margin-top: 8px;

  &:disabled {
    color: $color-muted;
    background: $bg-btn-disabled;
  }
}

@media (max-width: 1200px) {
  .form-title {
    font-size: 24px;
  }
}

@media (max-width: 992px) {
  .wrapper {
    flex: 0 0 calc(100% / 3 - 8px);
    margin-right: 16px;
  }
}

@media (max-width: 768px) {
  .wrapper {
    flex: 0 0 calc(100%);
    margin-right: 0;
    margin-bottom: 16px;
  }
}
</style>
