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

      <!-- Дополнение 1: Online service -->
      <!-- Убрали лишний атрибут for -->
      <label class="addon-card" :class="{ 'addon-card--selected': selectedAddons.includes('online') }">
        <input v-model="selectedAddons" class="addon-card__checkbox" type="checkbox" id="addon-online" value="online" />
        <div class="addon-card__info">
          <!-- Заменили h2 на семантичный div -->
          <div class="addon-card__name">Online service</div>
          <p class="addon-card__description">Access to multiplayer games</p>
        </div>
        <span class="addon-card__price">
          {{ billing === 'monthly' ? '+$1/mo' : '+$10/yr' }}
        </span>
      </label>

      <!-- Дополнение 2: Larger storage -->
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

      <!-- Дополнение 3: Customizable profile -->
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

.addons-list {
  border: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* Карточка-строка дополнения */
.addon-card {
  display: flex;
  align-items: center;
  padding: 12px 16px;
  border: 1px solid var(--light-gray);
  border-radius: 8px;
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
  /* Добавила красивое кольцо фокуса */
}

/* Состояние, когда чекбокс выбран */
.addon-card--selected {
  border-color: var(--purplish-blue);
  background-color: var(--magnolia);
}

/* Стилизация чекбокса */
.addon-card__checkbox {
  appearance: none;
  /* Скрываем стандартный чекбокс браузера */
  width: 20px;
  height: 20px;
  border: 1px solid var(--light-gray);
  border-radius: 4px;
  margin-right: 16px;
  display: grid;
  place-content: center;
  cursor: pointer;
  transition: background-color 0.2s, border-color 0.2s;
}

/* Рисуем галочку внутри чекбокса при выборе */
.addon-card__checkbox:checked {
  background-color: var(--purplish-blue);
  border-color: var(--purplish-blue);
}

.addon-card__checkbox:checked::before {
  content: "";
  width: 10px;
  height: 6px;
  border-left: 2px solid var(--white);
  border-bottom: 2px solid var(--white);
  transform: rotate(-45deg) translate(1px, -1px);
}

.addon-card__info {
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex-grow: 1;
  /* Чтобы цена ушла в правый край */
}

.addon-card__name {
  font-size: 14px;
  font-weight: 500;
  color: var(--marine-blue);
}

.addon-card__description {
  font-size: 12px;
  color: var(--cool-gray);
}

.addon-card__price {
  font-size: 12px;
  color: var(--purplish-blue);
}

/* Адаптация под десктоп */
@media (min-width: 992px) {
  .step-body__title {
    font-size: 32px;
    margin-bottom: 8px;
  }

  .step-body__description {
    margin-bottom: 35px;
  }

  .addon-card {
    padding: 18px 24px;
  }

  .addon-card__name {
    font-size: 16px;
  }

  .addon-card__description {
    font-size: 14px;
  }

  .addon-card__price {
    font-size: 14px;
  }
}
</style>
