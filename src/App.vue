<script setup>
import { ref, watch, onMounted } from 'vue'

import StepPersonalInfo from './components/StepPersonalInfo.vue'
import StepSelectPlan from './components/StepSelectPlan.vue'
import StepAddOns from './components/StepAddOns.vue'
import StepSummary from './components/StepSummary.vue'
import StepSuccess from './components/StepSuccess.vue'

// 1. БАЗОВЫЕ ПЕРЕМЕННЫЕ СОСТОЯНИЯ (Объявляем в первую очередь!)
const currentStep = ref(1)

const formData = ref({
  name: '',
  email: '',
  phone: '',
  plan: 'Arcade',
  billing: 'monthly',
  addons: []
})

const errors = ref({
  name: '',
  email: '',
  phone: ''
})

// 2. ФУНКЦИИ НАВИГАЦИИ И ВАЛИДАЦИИ
const prevStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--
  }
}

const nextStep = () => {
  if (currentStep.value === 1) {
    errors.value = { name: '', email: '', phone: '' }
    let isValid = true

    // Проверка имени
    if (!formData.value.name.trim()) {
      errors.value.name = 'This field is required'
      isValid = false
    }

    // Проверка Email
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!formData.value.email.trim()) {
      errors.value.email = 'This field is required'
      isValid = false
    } else if (!emailPattern.test(formData.value.email)) {
      errors.value.email = 'Invalid email format'
      isValid = false
    }

    const phoneString = typeof formData.value.phone === 'string'
      ? formData.value.phone
      : '';

    if (!phoneString.trim()) {
      errors.value.phone = 'This field is required'
      isValid = false
    } else if (phoneString.replace(/\D/g, '').length < 9) {
      errors.value.phone = 'Please enter a valid phone number'
      isValid = false
    }


    if (!isValid) return
  }

  if (currentStep.value < 5) currentStep.value++

  if (currentStep.value === 5) {
    localStorage.removeItem('wizard_form_data')
    localStorage.setItem('wizard_current_step', '5')
  }
}

// 3. НАБЛЮДАТЕЛИ И ХУКИ (LOCALSTORAGE)
watch(formData, (newVal) => {
  localStorage.setItem('wizard_form_data', JSON.stringify(newVal))
}, { deep: true })

watch(currentStep, (newStep) => {
  localStorage.setItem('wizard_current_step', newStep.toString())
})
// Наблюдаем за изменением полей формы, чтобы очищать ошибки в реальном времени
watch(
  () => formData.value.name,
  (newName) => {
    if (newName.trim()) errors.value.name = ''
  }
)

watch(
  () => formData.value.email,
  (newEmail) => {
    const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (newEmail.trim() && emailPattern.test(newEmail)) {
      errors.value.email = '' // Стираем ошибку, только если email стал полностью валидным
    }
  }
)

watch(
  () => formData.value.phone,
  (newPhone) => {
    // Приводим к строке на случай, если библиотека вернула объект
    const phoneStr = typeof newPhone === 'string' ? newPhone : ''
    if (phoneStr.trim() && phoneStr.replace(/\D/g, '').length >= 9) {
      errors.value.phone = '' // Очищаем ошибку, когда набрано нужное число цифр номера
    }
  }
)


onMounted(() => {
  const savedData = localStorage.getItem('wizard_form_data')
  const savedStep = localStorage.getItem('wizard_current_step')

  if (savedData) {
    formData.value = JSON.parse(savedData)
  }

  if (savedStep) {
    const step = parseInt(savedStep, 10)
    if (step === 5) {
      currentStep.value = 1
      localStorage.removeItem('wizard_current_step')
    } else {
      currentStep.value = step
    }
  }
})
</script>



