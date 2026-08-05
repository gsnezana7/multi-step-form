<script setup>
// Двустороннее связывание для массива дополнений
const selectedAddons = defineModel('addons')

// Принимаем период оплаты как обычный пропс (только для чтения), 
// чтобы знать, какие цены показывать — месячные или годовые
defineProps({
  billing: {
    type: String,
    required: true
  }
})
</script>
<template>
  <section class="step-body">
    <h2 class="step-body__title">Pick add-ons</h2>
    <p class="step-body__description">Add-ons help enhance your gaming experience.</p>

    <!-- Группируем чекбоксы в семантичный fieldset -->
    <fieldset class="addons-list">
      <legend class="visually-hidden">Available add-ons</legend>


      <label class="addon-card" :class="{ 'addon-card--selected': selectedAddons.includes('online') }">
        <input v-model="selectedAddons" class="addon-card__checkbox" type="checkbox" id="addon-online" value="online" />
        <div class="addon-card__info">

          <div class="addon-card__name">Online service</div>
          <p class="addon-card__description">Access to multiplayer games</p>
        </div>
        <span class="addon-card__price">
          {{ billing === 'monthly' ? '+$1/mo' : '+$10/yr' }}
        </span>
      </label>

      <label class="addon-card" :class="{ 'addon-card--selected': selectedAddons.includes('storage') }">
        <input v-model="selectedAddons" class="addon-card__checkbox" type="checkbox" id="addon-storage"
          value="storage" />
        <div class="addon-card__info">
          <div class="addon-card__name">Larger storage</div>
          <p class="addon-card__description">Extra 1TB of cloud save</p>
        </div>
        <span class="addon-card__price">
          {{ billing === 'monthly' ? '+$2/mo' : '+$20/yr' }}
        </span>
      </label>

      <label class="addon-card" :class="{ 'addon-card--selected': selectedAddons.includes('profile') }">
        <input v-model="selectedAddons" class="addon-card__checkbox" type="checkbox" id="addon-profile"
          value="profile" />
        <div class="addon-card__info">
          <div class="addon-card__name">Customizable profile</div>
          <p class="addon-card__description">Custom theme on your profile</p>
        </div>
        <span class="addon-card__price">
          {{ billing === 'monthly' ? '+$2/mo' : '+$20/yr' }}
        </span>
      </label>

    </fieldset>
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

/* ==========================================================================
   2. МОБИЛЬНЫЕ СТИЛИ (По умолчанию)
   ========================================================================== */

.addons-list {
  border: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  /* 12px */
}

/* Карточка-строка дополнения */
.addon-card {
  display: flex;
  align-items: center;
  padding: 0.75rem 1rem;
  /* 12px 16px */
  border: 1px solid var(--light-gray);
  border-radius: 0.5rem;
  /* 8px */
  cursor: pointer;
  background-color: var(--white);
  transition: border-color 0.2s ease, background-color 0.2s ease;
}

.addon-card:hover {
  border-color: var(--purplish-blue);
}

.addon-card:has(.addon-card__checkbox:focus-visible) {
  border-color: var(--purplish-blue);
  box-shadow: 0 0 0 2px var(--magnolia), 0 0 0 4px var(--purplish-blue);
}

/* Состояние, когда чекбокс выбран */
.addon-card--selected {
  border-color: var(--purplish-blue);
  background-color: var(--magnolia);
}

/* Стилизация кастомного чекбокса */
.addon-card__checkbox {
  appearance: none;
  /* Скрываем стандартный чекбокс браузера */
  width: 1.25rem;
  /* 20px */
  height: 1.25rem;
  /* 20px */
  border: 1px solid var(--light-gray);
  border-radius: 0.25rem;
  /* 4px */
  margin-right: 1rem;
  /* 16px */
  display: grid;
  place-content: center;
  cursor: pointer;
  transition: background-color 0.2s, border-color 0.2s;
}

.addon-card__checkbox:checked {
  background-color: var(--purplish-blue);
  border-color: var(--purplish-blue);
}

/* Рисуем галочку внутри чекбокса */
.addon-card__checkbox:checked::before {
  content: "";
  width: 0.625rem;
  /* 10px */
  height: 0.375rem;
  /* 6px */
  border-left: 2px solid var(--white);
  border-bottom: 2px solid var(--white);
  transform: rotate(-45deg) translate(0.0625rem, -0.0625rem);
  /* translate(1px, -1px) */
}

.addon-card__info {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  /* 4px */
  flex-grow: 1;
  /* Выталкивает цену в правый край */
}

.addon-card__name {
  font-size: 0.875rem;
  /* 14px */
  font-weight: 500;
  color: var(--marine-blue);
}

.addon-card__description {
  font-size: 0.75rem;
  /* 12px */
  color: var(--cool-gray);
}

.addon-card__price {
  font-size: 0.75rem;
  /* 12px */
  color: var(--purplish-blue);
}

/* ==========================================================================
   3. ДЕСКТОПНЫЕ СТИЛИ (Экраны от 992px / 62rem)
   ========================================================================== */

@media (min-width: 62rem) {

  .step-body__title {
    font-size: 2rem;
    /* 32px */
    margin-bottom: 0.5rem;
    /* 8px */
  }

  .step-body__description {
    margin-bottom: 2.1875rem;
    /* 35px свободного места до списка */
  }

  /* Ограничиваем максимальную ширину списка на больших мониторах */
  .addons-list {
    max-width: 28.125rem;
    /* 450px — встает ровно под линию полей первого шага */
  }

  .addon-card {
    padding: 1.125rem 1.5rem;
    /* 18px 24px */
  }

  .addon-card__name {
    font-size: 1rem;
    /* 16px */
  }

  .addon-card__description {
    font-size: 0.875rem;
    /* 14px */
  }

  .addon-card__price {
    font-size: 0.875rem;
    /* 14px */
  }
}
</style>
