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
/* ==========================================================================
   1. БАЗОВЫЕ ТЕКСТОВЫЕ СТИЛИ ШАГА (Общие для всех шагов)
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

.step-body__title {
  color: var(--marine-blue);
  font-size: 1.5rem;
  /* 24px на мобильных */
  font-weight: 700;
  margin-bottom: 0.5rem;
  /* 8px */
}

.step-body__description {
  color: var(--cool-gray);
  font-size: 1rem;
  /* 16px */
  line-height: 1.5;
  margin-bottom: 1.5rem;
  /* 24px */
}


/* ==========================================================================
   2. МОБИЛЬНЫЕ СТИЛИ (По умолчанию — вертикальный список)
   ========================================================================== */

/* Группа планов через CSS Grid */
.plans-group {
  border: none;
  padding: 0;
  margin: 0 0 1.5rem 0;
  /* 24px отступ снизу до переключателя */
  display: grid;
  grid-template-columns: 1fr;
  /* Одна колонка на мобильных */
  gap: 0.75rem;
  /* 12px между карточками */
}

/* Базовая карточка тарифа */
.plan-card {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  /* 16px */
  padding: 1rem;
  /* 16px */
  border: 1px solid var(--light-gray);
  border-radius: 0.5rem;
  /* 8px */
  background-color: var(--white);
  cursor: pointer;
  transition: border-color 0.2s ease, background-color 0.2s ease;
}

.plan-card:hover {
  border-color: var(--purplish-blue);
}

/* Связка с вашим JS: класс :class="{ 'plan-card--selected': ... }" */
.plan-card--selected {
  border-color: var(--purplish-blue);
  background-color: var(--alabaster);
}

.plan-card__icon {
  width: 2.5rem;
  /* 40px */
  height: 2.5rem;
  flex-shrink: 0;
}

.plan-card__info {
  display: flex;
  flex-direction: column;
}

.plan-card__name {
  color: var(--marine-blue);
  font-weight: 500;
  font-size: 1rem;
  /* 16px */
  margin-bottom: 0.25rem;
  /* 4px */
}

.plan-card__price {
  color: var(--cool-gray);
  font-size: 0.875rem;
  /* 14px */
}

/* Бонусный текст при годовой оплате */
.plan-card__bonus {
  color: var(--marine-blue);
  font-size: 0.75rem;
  /* 12px */
  margin-top: 0.25rem;
  /* 4px */
}

/* --- ПЕРЕКЛЮЧАТЕЛЬ ПЕРИОДА (Блок подложки) --- */
.billing-toggle {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
  /* 24px */
  background-color: var(--alabaster);
  padding: 0.875rem;
  /* 14px */
  border-radius: 0.5rem;
  /* 8px */
}

.billing-toggle__label {
  color: var(--cool-gray);
  font-size: 0.875rem;
  /* 14px */
  font-weight: 500;
  transition: color 0.2s ease;
}

.billing-toggle__label--active {
  color: var(--marine-blue);
  font-weight: 700;
}

/* Кнопка-переключатель (role="switch") */
.billing-toggle__switch {
  position: relative;
  width: 2.375rem;
  /* 38px */
  height: 1.25rem;
  /* 20px */
  background-color: var(--marine-blue);
  border: none;
  border-radius: 0.625rem;
  /* 10px */
  cursor: pointer;
  padding: 0.25rem;
  /* 4px внутренний отступ для кружка */
}

/* Бегунок внутри переключателя */
.billing-toggle__circle {
  position: absolute;
  top: 0.25rem;
  left: 0.25rem;
  width: 0.75rem;
  /* 12px */
  height: 0.75rem;
  background-color: var(--white);
  border-radius: 50%;
  transition: transform 0.2s ease;
  /* Плавное скольжение бегунка */
}

.billing-toggle__switch--yearly .billing-toggle__circle {
  /* Сдвигаем кружок вправо. Расчет: ширина кнопки 38px - кружок 12px - отступы 8px = 18px сдвига */
  transform: translateX(1.125rem);
  /* 18px */
}


/* ==========================================================================
   3. ДЕСКТОПНЫЕ СТИЛИ (Экраны от 992px / 62rem)
   ========================================================================== */

@media (min-width: 62rem) {
  .step-body__title {
    font-size: 2rem;
    /* 32px на десктопе */
    margin-bottom: 0.625rem;
    /* 10px */
  }

  .step-body__description {
    margin-bottom: 2.5rem;
    /* 40px свободного пространства до карточек */
  }

  /* Перестраиваем сетку карточек в 3 равные колонки */
  .plans-group {
    grid-template-columns: repeat(3, 1fr);
    gap: 1.125rem;
    /* 18px между карточками на десктопе */
    margin-bottom: 2rem;
    /* 32px */
  }

  /* Карточка переключается на вертикальную структуру (как на десктопном макете) */
  .plan-card {
    flex-direction: column;
    justify-content: space-between;
    /* Иконка сверху, текст снизу */
    height: 10rem;
    /* 160px — фиксированная высота карточки */
    padding: 1.25rem;
    /* 20px */
    gap: 0;
    /* Сбрасываем флекс-гэп, так как работает space-between */
  }

  .plan-card__info {
    margin-top: auto;
    /* Прижимает блок с текстом к низу карточки */
  }

  .plan-card__name {
    margin-bottom: 0.375rem;
    /* 6px */
  }
}
</style>
