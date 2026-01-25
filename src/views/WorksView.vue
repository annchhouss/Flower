<script setup>
import { ref } from 'vue'
import { RouterLink } from 'vue-router'
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'
import AppButton from '@/components/ui/AppButton.vue'
import AppContactModal from '@/components/ui/AppContactModal.vue'
import { worksHero, worksIntro, worksCategories } from '@models/worksData'

const showContactModal = ref(false)

const handleLogin = () => {
  console.log('Login clicked')
}

const handleContact = () => {
  showContactModal.value = true
}
</script>

<template>
  <div class="works-view">
    <AppHeader @login="handleLogin" />
    
    <main>
      <!-- Hero Section -->
      <section class="hero-section">
        <div class="hero-overlay">
          <div class="container">
            <h1 class="hero-title">{{ worksHero.title }}</h1>
            <p class="hero-description">{{ worksHero.description }}</p>
            <AppButton variant="outline" size="large" class="hero-button">
              Посмотреть
            </AppButton>
          </div>
        </div>
      </section>

      <!-- Intro Section -->
      <section class="intro-section section">
        <div class="container">
          <h2 class="intro-title">{{ worksIntro.title }}</h2>
          <p class="intro-description">{{ worksIntro.description }}</p>
          <div class="intro-subtitle">
            {{ worksIntro.subtitle }}
          </div>
        </div>
      </section>

      <!-- Categories Section -->
      <section class="categories-section section">
        <div class="container">
          <!-- Свадьбы -->
          <div class="category-block">
            <div class="category-content">
              <h2 class="category-main-title">{{ worksCategories[0].mainTitle }}</h2>
              <p class="category-description">{{ worksCategories[0].description }}</p>
            </div>
          </div>

          <!-- Корпоративы -->
          <div class="category-block category-block--reverse">
            <div class="category-image">
              <img :src="worksCategories[1].image" :alt="worksCategories[1].title" />
            </div>
            <div class="category-content">
              <h3 class="category-title">{{ worksCategories[1].title }}</h3>
              <AppButton variant="primary" size="medium" class="category-button">
                Перейти
              </AppButton>
            </div>
          </div>

          <div class="category-block">
            <div class="category-content">
              <h2 class="category-main-title">{{ worksCategories[1].mainTitle }}</h2>
              <p class="category-description">{{ worksCategories[1].description }}</p>
            </div>
          </div>

          <!-- Дни рождения -->
          <div class="category-block category-block--reverse">
            <div class="category-content">
              <h3 class="category-title">{{ worksCategories[2].title }}</h3>
              <AppButton variant="primary" size="medium" class="category-button">
                Перейти
              </AppButton>
            </div>
            <div class="category-image">
              <img :src="worksCategories[2].image" :alt="worksCategories[2].title" />
            </div>
          </div>

          <div class="category-block">
            <div class="category-content">
              <h2 class="category-main-title">{{ worksCategories[2].mainTitle }}</h2>
              <p class="category-description">{{ worksCategories[2].description }}</p>
            </div>
          </div>

          <!-- Сервировки -->
          <div class="category-block category-block--reverse">
            <div class="category-content">
              <h3 class="category-title">{{ worksCategories[3].title }}</h3>
              <AppButton variant="primary" size="medium" class="category-button">
                Перейти
              </AppButton>
            </div>
            <div class="category-image">
              <img :src="worksCategories[3].image" :alt="worksCategories[3].title" />
            </div>
          </div>

          <div class="category-block">
            <div class="category-content">
              <h2 class="category-main-title">{{ worksCategories[3].mainTitle }}</h2>
              <p class="category-description">{{ worksCategories[3].description }}</p>
              <AppButton variant="outline" size="medium" class="category-button">
                Посмотреть все работы
              </AppButton>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact Banner -->
      <section class="contact-banner section">
        <div class="contact-banner__overlay">
          <div class="container">
            <div class="contact-banner__content">
              <p class="contact-banner__logo">ДЛЯ ТЕБЯ</p>
              <h2 class="contact-banner__title">Позвольте нам воплотить ваши идеи в атмосфере.</h2>
              <p class="contact-banner__description">Мы гарантируем безупречный результат — и в интерьере, и в декоре мероприятия. Слаженная работа нашей многопрофильной команды обеспечит целостное и яркое воплощение вашей идеи</p>
              <AppButton variant="outline" size="large" class="contact-banner__button" @click="handleContact">
                Сотрудничать
              </AppButton>
            </div>
          </div>
        </div>
      </section>
    </main>
    
    <AppFooter />
    
    <!-- Contact Modal -->
    <AppContactModal 
      :is-open="showContactModal" 
      @close="showContactModal = false" 
    />
  </div>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.works-view {
  min-height: 100vh;
  flex-decoration: column;
  background: $color-dark;
}

