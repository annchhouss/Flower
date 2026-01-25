<script setup>
import {ref, computed} from 'vue'
import { RouterLink } from 'vue-router'
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'
import AppButton from '@/components/ui/AppButton.vue'
import AppFeatureCard from '@/components/ui/AppFeatureCard.vue'
import AppExpandItem from '@/components/ui/AppExpandItem.vue'
import AppContactModal from '@/components/ui/AppContactModal.vue'
import {
  heroBanner,
  servicesSection,
  services,
  whyChooseUsBanner,
  features,
  faqSection,
  faqItems,
  contactSection,
  eventTypes
} from '@models/homeData'

const form = ref({
  name: '',
  email: '',
  eventType: 'wedding',
  budget: [10000, 50000],
  message: ''
})

const faqState = ref(faqItems.map(item => ({ ...item })))
const showContactModal = ref(false)

const handleLogin = () => {
  console.log('Login clicked')
}

const handleContact = () => {
  showContactModal.value = true
}

const handleSubmit = () => {
  console.log('Form submitted:', form.value)
  alert('Заявка отправлена! Мы свяжемся с вами в течение 24 часов.')
  resetForm()
}

const resetForm = () => {
  form.value = {
    name: '',
    email: '',
    eventType: 'wedding',
    budget: [10000, 50000],
    message: ''
  }
}

const toggleFaq = (index) => {
  faqState.value[index].open = !faqState.value[index].open
}
</script>

