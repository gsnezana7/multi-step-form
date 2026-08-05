<script setup>
import { ref, onMounted } from 'vue'

const successTitle = ref(null)

onMounted(() => {
  // Как только экран успеха появился, мгновенно переводим фокус на заголовок
  successTitle.value?.focus()
})
</script>

<template>
  <section class="step-success">
    <!-- Иконка скрыта, так как является чисто декоративной -->
    <img class="step-success__icon" src="../assets/images/icon-thank-you.svg" alt="" aria-hidden="true" />

    <!-- 
      tabindex="-1" позволяет сфокусировать элемент через JS, 
      ref="successTitle" нужен для связи с секцией скриптов
    -->
    <h2 class="step-success__title" ref="successTitle" tabindex="-1">Thank you!</h2>

    <p class="step-success__description">
      Thanks for confirming your subscription! We hope you have fun using our platform.
      If you ever need support, please feel free to email us at support@loremgaming.com.
    </p>
  </section>
</template>

<style scoped>
/* ==========================================================================
   1. МОБИЛЬНЫЕ СТИЛИ (По умолчанию)
   ========================================================================== */

/* Центрируем весь контент внутри карточки успеха */
.step-success {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 2rem 0;
  /* 32px */
  margin: auto 0;
  /* Центрирует блок по вертикали, если вокруг есть место */
}

.step-success__icon {
  width: 3.5rem;
  /* 56px */
  height: 3.5rem;
  margin-bottom: 1.5rem;
  /* 24px */
}

/* Заголовок успеха */
.step-success__title {
  font-size: 1.5rem;
  /* 24px */
  font-weight: 700;
  color: var(--marine-blue);
  margin-bottom: 0.75rem;
  /* 12px */
  outline: none;
  /* Убираем дефолтную рамку аутлайна при JS-фокусе */
}

/* Описание */
.step-success__description {
  font-size: 1rem;
  /* 16px */
  color: var(--cool-gray);
  line-height: 1.6;
}

/* ==========================================================================
   2. ДЕСКТОПНЫЕ СТИЛИ (Экраны от 992px / 62rem)
   ========================================================================== */

@media (min-width: 62rem) {

  /* Синхронизировали с глобальной сеткой .wizard */
  .step-success {
    padding: 2.5rem 1.25rem;
    /* 40px 20px */
    /* Выравниваем ширину контента под общую линию полей ввода предыдущих шагов */
    max-width: 28.125rem;
    /* 450px */
    margin: auto;
    /* Идеально центрирует блок и по вертикали, и по горизонтали */
  }

  .step-success__icon {
    width: 5rem;
    /* 80px */
    height: 5rem;
    margin-bottom: 2rem;
    /* 32px */
  }

  .step-success__title {
    font-size: 2rem;
    /* 32px */
    margin-bottom: 1rem;
    /* 16px */
  }
}
</style>
