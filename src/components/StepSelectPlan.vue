<script setup>
// Принимаем тариф и период оплаты из App.vue
const selectedPlan = defineModel('plan')
const billingPeriod = defineModel('billing')

// Функция для переключения периода (месяц / год)
const toggleBilling = () => {
  billingPeriod.value = billingPeriod.value === 'monthly' ? 'yearly' : 'monthly'
}
</script>
<template>
  <section class="step-body">
    <h2 class="step-body__title">Select your plan</h2>
    <p class="step-body__description">You have the option of monthly or yearly billing.</p>

    <!-- Поле формы группируем в семантичный fieldset -->
    <fieldset class="plans-group">
      <legend class="visually-hidden">Subscription plans</legend>

      <!-- Карточка 1: Arcade -->
      <!-- label связывается с input автоматически, клик по карточке выберет радиокнопку -->
      <label class="plan-card" :class="{ 'plan-card--selected': selectedPlan === 'Arcade' }">
        <!-- Скрываем нативный инпут визуально в CSS (через visually-hidden), но оставляем для скринридеров -->
        <input type="radio" name="plan" value="Arcade" v-model="selectedPlan" class="visually-hidden" />
        <img class="plan-card__icon" src="../assets/images/icon-arcade.svg" alt="" aria-hidden="true" />
        <div class="plan-card__info">
          <div class="plan-card__name">Arcade</div>
          <span class="plan-card__price">
            {{ billingPeriod === 'monthly' ? '$9/mo' : '$90/yr' }}
          </span>
          <span v-if="billingPeriod === 'yearly'" class="plan-card__bonus">2 months free</span>
        </div>
      </label>

      <!-- Карточка 2: Advanced -->
      <label class="plan-card" :class="{ 'plan-card--selected': selectedPlan === 'Advanced' }">
        <input type="radio" name="plan" value="Advanced" v-model="selectedPlan" class="visually-hidden" />
        <img class="plan-card__icon" src="../assets/images/icon-advanced.svg" alt="" aria-hidden="true" />
        <div class="plan-card__info">
          <div class="plan-card__name">Advanced</div>
          <span class="plan-card__price">
            {{ billingPeriod === 'monthly' ? '$12/mo' : '$120/yr' }}
          </span>
          <span v-if="billingPeriod === 'yearly'" class="plan-card__bonus">2 months free</span>
        </div>
      </label>

      <!-- Карточка 3: Pro -->
      <label class="plan-card" :class="{ 'plan-card--selected': selectedPlan === 'Pro' }">
        <input type="radio" name="plan" value="Pro" v-model="selectedPlan" class="visually-hidden" />
        <img class="plan-card__icon" src="../assets/images/icon-pro.svg" alt="" aria-hidden="true" />
        <div class="plan-card__info">
          <div class="plan-card__name">Pro</div>
          <span class="plan-card__price">
            {{ billingPeriod === 'monthly' ? '$15/mo' : '$150/yr' }}
          </span>
          <span v-if="billingPeriod === 'yearly'" class="plan-card__bonus">2 months free</span>
        </div>
      </label>
    </fieldset>

    <!-- Переключатель Monthly / Yearly (Оставляем вашу отличную кнопку-свитч) -->
    <div class="billing-toggle">
      <span class="billing-toggle__label" :class="{ 'billing-toggle__label--active': billingPeriod === 'monthly' }"
        id="monthly-label">Monthly</span>

      <button @click="toggleBilling" class="billing-toggle__switch"
        :class="{ 'billing-toggle__switch--yearly': billingPeriod === 'yearly' }" type="button" role="switch"
        :aria-checked="billingPeriod === 'yearly'" aria-label="Toggle billing period">
        <span class="billing-toggle__circle"></span>
      </button>

      <span class="billing-toggle__label" :class="{ 'billing-toggle__label--active': billingPeriod === 'yearly' }"
        id="yearly-label">Yearly</span>
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
  margin-bottom: 22px;
}

/* Сетка для карточек (Мобильная версия — вертикальный стек) */
.plans-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
  /* Добавляем обнуление браузерных стилей */
  border: none;
  padding: 0;
  margin: 0;
}

/* Базовая карточка тарифа */
.plan-card {
  display: flex;
  align-items: flex-start;
  gap: 14px;
  padding: 16px;
  border: 1px solid var(--light-gray);
  border-radius: 8px;
  cursor: pointer;
  background-color: var(--white);
  transition: border-color 0.2s ease, background-color 0.2s ease;
}


.plan-card:hover {
  border-color: var(--purplish-blue);
}

.plan-card:has(input:focus-visible) {
  border-color: var(--purplish-blue);
  box-shadow: 0 0 0 2px var(--magnolia), 0 0 0 4px var(--purplish-blue);
}



/* Выбранная карточка тарифа */
.plan-card--selected {
  border-color: var(--purplish-blue);
  background-color: var(--magnolia);
}

.plan-card__info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.plan-card__name {
  font-size: 16px;
  font-weight: 500;
  color: var(--marine-blue);
}

.plan-card__price {
  font-size: 14px;
  color: var(--cool-gray);
}

/* Текст о бесплатных месяцах */
.plan-card__bonus {
  font-size: 12px;
  color: var(--marine-blue);
}



/* Переключатель периодов оплаты */
.billing-toggle {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 24px;
  background-color: var(--magnolia);
  padding: 12px;
  border-radius: 8px;
  margin-top: 24px;
}

.billing-toggle__label {
  font-size: 14px;
  font-weight: 500;
  color: var(--cool-gray);
  transition: color 0.2s ease;
}

.billing-toggle__label--active {
  color: var(--marine-blue);
  font-weight: 700;
}

/* Кнопка-переключатель */
.billing-toggle__switch {
  position: relative;
  width: 38px;
  height: 20px;
  background-color: var(--marine-blue);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  padding: 4px;
}

/* Кружок внутри переключателя по умолчанию (Monthly) */
.billing-toggle__circle {
  position: absolute;
  top: 3px;
  left: 4px;
  width: 14px;
  height: 14px;
  background-color: var(--white);
  border-radius: 50%;
  transition: transform 0.2s ease;
  /* Плавность анимации */
}

/* Смещаем кружок вправо, когда у СВИТЧА (кнопки) появляется класс --yearly */
.billing-toggle__switch--yearly .billing-toggle__circle {
  transform: translateX(16px);
}

/* Десктопные стили для Шага 2 */
@media (min-width: 62em) {
  .step-body__title {
    font-size: 32px;
    margin-bottom: 8px;
  }

  .step-body__description {
    margin-bottom: 35px;
  }

  /* На десктопе карточки выстраиваются в ряд */
  .plans-group {
    flex-direction: row;
    gap: 18px;
  }

  /* Карточки становятся вертикальными прямоугольниками */
  .plan-card {
    flex-direction: column;
    justify-content: space-between;
    flex-grow: 1;
    flex-basis: 0;
    height: 160px;
    padding: 20px 16px;
  }

  .plan-card__info {
    margin-top: auto;
    /* Прижимает тексты к низу карточки */
  }
}
</style>