<template>
  <div class="home-view">
    <AppHeader @login="handleLogin" />
    <main>
      <!-- Hero Section -->
      <section class="hero-section">
        <div class="hero-overlay">
          <div class="container">
            <div class="hero-content">
              <h1 class="hero-title">
                <span class="hero-title__line">{{ heroBanner.title }}</span>
                <span class="hero-title__line">{{ heroBanner.subtitle }}</span>
              </h1>
              <div class="hero-categories">
                <button
                  v-for="category in heroBanner.categories"
                  :key="category.id"
                  class="category-button"
                >
                  {{ category.text }}
                </button>
              </div>
              <div class="hero-actions">
                <AppButton
                  v-for="button in heroBanner.buttons"
                  :key="button.id"
                  :variant="button.variant"
                  :size="button.size"
                  class="hero-button"
                  :to="button.id === 1 ? '/works' : null"
                  @click="button.id === 2 ? handleContact() : null"
                >
                  {{ button.text }}
                </AppButton>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Services Section -->
      <section class="services-section section">
        <div class="container">
          <h2 class="section-title">{{ servicesSection.title }}</h2>
          <p class="section-description">{{ servicesSection.description }}</p>
        </div>
      </section>

      <!-- Three Services Columns -->
      <section class="services-columns section">
        <div class="container">
          <div class="services-grid">
            <div
              v-for="service in services"
              :key="service.id"
              class="service-card"
            >
              <h3 class="service-card__title">{{ service.title }}</h3>
              <p class="service-card__description">{{ service.description }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Why Choose Us Banner -->
      <section class="why-choose-us section">
        <div class="why-choose-us__overlay">
          <div class="container">
            <h2 class="why-choose-us__title">{{ whyChooseUsBanner.title }}</h2>
            <p class="why-choose-us__description">{{ whyChooseUsBanner.description }}</p>
          </div>
        </div>
      </section>

      <!-- Features Section (4 cards in 2x2 grid) -->
      <section class="features-section section">
        <div class="container">
          <div class="features-grid">
            <AppFeatureCard
              v-for="feature in features"
              :key="feature.id"
              :title="feature.title"
              :description="feature.description"
              :icon="feature.icon"
            />
          </div>
        </div>
      </section>

      <!-- FAQ Section -->
      <section class="faq-banner section">
        <div class="faq-banner__overlay">
          <div class="container">
            <h2 class="faq-banner__title">{{ faqSection.title }}</h2>
            <p class="faq-banner__description">{{ faqSection.description }}</p>
          </div>
        </div>
      </section>

      <section class="faq-section section">
        <div class="container">
          <div class="faq-container">
            <AppExpandItem
              v-for="(item, index) in faqState"
              :key="item.id"
              :title="item.title"
              :number="item.number"
              :is-open="item.open"
              @toggle="toggleFaq(index)"
            >
              <p>{{ item.content }}</p>
            </AppExpandItem>
          </div>
        </div>
      </section>

      <!-- Contact Section -->
      <section class="contact-banner section">
        <div class="contact-banner__overlay">
          <div class="container">
            <div class="contact-banner__content">
              <p class="contact-banner__logo">для тебя</p>
              <h2 class="contact-banner__title">{{ contactSection.title }}</h2>
              <p class="contact-banner__description">{{ contactSection.description }}</p>
              <AppButton variant="primary" size="large" class="contact-banner__button">
                {{ contactSection.buttonText }}
              </AppButton>
            </div>
          </div>
        </div>
      </section>

      <section class="contact-form-section section">
        <div class="container">
          <form @submit.prevent="handleSubmit" class="contact-form">
            <div class="form-row">
              <div class="form-group">
                <label class="form-label">Имя</label>
                <input
                  v-model="form.name"
                  type="text"
                  class="form-input"
                  placeholder="Type here"
                  required
                />
              </div>
              
              <div class="form-group">
                <label class="form-label">Почта</label>
                <input
                  v-model="form.email"
                  type="email"
                  class="form-input"
                  placeholder="Type here"
                  required
                />
              </div>
            </div>
            
            <div class="form-group">
              <label class="form-label">Какое у Вас мероприятие?</label>
              <div class="event-types">
                <label
                  v-for="eventType in eventTypes"
                  :key="eventType.id"
                  class="event-type-label"
                >
                  <input
                    v-model="form.eventType"
                    type="radio"
                    :value="eventType.value"
                    class="event-type-input"
                  />
                  <span class="event-type-text">{{ eventType.label }}</span>
                </label>
              </div>
            </div>
            
            <div class="form-group">
              <label class="form-label">Ваш Бюджет</label>
              <p class="form-sublabel">Slide to indicate your budget range</p>
              <div class="budget-slider-wrapper">
                <input
                  v-model.number="form.budget[0]"
                  type="range"
                  min="10000"
                  max="100000"
                  step="1000"
                  class="budget-slider"
                />
                <div class="budget-labels">
                  <span>{{ form.budget[0] }}₽</span>
                  <span>{{ form.budget[1] }}₽</span>
                </div>
              </div>
            </div>
            
            <div class="form-group">
              <label class="form-label">Ваше Сообщение</label>
              <textarea
                v-model="form.message"
                class="form-textarea"
                placeholder="Type here"
                rows="4"
              ></textarea>
            </div>
            
            <AppButton
              type="submit"
              variant="primary"
              size="large"
              class="submit-button"
            >
              Отправить
            </AppButton>
          </form>
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

.home-view {
  min-height: 100vh;
  flex-decoration: column;
  background: $color-dark;
}

.hero-section {
  position: relative;
  width: 100%;
  min-height: 691px;
  background-image: url('/src/assets/images/main-banner.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  
  @include respond-to(max-sm) {
    min-height: 500px;
  }
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(26, 26, 26, 0.6);
  display: flex;
  align-items: center;
  padding: 112px 0;
  
  @include respond-to(max-sm) {
    padding: 80px 0;
  }
}

.hero-content {
  max-width: 1280px;
  margin: 0 auto;
  text-align: center;
}

.hero-title {
  @include heading-large;
  color: $color-light;
  margin-bottom: $spacing-xl;
  display: flex;
  flex-direction: column;
  gap: $spacing-sm;
}

.hero-title__line {
  display: block;
}

.hero-categories {
  display: flex;
  gap: $spacing-md;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: $spacing-xl;
  
  @include respond-to(max-sm) {
    gap: $spacing-sm;
  }
}

.category-button {
  padding: $spacing-sm $spacing-lg;
  background: $color-dark-light;
  color: $color-light;
  border: 1px solid $color-dark-lighter;
  border-radius: $radius-md;
  @include text-base;
  font-weight: $font-weight-medium;
  cursor: pointer;
  transition: all $transition-base;
  
  &:hover {
    background: $color-dark-lighter;
    border-color: $color-primary;
  }
  
  @include respond-to(max-sm) {
    padding: $spacing-xs $spacing-md;
    font-size: $font-size-sm;
  }
}

.hero-actions {
  display: flex;
  gap: $spacing-lg;
  justify-content: center;
  flex-wrap: wrap;
}

.hero-button {
  min-width: 180px;
  
  @include respond-to(max-sm) {
    width: 100%;
    max-width: 300px;
  }
}

.section {
  padding: $spacing-4xl 0;
  
  @include respond-to(max-sm) {
    padding: $spacing-3xl 0;
  }
}

.services-section {
  background: $color-dark-light;
  position: relative;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: url('/src/assets/images/main-banner.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    opacity: 0.1;
    filter: blur(10px);
    z-index: 0;
  }
}

.section-title {
  @include heading-medium;
  text-align: center;
  margin-bottom: $spacing-lg;
  color: $color-light;
  position: relative;
  z-index: 1;
}

.section-description {
  font-size: 18px;
  text-align: center;
  color: $color-text-light;
  max-width: 800px;
  margin: 0 auto;
  line-height: $line-height-relaxed;
  position: relative;
  z-index: 1;
}

.services-columns {
  padding-top: 0;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: $spacing-xl;
  border-top: 1px solid $color-dark-light;
  border-left: 1px solid $color-dark-light;
  
  @include respond-to(max-md) {
    grid-template-columns: 1fr;
    border-left: none;
  }
}

.service-card {
  padding: $spacing-3xl;
  border-right: 1px solid $color-dark-light;
  border-bottom: 1px solid $color-dark-light;
  
  @include respond-to(max-md) {
    border-right: none;
  }
}

.service-card__title {
  @include heading-small;
  color: $color-light;
  margin-bottom: $spacing-lg;
  font-size: $font-size-xl;
}

.service-card__description {
  @include text-base;
  color: $color-text-light;
  line-height: $line-height-relaxed;
}

.why-choose-us {
  padding: 0;
  position: relative;
  min-height: 400px;
  background-image: url('/src/assets/images/main-banner.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  
  @include respond-to(max-sm) {
    min-height: 300px;
  }
}

.why-choose-us__overlay {
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

.why-choose-us__title {
  @include heading-medium;
  text-align: center;
  color: $color-light;
  margin-bottom: $spacing-lg;
}

.why-choose-us__description {
  font-size: 18px;
  text-align: center;
  color: $color-text-light;
  max-width: 800px;
  margin: 0 auto;
  line-height: $line-height-relaxed;
}

.features-section {
  padding-top: $spacing-4xl;
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: $spacing-xl;
  
  @include respond-to(max-md) {
    grid-template-columns: 1fr;
  }
}

.faq-banner {
  padding: 0;
  position: relative;
  min-height: 300px;
  background-image: url('/src/assets/images/main-banner.jpg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  
  @include respond-to(max-sm) {
    min-height: 250px;
  }
}

.faq-banner__overlay {
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

.faq-banner__title {
  @include heading-medium;
  text-align: center;
  color: $color-light;
  margin-bottom: $spacing-md;
}

.faq-banner__description {
  @include text-base;
  text-align: center;
  color: $color-text-light;
}

.faq-section {
  padding-top: $spacing-4xl;
}

.faq-container {
  max-width: 800px;
  margin: 0 auto;
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

.contact-form-section {
  padding-top: $spacing-4xl;
}

.contact-form {
  max-width: 800px;
  margin: 0 auto;
}

.form-row {
  @include grid(2, $spacing-lg);
  margin-bottom: $spacing-lg;
  
  @include respond-to(max-sm) {
    grid-template-columns: 1fr;
  }
}

.form-group {
  margin-bottom: $spacing-xl;
}

.form-label {
  display: block;
  margin-bottom: $spacing-sm;
  font-weight: $font-weight-medium;
  color: $color-light;
  @include text-base;
}

.form-sublabel {
  @include text-sm;
  color: $color-text-lighter;
  margin-bottom: $spacing-md;
}

.form-input,
.form-textarea {
  width: 100%;
  padding: $spacing-md;
  border: none;
  border-bottom: 1px solid $color-dark-light;
  @include text-base;
  font-family: inherit;
  background: transparent;
  color: $color-light;
  transition: all $transition-base;
  
  &::placeholder {
    color: $color-text-lighter;
    font-style: italic;
  }
  
  &:focus {
    outline: none;
    border-bottom-color: $color-primary;
  }
}

.form-textarea {
  resize: vertical;
  min-height: 120px;
}

.event-types {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: $spacing-md;
  margin-top: $spacing-md;
  
  @include respond-to(max-sm) {
    grid-template-columns: 1fr;
  }
}

.event-type-label {
  display: flex;
  align-items: center;
  gap: $spacing-sm;
  cursor: pointer;
  padding: $spacing-sm;
  border-radius: $radius-md;
  transition: background $transition-base;
  
  &:hover {
    background: $color-dark-light;
  }
}

.event-type-input {
  width: 20px;
  height: 20px;
  accent-color: $color-primary;
  cursor: pointer;
}

.event-type-text {
  color: $color-light;
  @include text-base;
}

.budget-slider-wrapper {
  margin-top: $spacing-md;
}

.budget-slider {
  width: 100%;
  height: 8px;
  margin: $spacing-md 0;
  background: $color-dark-light;
  border-radius: $radius-sm;
  outline: none;
  -webkit-appearance: none;
  
  &::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 20px;
    height: 20px;
    background: $color-primary;
    border-radius: $radius-full;
    cursor: pointer;
  }
  
  &::-moz-range-thumb {
    width: 20px;
    height: 20px;
    background: $color-primary;
    border-radius: $radius-full;
    cursor: pointer;
    border: none;
  }
}

.budget-labels {
  @include flex-between;
  color: $color-light;
  @include text-base;
  margin-top: $spacing-sm;
}

.submit-button {
  margin-top: $spacing-xl;
  width: 100%;
  
  @include respond-to(max-sm) {
    width: auto;
    float: right;
  }
}
</style>
