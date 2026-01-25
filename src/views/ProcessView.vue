<script setup>
import { ref } from 'vue'
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'
import AppButton from '@/components/ui/AppButton.vue'
import AppContactModal from '@/components/ui/AppContactModal.vue'
import { processHero, processIntro, processSteps } from '@models/processData'

const showContactModal = ref(false)

const handleLogin = () => {
  console.log('Login clicked')
}

const handleContact = () => {
  showContactModal.value = true
}
</script>

<template>
  <div class="process-view">
    <AppHeader @login="handleLogin" />
    
    <main>
      <!-- Hero Section -->
      <section class="hero-section">
        <div class="hero-overlay">
          <div class="container">
            <h1 class="hero-title">{{ processHero.title }}</h1>
            <p class="hero-description">{{ processHero.description }}</p>
          </div>
        </div>
      </section>

      <!-- Introduction Section -->
      <section class="intro-section section">
        <div class="container">
          <h2 class="intro-title">{{ processIntro.title }}</h2>
          <p class="intro-description">{{ processIntro.description }}</p>
          <div class="intro-subtitle">
            {{ processIntro.subtitle }}
          </div>
        </div>
      </section>

      <!-- Process Steps Grid -->
      <section class="steps-section section">
        <div class="container">
          <div class="steps-grid">
            <div
              v-for="step in processSteps"
              :key="step.id"
              class="step-card"
            >
              <div class="step-number">{{ step.number }}</div>
              <h3 class="step-title">{{ step.title }}</h3>
              <p class="step-description">{{ step.description }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Contact Banner -->
      <section class="contact-banner section">
        <div class="contact-banner__overlay">
          <div class="container">
            <div class="contact-banner__content">
              <p class="contact-banner__logo">для тебя</p>
              <h2 class="contact-banner__title">Спасибо за Ваш интерес к нашему Декор-агенству</h2>
              <p class="contact-banner__description">Мы будем рады услышать ваши идеи и обсудить, как создать неповторимую атмосферу вашего события. Выберите удобный способ связаться с нами:</p>
              <AppButton variant="primary" size="large" class="contact-banner__button" @click="handleContact">
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

.process-view {
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
  color: $color-primary;
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

.steps-section {
  padding: $spacing-4xl 0;
}

.steps-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 0;
  border-top: 1px solid $color-dark-light;
  border-left: 1px solid $color-dark-light;
  
  @include respond-to(max-md) {
    grid-template-columns: 1fr;
    border-left: none;
  }
}

.step-card {
  padding: $spacing-3xl;
  border-right: 1px solid $color-dark-light;
  border-bottom: 1px solid $color-dark-light;
  display: flex;
  flex-direction: column;
  gap: $spacing-lg;
  
  @include respond-to(max-md) {
    border-right: none;
  }
}

.step-number {
  font-size: $font-size-5xl;
  font-weight: $font-weight-bold;
  color: $color-primary;
  font-family: $font-family-heading;
  line-height: 1;
  
  @include respond-to(max-sm) {
    font-size: $font-size-4xl;
  }
}

.step-title {
  @include heading-small;
  color: $color-primary;
  font-size: $font-size-xl;
  margin: 0;
}

.step-description {
  @include text-base;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  margin: 0;
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
  font-style: italic;
  font-size: $font-size-xl;
  color: $color-light;
  margin-bottom: $spacing-lg;
  opacity: 0.9;
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
