<template>
  <div id="app">
    <div class="container">
      <div class="content">
        <CreateProductForm @create="addProduct" />
        <ProductList
          :sort-by="sortBy"
          :products="sortedProducts"
          :loading="loading"
          @sort="sortProducts"
          @delete-product="deleteProduct"
        />
      </div>
    </div>
  </div>
</template>

<script>
import CreateProductForm from './components/CreateProductForm.vue'
import ProductList from './components/ProductList.vue'
import { initialProducts } from '@/data/initial.js'

export default {
  name: 'App',
  components: { CreateProductForm, ProductList },
  data() {
    return {
      products: initialProducts,
      sortBy: 'default',
      loading: true
    }
  },
  computed: {
    sortedProducts() {
      const products = JSON.parse(JSON.stringify(this.products))
      if (this.sortBy !== 'default') {
        return products.sort((a, b) => {
          if (this.sortBy === 'price-desc') {
            return b.price - a.price
          }
          if (this.sortBy === 'price-asc') {
            return a.price - b.price
          }
          if (this.sortBy === 'alphabet') {
            return a.title.localeCompare(b.title)
          }
        })
      } else {
        return this.products
      }
    }
  },
  mounted() {
    const products = JSON.parse(localStorage.getItem('products'))
    const sortBy = localStorage.getItem('sort-by')
    setTimeout(() => {
      if (products) {
        this.products = products
      }

      if (sortBy) {
        this.sortBy = sortBy
      }

      this.loading = false
    }, 1000)
  },
  beforeUpdate() {
    localStorage.setItem('products', JSON.stringify(this.products))
    localStorage.setItem('sort-by', this.sortBy)
  },
  methods: {
    addProduct(product) {
      this.products.push(product)
    },
    deleteProduct(id) {
      this.products = this.products.filter((p) => p.id !== id)
    },
    sortProducts(sortBy) {
      this.sortBy = sortBy
    }
  }
}
</script>

<style lang="scss">
.content {
  display: flex;
  flex-wrap: wrap;
}
</style>
