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
  variant: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'secondary', 'dark', 'light'].includes(value)
  }
})

const slots = useSlots()
const hasActions = computed(() => !!slots.actions)
const hasImage = computed(() => !!slots.image)
</script>

<template>
  <section :class="['app-banner', `app-banner--${variant}`]">
    <div class="app-banner__content">
      <h2 v-if="title" class="app-banner__title">{{ title }}</h2>
      <p v-if="description" class="app-banner__description">{{ description }}</p>
      <div v-if="hasActions" class="app-banner__actions">
        <slot name="actions" />
      </div>
    </div>
    <div v-if="hasImage" class="app-banner__image">
      <slot name="image" />
    </div>
  </section>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-banner {
  display: flex;
  align-items: center;
  padding: $spacing-4xl;
  border-radius: $radius-xl;
  gap: $spacing-xl;
  
  @include respond-to(max-md) {
    flex-direction: column;
    padding: $spacing-3xl;
  }
}

.app-banner--primary {
  background: linear-gradient(135deg, $color-secondary 0%, $color-secondary-dark 100%);
  color: $color-light;
}

.app-banner--secondary {
  background: linear-gradient(135deg, $color-success 0%, $color-success-dark 100%);
  color: $color-light;
}

.app-banner--dark {
  background-color: $color-dark-light;
  color: $color-light;
  border: 1px solid $color-dark-lighter;
}

.app-banner--light {
  background-color: $color-light-gray;
  color: $color-text;
  border: 1px solid $color-gray;
}

.app-banner__content {
  flex: 1;
}

.app-banner__title {
  @include heading-medium;
  margin-bottom: $spacing-lg;
}

.app-banner__description {
  font-size: 18px;
  line-height: $line-height-relaxed;
  margin-bottom: $spacing-xl;
  opacity: 0.9;
}

.app-banner__actions {
  display: flex;
  gap: $spacing-md;
  flex-wrap: wrap;
}

.app-banner__image {
  flex: 1;
  text-align: center;
  
  img {
    max-width: 100%;
    border-radius: $radius-md;
  }
}
</style>