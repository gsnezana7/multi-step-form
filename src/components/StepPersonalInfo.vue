<script setup>
import { VueTelInput } from 'vue-tel-input'
import 'vue-tel-input/vue-tel-input.css'

const name = defineModel('name')
const email = defineModel('email')
const phone = defineModel('phone')

defineProps({
  errors: {
    type: Object,
    default: () => ({ name: '', email: '', phone: '' })
  }
})

const mode = 'international'

const dropdownOptions = {
  showDialCodeInList: true,
  showFlags: true
}

const inputOptions = {
  id: 'user-phone',
  placeholder: 'e.g. +1 234 567 890',
  'aria-describedby': 'phone-error'
}

const handleNameInput = (event) => {
  name.value = event.target.value.replace(/[0-9~`!@#$%^&*()_+=\[\]{}|\\:;\"'<>,.?\/]/g, '')
}
</script>

<template>
  <section class="step-body">
    <h2 class="step-body__title">Personal info</h2>
    <p class="step-body__description">Please provide your name, email address, and phone number.</p>

    <div class="form-group">
      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-name">Name</label>
          <span v-if="errors?.name" id="name-error" class="form-field__error-msg">
            {{ errors.name }}
          </span>
        </div>
        <input v-model="name" @input="handleNameInput" class="form-field__input"
          :class="{ 'form-field__input--error': errors?.name }" id="user-name" type="text"
          placeholder="e.g. Stephen King" :aria-invalid="!!errors?.name" aria-describedby="name-error" />
      </div>

      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-email">Email Address</label>
          <span v-if="errors?.email" id="email-error" class="form-field__error-msg">
            {{ errors.email }}
          </span>
        </div>
        <input v-model="email" class="form-field__input" :class="{ 'form-field__input--error': errors?.email }"
          id="user-email" type="email" placeholder="e.g. stephenking@lorem.com" :aria-invalid="!!errors?.email"
          aria-describedby="email-error" />
      </div>

      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-phone">Phone Number</label>
          <span v-if="errors?.phone" id="phone-error" class="form-field__error-msg">
            {{ errors.phone }}
          </span>
        </div>
        <vue-tel-input v-model="phone" :mode="mode" :dropdownOptions="dropdownOptions" :inputOptions="inputOptions"
          class="form-field__tel-input" :class="{ 'form-field__tel-input--error': errors?.phone }" />
      </div>
    </div>
  </section>
</template>

<style scoped>
/* ==========================================================================
   1. ТЕКСТОВЫЕ СТИЛИ (Мобильные по умолчанию в REM)
   ========================================================================== */

.step-body__title {
  font-size: 1.5rem;
  /* 24px */
  font-weight: 700;
  color: var(--marine-blue);
  margin-bottom: 0.75rem;
  /* 12px */
}

.step-body__description {
  font-size: 1rem;
  /* 16px */
  color: var(--cool-gray);
  line-height: 1.5;
}

/* ==========================================================================
   2. СТИЛИЗАЦИЯ ФОРМЫ И ПОЛЕЙ (REM/EM)
   ========================================================================== */

.form-group {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  /* 16px */
  margin-top: 1.375rem;
  /* 22px */
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  /* 4px */
}

/* Выравниваем Label и текст ошибки по краям */
.form-field__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.form-field__label {
  font-size: 0.75rem;
  /* 12px */
  color: var(--marine-blue);
  font-weight: 500;
}

/* Красный текст ошибки */
.form-field__error-msg {
  font-size: 0.75rem;
  /* 12px */
  color: var(--strawberry-red);
  font-weight: 700;
}

/* Базовый инпут (Имя, Email) */
.form-field__input {
  width: 100%;
  padding: 0.75rem 1rem;
  /* 12px 16px */
  font-family: inherit;
  font-size: 0.9375rem;
  /* 15px */
  font-weight: 500;
  color: var(--marine-blue);
  border: 1px solid var(--light-gray);
  border-radius: 0.25rem;
  /* 4px */
  outline: none;
  box-sizing: border-box;
  /* Защита от раздувания ширины */
  transition: border-color 0.2s ease;
}

.form-field__input::placeholder {
  color: var(--cool-gray);
  font-weight: 500;
  opacity: 0.8;
}

/* Состояния фокуса и ошибок */
.form-field__input:focus {
  border-color: var(--purplish-blue);
}

.form-field__input--error,
.form-field__input--error:focus {
  border-color: var(--strawberry-red);
}

/* ==========================================================================
   3. СТИЛИЗАЦИЯ ПЛАГИНА VUE-TEL-INPUT 
   ========================================================================== */

.form-field__tel-input {
  border: 1px solid var(--light-gray) !important;
  border-radius: 0.25rem !important;
  /* 4px */
  box-shadow: none !important;
  box-sizing: border-box;
}

.form-field__tel-input:focus-within {
  border-color: var(--purplish-blue) !important;
}

.form-field__tel-input--error {
  border-color: var(--strawberry-red) !important;
}


:deep(.vti__input) {
  padding: 0.75rem 1rem !important;
  /* 12px 16px */
  font-family: inherit !important;
  font-size: 0.9375rem !important;
  /* 15px */
  font-weight: 500 !important;
  color: var(--marine-blue) !important;
}

/* ==========================================================================
   4. ДЕСКТОПНЫЕ СТИЛИ (Согласованы с брейкпоинтом главного Grid-контейнера)
   ========================================================================== */

@media (min-width: 62rem) {

  /* Изменили 48em на 62rem (992px) для синхронности с общим Grid */
  .step-body__title {
    font-size: 2rem;
    /* 32px */
    margin-bottom: 0.5rem;
    /* 8px */
  }

  .form-group {
    max-width: 28.125rem;
    /* 450px */
    gap: 1.5rem;
    /* На десктопе увеличиваем расстояние между полями до 24px */
  }
}
</style>
