<script setup>
import { ref, watch, onMounted } from 'vue'

import StepPersonalInfo from './components/StepPersonalInfo.vue'
import StepSelectPlan from './components/StepSelectPlan.vue'
import StepAddOns from './components/StepAddOns.vue'
import StepSummary from './components/StepSummary.vue'
import StepSuccess from './components/StepSuccess.vue'

// 1. БАЗОВЫЕ ПЕРЕМЕННЫЕ СОСТОЯНИЯ 
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
      <div class="wizard__main-container">

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

        <!-- Панель управления кнопками -->
        <div class="wizard__buttons-panel" v-if="currentStep < 5">
          <button v-if="currentStep > 1" @click="prevStep" class="wizard__button wizard__button--back" type="button">
            Go Back
          </button>
          <button @click="nextStep" class="wizard__button wizard__button--next"
            :class="{ 'wizard__button--confirm': currentStep === 4 }" type="button">
            {{ currentStep === 4 ? 'Confirm' : 'Next Step' }}
          </button>
        </div>
      </div>
    </main>

    <!-- Глобальный футер-->
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
   1. БАЗОВЫЕ И МОБИЛЬНЫЕ СТИЛИ (Смартфоны по умолчанию)
   ========================================================================== */

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

/* Глобальный контейнер страницы */
.page-layout {
  min-height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  /* Центрирует форму по вертикали, если контента мало */
  align-items: center;
  /* Центрирует форму по горизонтали */
  background-color: var(--magnolia);
}

/* Главный блок формы (CSS Grid) */
.wizard {
  display: grid;
  grid-template-columns: 1fr;
  /* Делаем три строки: 1) Шапка, 2) Нахлест под карточку, 3) Остальной контент */
  grid-template-rows: 6.25rem 4.5rem auto;
  width: 100%;
}

/* Панель навигации шагов */
.wizard__navigation {
  grid-column: 1;
  /* Занимает первую и вторую строки (высота 6.25 + 4.5 = 10.75rem) */
  grid-row: 1 / 3;
  background-image: url('./assets/images/bg-sidebar-mobile.svg');
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  padding-top: 2rem;
  /* 32px */
}

/* Список кружков */
.wizard__steps-list {
  display: flex;
  justify-content: center;
  gap: 1rem;
  /* 16px */
  list-style: none;
}

.wizard__step-item {
  display: flex;
  align-items: center;
  gap: 1rem;
}

/* Скрываем текст шагов на мобилках */
.wizard__step-text-container {
  display: none;
}

/* Кружок с цифрой */
.wizard__step-number {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2.0625rem;
  /* 33px */
  height: 2.0625rem;
  border: 1px solid var(--white);
  border-radius: 50%;
  color: var(--white);
  font-weight: 700;
  font-size: 0.875rem;
  /* 14px */
}

.wizard__step-number.is-active {
  background-color: var(--light-blue);
  color: var(--marine-blue);
  border-color: var(--light-blue);
}

/* Правая / основная область, где лежат карточка и кнопки */
.wizard__main-container {
  grid-column: 1;
  /* Стартует со второй строки, создавая естественный нахлест на шапку */
  grid-row: 2 / 4;
  display: flex;
  flex-direction: column;
}

/* Белая карточка контента */
.wizard__content {
  background-color: var(--white);
  padding: 2rem 1.5rem;
  /* 32px 24px */
  border-radius: 0.625rem;
  /* 10px */
  box-shadow: 0px 25px 40px -20px rgba(0, 0, 0, 0.1);
  margin: 0 1rem;

}

/* Панель управления кнопками (завязана на v-if="currentStep < 5") */
.wizard__buttons-panel {
  background-color: var(--white);
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: auto;
  /* <-- Вытолкнет кнопки вниз */
}

/* Базовые стили кнопок */
.wizard__button {
  border: none;
  font-family: inherit;
  font-size: 0.875rem;
  font-weight: 500;
  cursor: pointer;
  padding: 0.85em 1.28em;
  border-radius: 0.285em;
}

/* Кнопка "Назад" (v-if="currentStep > 1") */
.wizard__button--back {
  background-color: transparent;
  color: var(--cool-gray);
  padding-left: 0;
}

