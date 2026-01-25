<script setup>
import { ref, computed, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import AppHeader from '@/components/layout/AppHeader.vue'
import AppFooter from '@/components/layout/AppFooter.vue'
import AppButton from '@/components/ui/AppButton.vue'
import AppContactModal from '@/components/ui/AppContactModal.vue'
import { getWorkBySlug, works } from '@models/worksData'

const route = useRoute()
const router = useRouter()
const showContactModal = ref(false)
const currentStyleIndex = ref(0)
const currentImageIndex = ref(0)

const work = computed(() => {
  const slug = route.params.slug
  return getWorkBySlug(slug)
})

const currentStyle = computed(() => {
  if (!work.value || !work.value.styles) return null
  return work.value.styles[currentStyleIndex.value]
})

const currentStyleImages = computed(() => {
  if (!currentStyle.value) return []
  return currentStyle.value.images || []
})

const handleLogin = () => {
  console.log('Login clicked')
}

const handleContact = () => {
  showContactModal.value = true
}

const goBack = () => {
  router.push('/works')
}

const nextStyle = () => {
  if (!work.value || !work.value.styles) return
  currentStyleIndex.value = (currentStyleIndex.value + 1) % work.value.styles.length
  currentImageIndex.value = 0
}

const prevStyle = () => {
  if (!work.value || !work.value.styles) return
  currentStyleIndex.value = currentStyleIndex.value === 0 
    ? work.value.styles.length - 1 
    : currentStyleIndex.value - 1
  currentImageIndex.value = 0
}

const nextImage = () => {
  if (currentStyleImages.value.length === 0) return
  currentImageIndex.value = (currentImageIndex.value + 1) % currentStyleImages.value.length
}

const prevImage = () => {
  if (currentStyleImages.value.length === 0) return
  currentImageIndex.value = currentImageIndex.value === 0 
    ? currentStyleImages.value.length - 1 
    : currentImageIndex.value - 1
}

onMounted(() => {
  if (!work.value) {
    router.push('/works')
  }
})
</script>

<template>
  <div class="work-detail-view" v-if="work">
    <AppHeader @login="handleLogin" />
    
    <main>
      <!-- Hero Section -->
      <section class="hero-section">
        <div class="hero-image-wrapper">
          <img 
            v-if="currentStyle && currentStyleImages.length > 0"
            :src="currentStyleImages[currentImageIndex]" 
            :alt="currentStyle.title" 
            class="hero-image" 
          />
          <img 
            v-else
            :src="work.image" 
            :alt="work.title" 
            class="hero-image" 
          />
        </div>
        <div class="hero-content">
          <div class="container">
            <div class="hero-text">
              <h1 class="hero-title">{{ work.title }}</h1>
              <p class="hero-subtitle" v-if="work.description">{{ work.description }}</p>
              <AppButton variant="outline" size="medium" class="hero-button">
                Посмотреть
              </AppButton>
            </div>
          </div>
        </div>
        <!-- Navigation Arrows for Main Carousel -->
        <button 
          v-if="work.styles && work.styles.length > 1"
          class="carousel-nav carousel-nav--left"
          @click="prevStyle"
          aria-label="Предыдущий стиль"
        >
          ←
        </button>
        <button 
          v-if="work.styles && work.styles.length > 1"
          class="carousel-nav carousel-nav--right"
          @click="nextStyle"
          aria-label="Следующий стиль"
        >
          →
        </button>
      </section>

      <!-- Our Approach Section -->
      <section class="approach-section section">
        <div class="container">
          <h2 class="section-title">Наш подход</h2>
          <p class="section-description">Мы выбираем подходящий вид декора специально для вас!</p>
          <div class="approach-subtitle">
            Ниже предоставлены стили, которые мы можем реализовать
          </div>

          <!-- Styles Carousel -->
          <div v-if="work.styles && work.styles.length > 0" class="styles-carousel">
            <button 
              class="carousel-nav-small carousel-nav-small--left"
              @click="prevStyle"
              aria-label="Предыдущий"
            >
              ←
            </button>
            
            <div class="styles-container">
              <div
                v-for="(style, index) in work.styles"
                :key="style.id"
                class="style-card"
                :class="{ 'style-card--active': index === currentStyleIndex }"
              >
                <div class="style-card__image">
                  <img :src="style.image" :alt="style.title" />
                </div>
                <div class="style-card__content">
                  <h3 class="style-card__title">{{ style.title }}</h3>
                  <p v-if="style.subtitle" class="style-card__subtitle">{{ style.subtitle }}</p>
                </div>
                <!-- Inner Navigation Arrows -->
                <div class="style-card__nav">
                  <button 
                    class="style-nav-arrow"
                    @click.stop="prevImage"
                    aria-label="Предыдущее фото"
                  >
                    ←
                  </button>
                  <button 
                    class="style-nav-arrow"
                    @click.stop="nextImage"
                    aria-label="Следующее фото"
                  >
                    →
                  </button>
                </div>
              </div>
            </div>
            
            <button 
              class="carousel-nav-small carousel-nav-small--right"
              @click="nextStyle"
              aria-label="Следующий"
            >
              →
            </button>
          </div>
        </div>
      </section>

      <!-- Gallery Section -->
      <section class="gallery-section section">
        <div class="container">
          <h2 class="section-title">Наши фото</h2>
          <div class="gallery-grid">
            <div
              v-for="(image, index) in work.images"
              :key="index"
              class="gallery-item"
            >
              <img :src="image" :alt="`${work.title} - фото ${index + 1}`" />
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

.work-detail-view {
  min-height: 100vh;
  flex-decoration: column;
  background: $color-dark;
}

.hero-section {
  position: relative;
  width: 100%;
  min-height: 600px;
  overflow: hidden;
  border-top: 1px solid $color-dark-light;
  
  @include respond-to(max-sm) {
    min-height: 400px;
  }
}

.hero-image-wrapper {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.hero-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.hero-content {
  position: relative;
  z-index: 2;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: flex-end;
  padding: $spacing-4xl 0;
}

.hero-text {
  max-width: 800px;
}

.hero-title {
  @include heading-large;
  color: $color-light;
  margin-bottom: $spacing-md;
}

.hero-subtitle {
  font-size: 18px;
  color: $color-light;
  margin-bottom: $spacing-lg;
  line-height: $line-height-relaxed;
}

.hero-button {
  margin-top: $spacing-md;
}

.carousel-nav {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 60px;
  height: 60px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: $radius-full;
  color: $color-light;
  font-size: $font-size-2xl;
  cursor: pointer;
  z-index: 3;
  @include flex-center;
  backdrop-filter: blur(10px);
  transition: all $transition-base;
  
  &:hover {
    background: rgba(255, 255, 255, 0.2);
    border-color: $color-primary;
  }
  
  &--left {
    left: $spacing-xl;
  }
  
  &--right {
    right: $spacing-xl;
  }
  
  @include respond-to(max-sm) {
    width: 40px;
    height: 40px;
    font-size: $font-size-xl;
    
    &--left {
      left: $spacing-md;
    }
    
    &--right {
      right: $spacing-md;
    }
  }
}

.approach-section {
  padding: $spacing-4xl 0;
  border-top: 1px solid $color-dark-light;
  border-bottom: 1px solid $color-dark-light;
}

.section-title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-lg;
  text-align: left;
}

.section-description {
  font-size: 18px;
  color: $color-text-light;
  margin-bottom: $spacing-md;
  line-height: $line-height-relaxed;
}

.approach-subtitle {
  @include text-base;
  color: $color-text-light;
  padding: $spacing-lg;
  background: $color-dark-light;
  border-radius: $radius-md;
  border: 1px solid $color-dark-lighter;
  margin-bottom: $spacing-4xl;
}

.styles-carousel {
  position: relative;
  display: flex;
  align-items: center;
  gap: $spacing-xl;
}

.carousel-nav-small {
  width: 50px;
  height: 50px;
  background: $color-dark-light;
  border: 1px solid $color-dark-lighter;
  border-radius: $radius-full;
  color: $color-light;
  font-size: $font-size-xl;
  cursor: pointer;
  @include flex-center;
  transition: all $transition-base;
  flex-shrink: 0;
  
  &:hover {
    border-color: $color-primary;
    background: $color-dark-lighter;
  }
  
  @include respond-to(max-sm) {
    width: 40px;
    height: 40px;
    font-size: $font-size-lg;
  }
}

.styles-container {
  display: flex;
  gap: $spacing-xl;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  flex: 1;
  scrollbar-width: none;
  
  &::-webkit-scrollbar {
    display: none;
  }
  
  @include respond-to(max-sm) {
    gap: $spacing-md;
  }
}

.style-card {
  position: relative;
  min-width: 400px;
  border-radius: $radius-lg;
  overflow: hidden;
  background: $color-dark-light;
  border: 2px solid transparent;
  transition: all $transition-base;
  scroll-snap-align: start;
  
  @include respond-to(max-sm) {
    min-width: 300px;
  }
  
  &--active {
    border-color: $color-primary;
  }
}

.style-card__image {
  width: 100%;
  height: 300px;
  overflow: hidden;
  
  @include respond-to(max-sm) {
    height: 200px;
  }
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
}

.style-card__content {
  padding: $spacing-xl;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0, 0, 0, 0.9), transparent);
}

