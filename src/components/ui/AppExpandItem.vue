<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  number: {
    type: String,
    default: ''
  },
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['toggle'])

const toggle = () => {
  emit('toggle', !props.isOpen)
}
</script>

<template>
  <div class="app-expand-item" :class="{ 'app-expand-item--active': isOpen }">
    <div class="app-expand-item__header" @click="toggle">
      <span v-if="number" class="app-expand-item__number">{{ number }}</span>
      <h3 class="app-expand-item__title">{{ title }}</h3>
      <span class="app-expand-item__toggle">
        {{ isOpen ? '×' : '+' }}
      </span>
    </div>
    <transition name="expand">
      <div v-show="isOpen" class="app-expand-item__content">
        <slot />
      </div>
    </transition>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-expand-item {
  border-bottom: 1px solid $color-dark-light;
  padding: $spacing-xl 0;
  transition: border-color $transition-base;
}

.app-expand-item--active {
  border-bottom-color: $color-dark-light;
}

.app-expand-item__header {
  display: flex;
  align-items: flex-start;
  gap: $spacing-lg;
  cursor: pointer;
  user-select: none;
}

.app-expand-item__number {
  font-size: $font-size-2xl;
  font-weight: $font-weight-bold;
  color: $color-primary;
  min-width: 60px;
  flex-shrink: 0;
  
  @include respond-to(max-sm) {
    min-width: 40px;
    font-size: $font-size-xl;
  }
}

.app-expand-item__title {
  @include heading-small;
  font-size: $font-size-xl;
  color: $color-light;
  margin: 0;
  flex: 1;
  text-align: left;
}

.app-expand-item__toggle {
  font-size: $font-size-2xl;
  font-weight: $font-weight-normal;
  color: $color-light;
  width: 36px;
  height: 36px;
  @include flex-center;
  border-radius: $radius-full;
  transition: all $transition-base;
  flex-shrink: 0;
  
  &:hover {
    background-color: $color-dark-light;
  }
}

.app-expand-item__content {
  padding-top: $spacing-lg;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  
  p {
    margin: 0;
  }
}

.expand-enter-active,
.expand-leave-active {
  transition: height $transition-base;
  overflow: hidden;
}

.expand-enter-from,
.expand-leave-to {
  height: 0;
}

.expand-enter-to,
.expand-leave-from {
  height: auto;
}
</style>