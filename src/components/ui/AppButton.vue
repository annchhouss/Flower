<script setup>
import { computed } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps({
  variant: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'secondary', 'outline'].includes(value)
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large'].includes(value)
  },
  fullWidth: {
    type: Boolean,
    default: false
  },
  disabled: {
    type: Boolean,
    default: false
  },
  to: {
    type: String,
    default: null
  },
  as: {
    type: [String, Object],
    default: 'button'
  }
})

const emit = defineEmits(['click'])

const buttonClasses = computed(() => [
  'app-button',
  `app-button--${props.variant}`,
  `app-button--${props.size}`,
  { 
    'app-button--full': props.fullWidth,
    'app-button--disabled': props.disabled
  }
])

const handleClick = (event) => {
  if (!props.disabled) {
    emit('click', event)
  }
}

const isRouterLink = computed(() => {
  return props.to || props.as === RouterLink || (typeof props.as === 'string' && props.as.toLowerCase() === 'routerlink')
})

const component = computed(() => {
  if (isRouterLink.value) {
    return RouterLink
  }
  return props.as === 'button' || !props.as ? 'button' : props.as
})
</script>

<template>
  <component
    :is="component"
    :class="buttonClasses"
    :to="to"
    :disabled="disabled && !isRouterLink"
    @click="handleClick"
  >
    <slot />
  </component>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-button {
  @include button-base;
}

.app-button--primary {
  @include button-primary;
}

.app-button--secondary {
  @include button-secondary;
}

.app-button--outline {
  @include button-outline;
}

.app-button--small {
  padding: $spacing-sm $spacing-md;
  font-size: $font-size-sm;
}

.app-button--medium {
  padding: $spacing-md $spacing-lg;
  font-size: $font-size-lg;
}

.app-button--large {
  padding: $spacing-md $spacing-xl;
  font-size: $font-size-lg;
}

.app-button--full {
  width: 100%;
}

.app-button--disabled {
  opacity: 0.5;
  cursor: not-allowed;
  
  &:hover {
    transform: none !important;
    box-shadow: none !important;
  }
}
</style>