.wizard__button--back:hover {
  color: var(--marine-blue);
}

/* Кнопка "Вперед" */
.wizard__button--next {
  background-color: var(--marine-blue);
  color: var(--white);
  margin-left: auto;
  /* Сама улетает вправо, если кнопки Назад нет (на 1 шаге) */
}

.wizard__button--confirm {
  background-color: var(--purplish-blue);
}

.wizard__button--confirm:hover {
  background-color: hsl(243, 100%, 72%);
}

/* Статичный футер внизу */
.attribution {
  font-size: 0.6875rem;
  text-align: center;
  padding: 1rem;
  background-color: var(--magnolia);
}

.attribution a {
  color: var(--purplish-blue);
}


/* ==========================================================================
   2. ДЕСКТОПНЫЕ СТИЛИ (Экраны от 992px)
   ========================================================================== */

@media (min-width: 62rem) {

  /* 62rem = 992px */
  .page-layout {
    justify-content: center;
    align-items: center;
  }


  /* Переключаем .wizard в горизонтальный двухколоночный Grid */
  .wizard {
    grid-template-columns: 17.1875rem 1fr;
    grid-template-rows: 1fr;

    max-width: 58.75rem;
    /* 1. Вместо width: делаем максимальную ширину */
    width: 100%;
    /* Чтобы на экранах чуть меньше 940px форма сужалась */

    min-height: 37.5rem;
    /* 2. Вместо height: делаем МИНИМАЛЬНУЮ высоту */
    height: auto;
    /* Позволяет форме расти бесконечно, если контента много */

    background-color: var(--white);
    padding: 1rem;
    border-radius: 0.9375rem;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.05);

    display: grid;
    /* Убедимся, что grid на месте */
  }


  /* Навигация становится левым сайдбаром */
  .wizard__navigation {
    grid-column: 1;
    grid-row: 1;
    background-image: url('./assets/images/bg-sidebar-desktop.svg');
    border-radius: 0.625rem;
    padding: 2.5rem 2rem;
  }

  .wizard__steps-list {
    flex-direction: column;
    justify-content: flex-start;
    gap: 1.5rem;
  }

  /* Включаем отображение текстовых названий шагов на десктопе */
  .wizard__step-text-container {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
    text-transform: uppercase;
  }

  .wizard__step-meta {
    font-size: 0.75rem;
    color: var(--cool-gray);
  }

  .wizard__step-name {
    font-size: 0.875rem;
    color: var(--white);
    font-weight: 700;
    letter-spacing: 0.0625rem;
  }

  /* Основной контейнер встает в правую часть Grid */
  .wizard__main-container {
    grid-column: 2;
    grid-row: 1;
    padding: 2.5rem 6.25rem 1rem 6.25rem;

    /* 3. ДЕЛАЕМ ПРАВУЮ КОЛОНКУ ФЛЕКС-КОНТЕЙНЕРОМ */
    display: flex;
    flex-direction: column;
  }

  /* Сбрасываем мобильный подъем карточки */
  .wizard__content {
    transform: none;
    margin: 0;
    padding: 0;
    box-shadow: none;
  }

  /* Панель кнопок теряет белую подложку */
  .wizard__buttons-panel {
    background-color: transparent;
    padding: 0;

    /* 4. МАГИЯ: Авто-отступ вытолкнет кнопки в самый низ */
    margin-top: auto;

    /* Добавим безопасный отступ сверху, чтобы инпуты никогда не липли к кнопкам */
    padding-top: 3rem;
  }
}


/* ==========================================================================
   3. ДОСТУПНОСТЬ И АНИМАЦИЯ ПЕРЕХОДОВ VUE <Transition>
   ========================================================================== */

/* Стили для анимации <Transition name="fade" mode="out-in"> */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Фикс для снижения интенсивности движения */
@media (prefers-reduced-motion: reduce) {

  /* stylelint-disable declaration-no-important */
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }

  .wizard__content {
    transform: none !important;
    transition: none !important;
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: none !important;
  }

  /* stylelint-enable declaration-no-important */
}
</style>
