<script setup>
import { ref } from 'vue'
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'
import AppButton from '@/components/ui/AppButton.vue'
import AppContactModal from '@/components/ui/AppContactModal.vue'
import { aboutHero, aboutAgency, aboutHistory } from '@models/aboutData'

const showContactModal = ref(false)

const handleLogin = () => {
  console.log('Login clicked')
}

const handleContact = () => {
  showContactModal.value = true
}
</script>

<template>
  <div class="about-view">
    <AppHeader @login="handleLogin" />
    
    <main>
      <!-- Hero Section -->
      <section class="hero-section">
        <div class="hero-overlay">
          <div class="container">
            <h1 class="hero-title">{{ aboutHero.title }}</h1>
            <p class="hero-description">{{ aboutHero.description }}</p>
          </div>
        </div>
      </section>

      <!-- Agency Section -->
      <section class="agency-section section">
        <div class="container">
          <div class="agency-content">
            <div class="agency-text">
              <h2 class="agency-title">{{ aboutAgency.title }}</h2>
              <p class="agency-description">{{ aboutAgency.description }}</p>
            </div>
            <div class="agency-image">
              <img src="/src/assets/images/main-banner.jpg" alt="Агентство Для тебя" />
            </div>
          </div>
        </div>
      </section>

      <!-- History Section -->
      <section class="history-section section">
        <div class="container">
          <h2 class="history-title">{{ aboutHistory.title }}</h2>
          <div class="history-steps">
            <div
              v-for="step in aboutHistory.steps"
              :key="step.id"
              class="history-step"
            >
              <div class="history-step__number">{{ step.number }}</div>
              <div class="history-step__content">
                <h3 class="history-step__title">{{ step.title }}</h3>
                <p class="history-step__description">{{ step.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Banner Section -->
      <section class="about-banner section">
        <div class="container">
          <div class="about-banner__content">
            <h2 class="about-banner__title">{{ aboutHistory.banner.title }}</h2>
            <p class="about-banner__description">{{ aboutHistory.banner.description }}</p>
            <div class="about-banner__cta">
              <p class="about-banner__welcome">Добро пожаловать в «Для Тебя»</p>
              <p class="about-banner__invite">Давайте создадим вашу историю вместе.</p>
              <AppButton variant="primary" size="large" class="about-banner__button" @click="handleContact">
                Начать проект
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

.about-view {
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
  border: 1px solid $color-dark-light;
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
  margin: 0 auto;
  line-height: $line-height-relaxed;
}

.agency-section {
  padding: $spacing-4xl 0;
  border-top: 1px solid $color-dark-light;
  border-bottom: 1px solid $color-dark-light;
}

.agency-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: $spacing-4xl;
  align-items: center;
  
  @include respond-to(max-md) {
    grid-template-columns: 1fr;
    gap: $spacing-3xl;
  }
}

.agency-text {
  display: flex;
  flex-direction: column;
  gap: $spacing-lg;
}

.agency-title {
  @include heading-medium;
  color: $color-light;
  margin: 0;
  text-align: left;
}

.agency-description {
  font-size: 18px;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  margin: 0;
}

.agency-image {
  width: 100%;
  height: 500px;
  border-radius: $radius-lg;
  overflow: hidden;
  border: 1px solid $color-dark-light;
  
  @include respond-to(max-sm) {
    height: 300px;
  }
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.history-section {
  padding: $spacing-4xl 0;
}

.history-title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-4xl;
  text-align: left;
}

.history-steps {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.history-step {
  display: flex;
  gap: $spacing-3xl;
  padding: $spacing-3xl 0;
  border-bottom: 1px solid $color-dark-light;
  
  &:last-child {
    border-bottom: none;
  }
  
  @include respond-to(max-sm) {
    flex-direction: column;
    gap: $spacing-lg;
    padding: $spacing-xl 0;
  }
}

.history-step__number {
  font-size: $font-size-5xl;
  font-weight: $font-weight-bold;
  color: $color-primary;
  font-family: $font-family-heading;
  line-height: 1;
  min-width: 100px;
  flex-shrink: 0;
  
  @include respond-to(max-sm) {
    font-size: $font-size-4xl;
    min-width: 60px;
  }
}

.history-step__content {
  flex: 1;
}

.history-step__title {
  @include heading-small;
  color: $color-primary;
  font-size: $font-size-xl;
  margin: 0 0 $spacing-lg 0;
}

.history-step__description {
  font-size: 18px;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  margin: 0;
}

.about-banner {
  padding: $spacing-4xl 0;
  border-top: 1px solid $color-dark-light;
}

.about-banner__content {
  max-width: 900px;
  margin: 0 auto;
  text-align: center;
}

.about-banner__title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-lg;
}

.about-banner__description {
  font-size: 18px;
  color: $color-text-light;
  margin-bottom: $spacing-4xl;
  line-height: $line-height-relaxed;
}

.about-banner__cta {
  padding: $spacing-3xl;
  background: $color-dark-light;
  border-radius: $radius-lg;
  border: 1px solid $color-dark-lighter;
}

.about-banner__welcome {
  @include text-base;
  color: $color-text-lighter;
  margin-bottom: $spacing-md;
}

.about-banner__invite {
  font-size: 18px;
  color: $color-light;
  margin-bottom: $spacing-xl;
}

.about-banner__button {
  margin-top: $spacing-lg;
}
</style>
