<template>
  <div class="products">
    <SortSelector :sort-by="sortBy" @sort="(sortBy) => $emit('sort', sortBy)" />
    <LoadingSpinner v-if="loading" />
    <transition-group
      v-else-if="products.length"
      class="products-list"
      name="list"
      tag="div"
    >
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @delete-product="deleteProduct"
      />
    </transition-group>
    <transition v-else name="slide-fade">
      <div class="empty-block">Товаров нет</div>
    </transition>
  </div>
</template>

<script>
import ProductCard from '@/components/ProductCard.vue'
import SortSelector from './SortSelector.vue'
import LoadingSpinner from './UI/LoadingSpinner.vue'

export default {
  components: {
    ProductCard,
    SortSelector,
    LoadingSpinner
  },
  props: {
    products: {
      type: Array,
      default: () => {
        return []
      }
    },
    sortBy: {
      type: String,
      default: ''
    },
    loading: {
      type: Boolean,
      default: true
    }
  },

  methods: {
    deleteProduct(id) {
      this.$emit('delete-product', id)
    }
  }
}
</script>

<style lang="scss" scoped>
.products {
  flex: 0 0 calc(75% - 8px);
  position: relative;
  max-width: calc(75% - 8px);
  &-list {
    display: flex;
    flex-wrap: wrap;
  }
}

.empty-block {
  text-align: center;
  padding: 30px;
  background-color: $bg-element;
  border-radius: $radius-default;
  box-shadow: $shadow-block;
  font-size: 20px;
  font-weight: 600;
}

.list-enter-active {
  transition: all 0.3s;
}
.list-leave-active {
  transition: all 0.3s;
  position: absolute;
}
.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateX(-20px);
}

.slide-fade-enter-active {
  transition: all 0.2s ease;
}
.slide-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter,
.slide-fade-leave-to {
  opacity: 0;
}

@media (max-width: 992px) {
  .products {
    flex: 0 0 calc(100% / 3 * 2 - 8px);
    position: relative;
    max-width: calc(100% / 3 * 2 - 8px);
    &-list {
      flex-direction: column;
    }
  }
}

@media (max-width: 768px) {
  .products {
    flex: 0 0 calc(100%);
    position: relative;
    max-width: calc(100%);
    &-list {
      flex-direction: column;
    }
  }
}
</style>