.hero-section {
  position: relative;
  width: 100%;
  min-height: 400px;
  background-image: url('/src/assets/images/main-banner.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  
  @include respond-to(max-sm) {
    min-height: 300px;
  }
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(26, 26, 26, 0.7);
  display: flex;
  align-items: center;
  padding: $spacing-4xl 0;
}

.hero-title {
  @include heading-large;
  color: $color-light;
  text-align: center;
  margin-bottom: $spacing-lg;
}

.hero-description {
  font-size: 18px;
  color: $color-light;
  text-align: center;
  max-width: 800px;
  margin: 0 auto $spacing-xl;
  line-height: $line-height-relaxed;
}

.hero-button {
  display: block;
  margin: 0 auto;
}

.intro-section {
  padding: $spacing-4xl 0;
  border-top: 1px solid $color-dark-light;
  border-bottom: 1px solid $color-dark-light;
}

.intro-title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-lg;
  text-align: left;
}

.intro-description {
  font-size: 18px;
  color: $color-text-light;
  margin-bottom: $spacing-xl;
  line-height: $line-height-relaxed;
  max-width: 900px;
}

.intro-subtitle {
  @include text-base;
  color: $color-text-light;
  padding: $spacing-lg;
  background: $color-dark-light;
  border-radius: $radius-md;
  border: 1px solid $color-dark-lighter;
}

.categories-section {
  padding: $spacing-4xl 0;
}

.category-block {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: $spacing-4xl;
  margin-bottom: $spacing-4xl;
  align-items: center;
  
  @include respond-to(max-md) {
    grid-template-columns: 1fr;
    gap: $spacing-xl;
  }
  
  &--reverse {
    direction: rtl;
    
    @include respond-to(max-md) {
      direction: ltr;
    }
    
    > * {
      direction: ltr;
    }
  }
  
  &:last-child {
    margin-bottom: 0;
  }
}

.category-image {
  width: 100%;
  height: 500px;
  overflow: hidden;
  border-radius: $radius-lg;
  
  @include respond-to(max-sm) {
    height: 300px;
  }
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.category-content {
  display: flex;
  flex-direction: column;
  gap: $spacing-lg;
}

.category-title {
  @include heading-medium;
  color: $color-light;
  margin: 0;
}

.category-main-title {
  @include heading-large;
  color: $color-light;
  margin-bottom: $spacing-lg;
}

.category-description {
  font-size: 18px;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  margin: 0;
}

.category-button {
  align-self: flex-start;
  margin-top: $spacing-md;
}

.contact-banner {
  padding: 0;
  position: relative;
  min-height: 400px;
  background-image: url('/src/assets/images/main-banner.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  
  @include respond-to(max-sm) {
    min-height: 350px;
  }
}

.contact-banner__overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(26, 26, 26, 0.7);
  display: flex;
  align-items: center;
  padding: $spacing-4xl 0;
}

.contact-banner__content {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

.contact-banner__logo {
  font-family: $font-family-heading;
  font-size: $font-size-2xl;
  color: $color-light;
  margin-bottom: $spacing-lg;
  opacity: 0.9;
  text-transform: uppercase;
  letter-spacing: 2px;
}

.contact-banner__title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-lg;
}

.contact-banner__description {
  font-size: 18px;
  color: $color-text-light;
  margin-bottom: $spacing-xl;
  line-height: $line-height-relaxed;
}

.contact-banner__button {
  margin-top: $spacing-lg;
}
</style>
