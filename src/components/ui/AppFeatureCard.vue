<script setup>
import { computed, useSlots } from 'vue'

const props = defineProps({
  title: {
    type: String,
    default: ''
  },
  description: {
    type: String,
    default: ''
  },
  icon: {
    type: String,
    default: ''
  }
})

const slots = useSlots()
const hasIconSlot = computed(() => !!slots.icon)
</script>

<template>
  <div class="app-feature-card">
    <div class="app-feature-card__icon" v-if="icon || hasIconSlot">
      <slot name="icon">
        <span class="icon-text">{{ icon }}</span>
      </slot>
    </div>
    <div class="app-feature-card__content">
      <h3 class="app-feature-card__title" v-if="title">{{ title }}</h3>
      <p class="app-feature-card__description" v-if="description">{{ description }}</p>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-feature-card {
  @include card;
  padding: $spacing-xl;
  text-align: left;
  height: 100%;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  gap: $spacing-lg;
  background: $color-dark-light;
  border: 1px solid $color-dark-lighter;
  
  @include respond-to(max-sm) {
    flex-direction: column;
    text-align: center;
  }
}

.app-feature-card__icon {
  width: 60px;
  height: 60px;
  background: $color-dark;
  border-radius: $radius-md;
  @include flex-center;
  flex-shrink: 0;
  color: $color-primary;
  font-size: $font-size-2xl;
  
  @include respond-to(max-sm) {
    margin: 0 auto;
  }
}

.icon-text {
  font-size: $font-size-2xl;
}

.app-feature-card__content {
  flex: 1;
}

.app-feature-card__title {
  @include heading-small;
  margin-bottom: $spacing-md;
  color: $color-light;
  font-size: $font-size-xl;
}

.app-feature-card__description {
  color: $color-text-light;
  line-height: $line-height-relaxed;
  @include text-base;
  margin: 0;
}
</style>