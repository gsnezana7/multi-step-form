<script setup>
import { computed } from 'vue'

// Принимаем объект formData из App.vue
const props = defineProps({
  formData: {
    type: Object,
    required: true
  }
})

// Регистрируем событие клика по кнопке Change
const emit = defineEmits(['change-plan'])

// 1. Справочник цен для тарифов и дополнений
const prices = {
  plans: {
    Arcade: { monthly: 9, yearly: 90 },
    Advanced: { monthly: 12, yearly: 120 },
    Pro: { monthly: 15, yearly: 150 }
  },
  addons: {
    online: { name: 'Online service', monthly: 1, yearly: 10 },
    storage: { name: 'Larger storage', monthly: 2, yearly: 20 },
    profile: { name: 'Customizable profile', monthly: 2, yearly: 20 }
  }
}

// 2. Вычисляем стоимость выбранного тарифа
const planPrice = computed(() => {
  const currentPlan = props.formData.plan
  const currentBilling = props.formData.billing
  return prices.plans[currentPlan][currentBilling]
})

// 3. Формируем массив объектов для выбранных дополнений
const selectedAddonsDetails = computed(() => {
  const currentBilling = props.formData.billing
  return props.formData.addons.map(addonKey => {
    const addonData = prices.addons[addonKey]
    return {
      name: addonData.name,
      price: addonData[currentBilling]
    }
  })
})

// 4. Считаем финальную сумму за всё
const totalPrice = computed(() => {
  const addonsTotal = selectedAddonsDetails.value.reduce((sum, item) => sum + item.price, 0)
  return planPrice.value + addonsTotal
})

// Вспомогательное свойство для сокращения текста (mo или yr)
const isMonthly = computed(() => props.formData.billing === 'monthly')
</script>
<template>
  <section class="step-body">
    <h2 class="step-body__title">Finishing up</h2>
    <p class="step-body__description">Double-check everything looks OK before confirming.</p>
    <div class="summary-container">


      <!-- Главная серая карточка с итогами -->
      <div class="summary-card">

        <!-- Верхняя часть: Тариф -->
        <div class="summary-card__main">
          <div class="summary-card__plan-info">
            <div class="summary-card__plan-name">
              {{ formData.plan }} ({{ isMonthly ? 'Monthly' : 'Yearly' }})
            </div>

            <button @click="emit('change-plan')" class="summary-card__change-btn" type="button"
              aria-label="Change subscription plan">
              Change
            </button>
          </div>
          <span class="summary-card__plan-price">
            ${{ planPrice }}/{{ isMonthly ? 'mo' : 'yr' }}
          </span>
        </div>

        <!-- Разделительная линия -->
        <hr v-if="formData.addons.length > 0" class="summary-card__divider" />

        <dl v-if="formData.addons.length > 0" class="summary-card__addons-list">
          <!-- Обертка div внутри dl валидна по спецификации HTML5 -->
          <div v-for="addon in selectedAddonsDetails" :key="addon.name" class="summary-card__addon-item">
            <!-- dt — название услуги -->
            <dt class="summary-card__addon-name">{{ addon.name }}</dt>
            <!-- dd — её значение/цена -->
            <dd class="summary-card__addon-price">
              +${{ addon.price }}/{{ isMonthly ? 'mo' : 'yr' }}
            </dd>
          </div>
        </dl>

      </div>

      <dl class="summary-total">
        <div class="summary-total__wrapper">
          <dt class="summary-total__label">
            Total (per {{ isMonthly ? 'month' : 'year' }})
          </dt>
          <dd class="summary-total__sum">
            +${{ totalPrice }}/{{ isMonthly ? 'mo' : 'yr' }}
          </dd>
        </div>
      </dl>
    </div>
  </section>
</template>



<style scoped>
/* ==========================================================================
   1. ТЕКСТОВЫЕ СТИЛИ (Общие для всех шагов)
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
  margin-bottom: 1.375rem;
  /* 22px */
}

/* Обертка для чека, ограничивающая его ширину на больших экранах */
.summary-container {
  width: 100%;
}

/* ==========================================================================
   2. МОБИЛЬНЫЕ СТИЛИ (По умолчанию)
   ========================================================================== */

/* Серая подложка для чека */
.summary-card {
  background-color: var(--magnolia);
  padding: 1rem;
  /* 16px */
  border-radius: 0.5rem;
  /* 8px */
}

/* Строка тарифа */
.summary-card__main {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.summary-card__plan-name {
  font-size: 0.875rem;
  /* 14px */
  font-weight: 700;
  color: var(--marine-blue);
}

/* Кнопка "Change" */
.summary-card__change-btn {
  background: none;
  border: none;
  font-family: inherit;
  font-size: 0.875rem;
  /* 14px */
  color: var(--cool-gray);
  text-decoration: underline;
  cursor: pointer;
  padding: 0;
  margin-top: 0.25rem;
  /* 4px */
  display: block;
  transition: color 0.2s ease;
}

.summary-card__change-btn:hover {
  color: var(--purplish-blue);
}

.summary-card__plan-price {
  font-size: 0.875rem;
  /* 14px */
  font-weight: 700;
  color: var(--marine-blue);
}

/* Линия разрыва */
.summary-card__divider {
  border: none;
  border-top: 1px solid var(--light-gray);
  margin: 1rem 0;
  /* 16px */
}

/* Список дополнений */
.summary-card__addons-list {
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  /* 12px */
}

.summary-card__addon-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.summary-card__addon-name {
  font-size: 0.875rem;
  /* 14px */
  color: var(--cool-gray);
}

.summary-card__addon-price {
  margin: 0;
  /* Сброс дефолтных отступов dd */
  font-size: 0.875rem;
  /* 14px */
  color: var(--marine-blue);
}

/* Итоговый блок снизу */
.summary-total {
  margin: 0;
  padding: 0;
}

.summary-total__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1.5rem 1rem 0 1rem;
  /* 24px 16px 0 16px */
  width: 100%;
}

.summary-total__label {
  font-size: 0.875rem;
  /* 14px */
  color: var(--cool-gray);
}

/* Финальная крупная сумма */
.summary-total__sum {
  margin: 0;
  /* Сброс дефолтных отступов dd */
  font-size: 1rem;
  /* 16px */
  font-weight: 700;
  color: var(--purplish-blue);
}

/* ==========================================================================
   3. ДЕСКТОПНЫЕ СТИЛИ (Экраны от 992px / 62rem)
   ========================================================================== */

@media (min-width: 62rem) {

  /* Синхронизация с глобальным брейкпоинтом визарда */
  .step-body__title {
    font-size: 2rem;
    /* 32px */
    margin-bottom: 0.5rem;
    /* 8px */
  }

  .step-body__description {
    margin-bottom: 2.1875rem;
    /* 35px свободного места до карточки */
  }

  /* Ограничиваем максимальную ширину всего чека по одной линии с полями ввода */
  .summary-container {
    max-width: 28.125rem;
    /* 450px */
  }

  .summary-card {
    padding: 1.5rem;
    /* 24px */
  }

  .summary-card__plan-name,
  .summary-card__plan-price {
    font-size: 1rem;
    /* 16px */
  }

  .summary-total__wrapper {
    padding: 1.5rem 1.5rem 0 1.5rem;
    /* Делаем боковые отступы итога вровень с карточкой */
  }

  .summary-total__sum {
    font-size: 1.25rem;
    /* 20px */
  }
}
</style>
