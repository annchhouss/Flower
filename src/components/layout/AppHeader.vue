<script setup>
import { computed } from 'vue'
import { useRoute } from 'vue-router'
import { RouterLink } from 'vue-router'
import AppButton from '@/components/ui/AppButton.vue'
import { menuItems } from '@models/homeData'

const emit = defineEmits(['login'])
const route = useRoute()

const handleLogin = () => {
  emit('login')
}

const isActive = (link) => {
  return route.path === link
}
</script>

<template>
  <header class="app-header">
    <div class="container">
      <div class="app-header__inner">
        <div class="app-header__logo">
          <slot name="logo">
            <RouterLink to="/">
              <img src="/src/assets/images/main-logo.png" alt="Logo">
            </RouterLink>
          </slot>
        </div>
        
        <nav class="app-header__nav">
          <ul class="app-header__menu">
            <li 
              v-for="item in menuItems" 
              :key="item.id" 
              class="app-header__menu-item"
            >
              <RouterLink 
                :to="item.link" 
                class="app-header__menu-link"
                :class="{ 'app-header__menu-link--active': isActive(item.link) }"
              >
                {{ item.title }}
              </RouterLink>
            </li>
          </ul>
        </nav>
        
        <div class="app-header__actions">
          <slot name="actions">
            <AppButton 
              size="medium" 
              @click="handleLogin"
            >
              Связаться с нами
            </AppButton>
          </slot>
        </div>
      </div>
    </div>
  </header>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-header {
  background: $color-dark;
  border-bottom: 1px solid $color-dark-lighter;
  position: sticky;
  top: 0;
  z-index: $z-index-sticky;
}

.app-header__inner {
  @include container;
  @include flex-between;
  padding: $spacing-lg $container-padding;
  
  @include respond-to(max-md) {
    flex-direction: column;
    gap: $spacing-lg;
  }
}

.app-header__logo {
  flex-shrink: 0;
  
  a {
    display: block;
  }
  
  img {
    height: 50px;
    width: auto;
  }
}

.app-header__nav {
  flex: 1;
  display: flex;
  justify-content: center;
  
  @include respond-to(max-md) {
    width: 100%;
  }
}

.app-header__menu {
  @include flex-center;
  list-style: none;
  margin: 0;
  padding: 0;
  gap: $spacing-xs;
  
  @include respond-to(max-sm) {
    flex-wrap: wrap;
    gap: $spacing-sm;
  }
}

.app-header__menu-item {
  list-style: none;
}

.app-header__menu-link {
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  font-family: $font-family-primary;
  color: $color-light;
  font-weight: $font-weight-medium;
  font-size: $font-size-lg;
  padding: $spacing-md $spacing-xl;
  transition: all $transition-base;
  border-radius: $radius-md;
  min-width: 140px;
  
  @include respond-to(max-sm) {
    min-width: auto;
    padding: $spacing-sm $spacing-md;
    font-size: $font-size-base;
  }
  
  &:hover,
  &.app-header__menu-link--active {
    background: $color-dark-light;
    color: $color-primary;
  }
}

.app-header__actions {
  display: flex;
  gap: $spacing-md;
  flex-shrink: 0;
  
  @include respond-to(max-md) {
    width: 100%;
    justify-content: center;
  }
}
</style>