.style-card__title {
  @include heading-small;
  color: $color-light;
  margin: 0 0 $spacing-xs 0;
  font-size: $font-size-xl;
}

.style-card__subtitle {
  @include text-base;
  color: $color-text-light;
  margin: 0;
}

.style-card__nav {
  position: absolute;
  bottom: $spacing-md;
  right: $spacing-md;
  display: flex;
  gap: $spacing-sm;
}

.style-nav-arrow {
  width: 30px;
  height: 30px;
  background: rgba(0, 0, 0, 0.5);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: $radius-full;
  color: $color-light;
  font-size: $font-size-base;
  cursor: pointer;
  @include flex-center;
  transition: all $transition-base;
  
  &:hover {
    background: rgba(0, 0, 0, 0.7);
    border-color: $color-primary;
  }
}

.gallery-section {
  padding: $spacing-4xl 0;
  border-top: 1px solid $color-dark-light;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: $spacing-xl;
  
  @include respond-to(max-md) {
    grid-template-columns: repeat(2, 1fr);
    gap: $spacing-lg;
  }
  
  @include respond-to(max-sm) {
    grid-template-columns: 1fr;
    gap: $spacing-md;
  }
}

.gallery-item {
  border-radius: $radius-lg;
  overflow: hidden;
  aspect-ratio: 1;
  background: $color-dark-light;
  
  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform $transition-slow;
  }
  
  &:hover img {
    transform: scale(1.05);
  }
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