<template>
  <div class="page-layout">
    <main class="wizard">
      <!-- Перенесли h1 внутрь main — теперь семантика идеальна -->
      <h1 class="visually-hidden">Multi-step Subscription Form</h1>

      <!-- Навигационная панель шагов -->
      <nav class="wizard__navigation" aria-label="Progress">
        <ol class="wizard__steps-list">

          <!-- Шаг 1 -->
          <!-- Динамически добавляем aria-current для экранных дикторов -->
          <li class="wizard__step-item" :aria-current="currentStep === 1 ? 'step' : undefined">
            <span class="wizard__step-number" :class="{ 'is-active': currentStep === 1 }" aria-hidden="true">1</span>
            <div class="wizard__step-text-container">
              <span class="wizard__step-meta">Step 1</span>
              <span class="wizard__step-name">Your info</span>
            </div>
          </li>

          <!-- Шаг 2 -->
          <li class="wizard__step-item" :aria-current="currentStep === 2 ? 'step' : undefined">
            <span class="wizard__step-number" :class="{ 'is-active': currentStep === 2 }" aria-hidden="true">2</span>
            <div class="wizard__step-text-container">
              <span class="wizard__step-meta">Step 2</span>
              <span class="wizard__step-name">Select plan</span>
            </div>
          </li>

          <!-- Шаг 3 -->
          <li class="wizard__step-item" :aria-current="currentStep === 3 ? 'step' : undefined">
            <span class="wizard__step-number" :class="{ 'is-active': currentStep === 3 }" aria-hidden="true">3</span>
            <div class="wizard__step-text-container">
              <span class="wizard__step-meta">Step 3</span>
              <span class="wizard__step-name">Add-ons</span>
            </div>
          </li>

          <!-- Шаг 4 -->
          <li class="wizard__step-item" :aria-current="currentStep >= 4 ? 'step' : undefined">
            <span class="wizard__step-number" :class="{ 'is-active': currentStep >= 4 }" aria-hidden="true">4</span>
            <div class="wizard__step-text-container">
              <span class="wizard__step-meta">Step 4</span>
              <span class="wizard__step-name">Summary</span>
            </div>
          </li>

        </ol>
      </nav>

      <!-- Основная контентная область с динамическими компонентами -->
      <div class="wizard__content">
        <Transition name="fade" mode="out-in">
          <StepPersonalInfo v-if="currentStep === 1" v-model:name="formData.name" v-model:email="formData.email"
            v-model:phone="formData.phone" :errors="errors" />
          <StepSelectPlan v-else-if="currentStep === 2" v-model:plan="formData.plan"
            v-model:billing="formData.billing" />
          <StepAddOns v-else-if="currentStep === 3" v-model:addons="formData.addons" :billing="formData.billing" />
          <StepSummary v-else-if="currentStep === 4" :form-data="formData" @change-plan="currentStep = 2" />
          <StepSuccess v-else-if="currentStep === 5" />
        </Transition>
      </div>

      <!-- Панель управления кнопками: заменили footer на семантичный div -->
      <div class="wizard__buttons-panel" v-if="currentStep < 5">
        <button v-if="currentStep > 1" @click="prevStep" class="wizard__button wizard__button--back" type="button">
          Go Back
        </button>
        <button @click="nextStep" class="wizard__button wizard__button--next"
          :class="{ 'wizard__button--confirm': currentStep === 4 }" type="button">
          {{ currentStep === 4 ? 'Confirm' : 'Next Step' }}
        </button>
      </div>
    </main>

    <!-- Глобальный футер: убрали лишний атрибут role -->
    <footer class="attribution">
      Challenge by <a href="https://www.frontendmentor.io/profile/gsnezana7" target="_blank"
        rel="noopener noreferrer">Frontend Mentor</a>.
      Coded by <a href="https://github.com/gsnezana7?tab=repositories" target="_blank"
        rel="noopener noreferrer">Snezana</a>.
    </footer>

  </div>
</template>


<style scoped>
/* ==========================================================================
   1. БАЗОВЫЕ И МОБИЛЬНЫЕ СТИЛИ (Применяются на смартфонах по умолчанию)
   ========================================================================== */

/* Глобальный контейнер страницы */
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}


.page-layout {
  min-height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  position: relative;
  background-color: var(--magnolia);
}

