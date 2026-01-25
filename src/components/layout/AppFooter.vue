<script setup>
import { footerData } from '@models/homeData'
</script>

<template>
  <footer class="app-footer">
    <div class="container">
      <div class="app-footer__top">
        <div class="app-footer__logo">
          <h2>{{ footerData.logo }}</h2>
          <p class="app-footer__copyright">{{ footerData.copyright }}</p>
        </div>
        
        <div class="app-footer__quick-links">
          <h3 class="app-footer__section-title">Быстрые ссылки</h3>
          <ul class="app-footer__links">
            <li v-for="link in footerData.quickLinks" :key="link.id">
              <a :href="link.link" class="app-footer__link">{{ link.title }}</a>
            </li>
          </ul>
        </div>
        
        <div class="app-footer__contacts">
          <h3 class="app-footer__section-title">Контакты</h3>
          <div class="app-footer__social">
            <a 
              v-for="social in footerData.socialLinks" 
              :key="social.id"
              :href="social.link"
              class="app-footer__social-link"
            >
              <span class="app-footer__social-icon">{{ social.icon }}</span>
              <span class="app-footer__social-text">{{ social.name }}</span>
            </a>
          </div>
        </div>
      </div>
      
      <div class="app-footer__bottom">
        <div class="app-footer__bottom-content">
          <p>Разработано с ❤️ в 2024</p>
        </div>
      </div>
    </div>
  </footer>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.app-footer {
  background-color: $color-dark-light;
  color: $color-light;
  padding-top: $spacing-4xl;
  margin-top: auto;
}

.app-footer__top {
  @include container;
  @include grid-auto-fit(250px, $spacing-xl);
  padding-bottom: $spacing-xl;
  border-bottom: 1px solid $color-dark-lighter;
}

.app-footer__logo h2 {
  @include heading-small;
  color: $color-light;
  margin-bottom: $spacing-md;
}

.app-footer__copyright {
  color: $color-text-lighter;
  font-size: $font-size-sm;
}

.app-footer__section-title {
  @include heading-small;
  font-size: $font-size-xl;
  margin-bottom: $spacing-lg;
  color: $color-light;
}

.app-footer__links {
  list-style: none;
  padding: 0;
  margin: 0;
}

.app-footer__links li {
  margin-bottom: $spacing-md;
}

.app-footer__link {
  color: $color-gray-dark;
  text-decoration: none;
  transition: color $transition-base;
  
  &:hover {
    color: $color-primary;
  }
}

.app-footer__social {
  flex-decoration: column;
  gap: $spacing-md;
}

.app-footer__social-link {
  @include flex-center;
  justify-content: flex-start;
  gap: $spacing-md;
  color: $color-gray-dark;
  text-decoration: none;
  transition: color $transition-base;
  
  &:hover {
    color: $color-primary;
  }
}

.app-footer__social-icon {
  font-size: $font-size-xl;
}

.app-footer__social-text {
  font-size: $font-size-base;
}

.app-footer__bottom {
  @include container;
  padding: $spacing-xl 0;
  text-align: center;
}

.app-footer__bottom-content {
  color: $color-text-lighter;
  font-size: $font-size-sm;
}
</style>