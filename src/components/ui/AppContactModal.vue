<script setup>
import { ref, watch } from 'vue'
import AppButton from '@/components/ui/AppButton.vue'
import { eventTypes } from '@models/homeData'

const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close'])

const form = ref({
  name: '',
  email: '',
  eventType: 'wedding',
  budget: [10000, 50000],
  message: ''
})

const closeModal = () => {
  emit('close')
}

const handleSubmit = () => {
  console.log('Form submitted:', form.value)
  alert('Заявка отправлена! Мы свяжемся с вами в течение 24 часов.')
  resetForm()
  closeModal()
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

// Закрытие по клику на backdrop
const handleBackdropClick = (e) => {
  if (e.target === e.currentTarget) {
    closeModal()
  }
}

// Закрытие по Escape
watch(() => props.isOpen, (isOpen) => {
  if (isOpen) {
    document.body.style.overflow = 'hidden'
    const handleEscape = (e) => {
      if (e.key === 'Escape') {
        closeModal()
        document.removeEventListener('keydown', handleEscape)
      }
    }
    document.addEventListener('keydown', handleEscape)
  } else {
    document.body.style.overflow = ''
  }
})
</script>

<template>
  <Teleport to="body">
    <Transition name="modal">
      <div
        v-if="isOpen"
        class="modal-overlay"
        @click="handleBackdropClick"
      >
        <div class="modal-container" @click.stop>
          <button class="modal-close" @click="closeModal" aria-label="Закрыть">
            ×
          </button>
          
          <!-- Banner Section -->
          <div class="modal-banner">
            <div class="modal-banner__overlay">
              <p class="modal-banner__logo">для тебя</p>
              <h2 class="modal-banner__title">Спасибо за Ваш интерес к нашему Декор-агенству</h2>
              <p class="modal-banner__description">Мы будем рады услышать ваши идеи и обсудить, как создать неповторимую атмосферу вашего события. Выберите удобный способ связаться с нами:</p>
            </div>
          </div>

          <!-- Form Section -->
          <div class="modal-form-wrapper">
            <form @submit.prevent="handleSubmit" class="modal-form">
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
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style lang="scss" scoped>
@use '@/assets/styles/variables' as *;
@use '@/assets/styles/mixins' as *;

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: $z-index-modal-backdrop;
  padding: $spacing-xl;
  overflow-y: auto;
  
  @include respond-to(max-sm) {
    padding: $spacing-md;
    align-items: flex-start;
    padding-top: $spacing-3xl;
  }
}

.modal-container {
  position: relative;
  width: 100%;
  max-width: 900px;
  background: $color-dark;
  border-radius: $radius-xl;
  overflow: hidden;
  max-height: 90vh;
  overflow-y: auto;
  z-index: $z-index-modal;
  
  @include respond-to(max-sm) {
    max-height: 95vh;
    border-radius: $radius-lg;
  }
}

.modal-close {
  position: absolute;
  top: $spacing-lg;
  right: $spacing-lg;
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: $radius-full;
  color: $color-light;
  font-size: $font-size-2xl;
  cursor: pointer;
  z-index: 10;
  @include flex-center;
  transition: all $transition-base;
  
  &:hover {
    background: rgba(255, 255, 255, 0.2);
    border-color: $color-primary;
  }
}

.modal-banner {
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

.modal-banner__overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(26, 26, 26, 0.7);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: $spacing-4xl $spacing-3xl;
  text-align: center;
  
  @include respond-to(max-sm) {
    padding: $spacing-3xl $spacing-xl;
  }
}

.modal-banner__logo {
  font-family: $font-family-heading;
  font-style: italic;
  font-size: $font-size-xl;
  color: $color-light;
  margin-bottom: $spacing-lg;
  opacity: 0.9;
}

.modal-banner__title {
  @include heading-medium;
  color: $color-light;
  margin-bottom: $spacing-lg;
  
  @include respond-to(max-sm) {
    font-size: $font-size-2xl;
  }
}

.modal-banner__description {
  font-size: 18px;
  color: $color-text-light;
  line-height: $line-height-relaxed;
  max-width: 700px;
}

.modal-form-wrapper {
  padding: $spacing-4xl;
  
  @include respond-to(max-sm) {
    padding: $spacing-xl;
  }
}

.modal-form {
  max-width: 800px;
  margin: 0 auto;
}

.form-row {
  @include grid(2, $spacing-lg);
  margin-bottom: $spacing-xl;
  
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

// Modal transitions
.modal-enter-active,
.modal-leave-active {
  transition: opacity $transition-base;
}

.modal-enter-active .modal-container,
.modal-leave-active .modal-container {
  transition: transform $transition-base, opacity $transition-base;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  
  .modal-container {
    transform: scale(0.9) translateY(-20px);
  }
}

.modal-enter-to,
.modal-leave-from {
  opacity: 1;
  
  .modal-container {
    transform: scale(1) translateY(0);
  }
}
</style>
