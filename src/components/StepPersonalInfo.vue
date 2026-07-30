<script setup>
import { VueTelInput } from 'vue-tel-input'
import 'vue-tel-input/vue-tel-input.css' // Импортируем стили плагина

const name = defineModel('name')
const email = defineModel('email')
const phone = defineModel('phone')

defineProps({
  errors: {
    type: Object,
    default: () => ({ name: '', email: '', phone: '' })
  }
})

// Объект с настройками для красивого внешнего вида
const telOptions = {
  placeholder: 'e.g. +1 234 567 890',
  mode: 'international', // Формат номера всегда с +кодом страны
  dropdownOptions: {
    showDialCodeInList: true, // Показывать код страны (+375, +7) рядом с флагом в списке
    showFlags: true
  }
}

// Функция для очистки имени от цифр и спецсимволов (безопасно для компилятора)
const handleNameInput = (event) => {
  name.value = event.target.value.replace(/[0-9~`!@#$%^&*()_+=\[\]{}|\\:;\"'<>,.?\/]/g, '')
}


</script>




<template>
  <section class="step-body">
    <h2 class="step-body__title">Personal info</h2>
    <p class="step-body__description">Please provide your name, email address, and phone number.</p>

    <div class="form-group">

      <!-- 1. Поле: Имя (ОСТАВЛЯЕМ) -->
      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-name">Name</label>
          <span v-if="errors?.name" class="form-field__error-msg">{{ errors.name }}</span>
        </div>
        <input v-model="name" @input="handleNameInput" class="form-field__input"
          :class="{ 'form-field__input--error': errors?.name }" id="user-name" type="text"
          placeholder="e.g. Stephen King" />

      </div>

      <!-- 2. Поле: Email (ОСТАВЛЯЕМ) -->
      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-email">Email Address</label>
          <span v-if="errors?.email" class="form-field__error-msg">{{ errors.email }}</span>
        </div>
        <input v-model="email" class="form-field__input" :class="{ 'form-field__input--error': errors?.email }"
          id="user-email" type="email" placeholder="e.g. stephenking@lorem.com" />
      </div>

      <!-- 3. Поле: Телефон (ВНУТРИ ДОЛЖЕН ОСТАТЬСЯ ТОЛЬКО VUE-TEL-INPUT) -->
      <div class="form-field">
        <div class="form-field__header">
          <label class="form-field__label" for="user-phone">Phone Number</label>
          <span v-if="errors?.phone" class="form-field__error-msg">{{ errors.phone }}</span>
        </div>

        <!-- Оставляем ТОЛЬКО этот умный компонент -->
        <vue-tel-input v-model="phone" :options="telOptions" class="form-field__tel-input"
          :class="{ 'form-field__tel-input--error': errors?.phone }" id="user-phone" />

      </div>

    </div>
  </section>
</template>



<style scoped>
.step-body__title {
  font-size: 24px;
  font-weight: 700;
  color: var(--marine-blue);
  margin-bottom: 12px;
}

.step-body__description {
  font-size: 16px;
  color: var(--cool-gray);
  line-height: 1.5;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 16px;
  margin-top: 22px;
}

.form-field {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.form-field__label {
  font-size: 12px;
  color: var(--marine-blue);
  font-weight: 500;
}

.form-field__input {
  width: 100%;
  padding: 12px 16px;
  font-family: inherit;
  font-size: 15px;
  font-weight: 500;
  color: var(--marine-blue);
  border: 1px solid var(--light-gray);
  border-radius: 4px;
  outline: none;
  transition: border-color 0.2s ease;
}

.form-field__input::placeholder {
  color: var(--cool-gray);
  font-weight: 500;
  opacity: 0.8;
}

.form-field__input:focus {
  border-color: var(--purplish-blue);
}

/* Выравниваем Label и текст ошибки по краям */
.form-field__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

/* Красный текст ошибки из ТЗ */
.form-field__error-msg {
  font-size: 12px;
  color: var(--strawberry-red);
  font-weight: 700;
}

/* Красная рамка для инпута в случае ошибки */
.form-field__input--error {
  border-color: var(--strawberry-red);
}

/* Изменяем цвет фокуса, если поле с ошибкой, чтобы оно оставалось красным */
.form-field__input--error:focus {
  border-color: var(--strawberry-red);
}

м

/* Стилизуем обертку плагина под наш дизайн */
.form-field__tel-input {
  border: 1px solid var(--light-gray) !important;
  border-radius: 4px !important;
  box-shadow: none !important;
}

/* Эффект фокуса */
.form-field__tel-input:focus-within {
  border-color: var(--purplish-blue) !important;
}

/* Ошибка */
.form-field__tel-input--error {
  border-color: var(--strawberry-red) !important;
}

/* Внутренний инпут плагина делаем похожим на наши остальные поля */
:deep(.vti__input) {
  padding: 12px 16px !important;
  font-family: inherit !important;
  font-size: 15px !important;
  font-weight: 500 !important;
  color: var(--marine-blue) !important;
}

@media (min-width: 992px) {
  .step-body__title {
    font-size: 32px;
    margin-bottom: 8px;
  }
}
</style>
