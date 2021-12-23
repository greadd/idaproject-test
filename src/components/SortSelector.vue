<template>
  <div class="select-wrapper">
    <select
      aria-label="sort"
      :value="sortBy"
      @input="$emit('sort', $event.target.value)"
    >
      <option value="default">По умолчанию</option>
      <option value="alphabet">По алфавиту</option>
      <option value="price-desc">По убыванию цены</option>
      <option value="price-asc">По возрастанию цены</option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    sortBy: {
      type: String,
      default: 'default'
    }
  }
}
</script>

<style lang="scss" scoped>
.select-wrapper {
  text-align: right;
  max-height: 35px;
  margin-bottom: 16px;
  &::after {
    content: '';
    color: #0ebeff;
    right: 16px;
    top: 15px;
    height: 5px;
    width: 5px;
    position: absolute;
    border: solid #b4b4b4;
    border-width: 0 1px 1px 0;
    display: inline-block;
    padding: 2px;
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
  }
  select {
    height: 35px;
  }
}
</style>
