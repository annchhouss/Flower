<script setup>
import AppButton from '@/components/ui/AppButton.vue'
import { menuItems } from '@models/homeData'

const emit = defineEmits(['login'])

const handleLogin = () => {
  emit('login')
}
</script>

<template>
  <header class="app-header">
    <div class="container">
      <div class="app-header__inner">
        <div class="app-header__logo">
          <slot name="logo">
            <img src="/src/assets/images/main-logo.png" alt="Logo">
          </slot>
        </div>
        
        <nav class="app-header__nav">
          <ul class="app-header__menu">
            <li 
              v-for="item in menuItems" 
              :key="item.id" 
              class="app-header__menu-item"
            >
              <a 
                :href="item.link" 
                class="app-header__menu-link"
                :class="{ 'app-header__menu-link--active': item.active }"
              >
                {{ item.title }}
              </a>
            </li>
          </ul>
        </nav>
        
        <div class="app-header__actions">
          <slot name="actions">
            <AppButton 
              size="medium" 
              @click="handleLogin"
            >
              Связатсья с нами
            </AppButton>
          </slot>
        </div>
      </div>
    </div>
  </header>
</template>

<style scoped>
.app-header {
  background: #262626;
  border-bottom: 1px solid #676767;
  position: sticky;
  top: 0;
  z-index: 1000;
}

.app-header__inner {
  max-width: 1600px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin: 0 auto;
  padding: 20px;
}

.app-header__nav {
  flex: 1;
  display: flex;
  justify-content: center;
}

.app-header__menu {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.app-header__menu-link {
  display: flex;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  font-family: 'Barlow', sans-serif;
  color: #ffffff;
  font-weight: 500;
  font-size: 18px;
  padding: 14px 28px;
  transition: all 0.3s ease;
  width: 180px;
}

.app-header__menu-link:hover,
.app-header__menu-link--active {
  background: #3c3c3c;
  border-radius: 8px;
}

.app-header__actions {
  display: flex;
  gap: 16px;
}
</style>