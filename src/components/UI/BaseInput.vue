<template>
  <div class="input">
    <label :class="{ required }" :for="id">{{ label }}</label>
    <input
      :id="id"
      v-bind="$attrs"
      :value="value"
      :class="{ 'input-invalid': invalid }"
      :aria-describedby="`${id}-feedback`"
      autocomplete="off"
      @input="$emit('input', $event.target.value)"
      @blur="$emit('blur', $event)"
    />
    <transition name="slide-fade">
      <small v-if="invalid" :id="`${id}-feedback`" class="invalid-feedback"
        >Поле является обязательным</small
      >
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: [String, Number],
      default: ''
    },
    id: {
      type: String,
      default: ''
    },
    label: {
      type: String,
      default: ''
    },
    required: {
      type: Boolean,
      default: false
    },
    invalid: {
      type: Boolean,
      default: false
    }
  }
}
</script>

<style lang="scss" scoped>
.input {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  position: relative;

  label {
    color: $color-secondary;
    position: relative;
    font-size: 10px;
    line-height: 13px;
    margin-bottom: 4px;

    &.required::after {
      position: absolute;
      content: '';
      background: $color-danger;
      height: 4px;
      width: 4px;
      border-radius: $radius-default;
    }
  }
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.1s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateY(-4px);
  opacity: 0;
}
</style>