/* Главный блок формы */
.wizard {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

/* Верхняя панель навигации (синий фон на мобильных) */
.wizard__navigation {
  background-image: url('./assets/images/bg-sidebar-mobile.svg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  height: 172px;
  padding-top: 32px;
}

/* Список кружков */
.wizard__steps-list {
  display: flex;
  justify-content: center;
  gap: 16px;
  list-style: none;
}

/* Элемент списка навигации */
.wizard__step-item {
  display: flex;
  align-items: center;
  gap: 16px;
}

/* На мобильных экранах текстовый контейнер шага полностью скрываем */
.wizard__step-text-container {
  display: none;
}

/* Кружок с цифрой */
.wizard__step-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 33px;
  height: 33px;
  border: 1px solid var(--white);
  border-radius: 50%;
  color: var(--white);
  font-weight: 700;
  font-size: 14px;
}

/* Активный кружок */
.wizard__step-number.is-active {
  background-color: var(--light-blue);
  color: var(--marine-blue);
  border-color: var(--light-blue);
}

/* Контентная карточка (белый блок) */
.wizard__content {
  background-color: var(--white);
  margin: -72px 16px 0 16px;
  /* Наплыв на синий фон */
  padding: 32px 24px;
  border-radius: 10px;
  box-shadow: 0px 25px 40px -20px rgba(0, 0, 0, 0.1);
}





/* Нижняя панель с кнопками на мобильном */
.wizard__buttons-panel {
  margin-top: auto;
  /* Прижимает панель к низу экрана */
  background-color: var(--white);
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.wizard__button {
  border: none;
  font-family: inherit;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  padding: 12px 18px;
  border-radius: 4px;
}

.wizard__button--back {
  background-color: transparent;
  color: var(--cool-gray);
  padding-left: 0;
}

.wizard__button--back:hover {
  color: var(--marine-blue);
}

.wizard__button--next {
  background-color: var(--marine-blue);
  color: var(--white);
  margin-left: auto;
}

/* Фиолетовая кнопка подтверждения на 4-м шаге */
.wizard__button--confirm {
  background-color: var(--purplish-blue);
}

.wizard__button--confirm:hover {
  background-color: hsl(243, 100%, 72%);
  /* Становится чуть светлее при наведении */
}

/* Подпись Frontend Mentor в самом низу */
.attribution {
  font-size: 11px;
  text-align: center;
  padding: 16px;
  background-color: var(--magnolia);
}

.attribution a {
  color: var(--purplish-blue);
}


/* ==========================================================================
   2. ДЕСКТОПНЫЕ СТИЛИ (Включаются только на экранах от 992px)
   ========================================================================== */
@media (min-width: 992px) {
  .page-layout {
    justify-content: center;
    align-items: center;
    padding: 20px;
  }

  /* Форма превращается в фиксированную двухколоночную карточку */
  .wizard {
    flex-direction: row;
    background-color: var(--white);
    padding: 16px;
    border-radius: 15px;
    width: 940px;
    height: 600px;
    box-shadow: 0px 25px 40px -20px rgba(0, 0, 0, 0.05);
    flex-grow: 0;
    position: relative;
    /* Чтобы абсолютно позиционировать кнопки внутри */
  }

  /* Сайдбар становится вертикальным слева */
  .wizard__navigation {
    background-image: url('./assets/images/bg-sidebar-desktop.svg');
    width: 274px;
    height: 100%;
    border-radius: 10px;
    padding: 40px 32px;
  }

  .wizard__steps-list {
    flex-direction: column;
    gap: 24px;
    justify-content: flex-start;
  }

  /* На десктопе возвращаем текстовые подписи шагов в поток */
  .wizard__step-text-container {
    display: flex;
    flex-direction: column;
    text-transform: uppercase;
    gap: 4px;
  }

  .wizard__step-meta {
    font-size: 12px;
    color: var(--pastel-blue);
    font-weight: 400;
    letter-spacing: 1px;
  }

  .wizard__step-name {
    font-size: 14px;
    color: var(--white);
    font-weight: 700;
    letter-spacing: 1px;
  }

  /* Контентная область встает справа от сайдбара */
  .wizard__content {
    flex-grow: 1;
    margin: 0;
    padding: 40px 84px 16px 100px;
    display: flex;
    flex-direction: column;
    box-shadow: none;
  }


  /* Панель кнопок позиционируется строго под контентом */
  .wizard__buttons-panel {
    position: absolute;
    bottom: 32px;
    right: 84px;
    left: 422px;
    /* Выравнивание точно по левой границе текстового контента */
    padding: 0;
    background-color: transparent;
    display: flex;
    justify-content: space-between;
  }

  .wizard__button {
    padding: 14px 24px;
    font-size: 16px;
    border-radius: 8px;
  }

  .wizard__button--next {
    transition: background-color 0.2s ease;
  }

  .wizard__button--next:hover {
    background-color: hsl(213, 96%, 28%);
  }

  .wizard__button--confirm:hover {
    background-color: hsl(243, 100%, 72%);
  }
}

/* Классы для анимации плавного исчезновения и появления (Fade) */

/* Состояние в процессе анимации (активная фаза) */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

/* Начальное состояние при появлении и финальное при исчезновении */
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
  /* Легкий эффект выплывания снизу вверх */
}
</style>
