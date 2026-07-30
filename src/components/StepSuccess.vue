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
/* Центрируем весь контент внутри карточки */
.step-success {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 32px 0;
  margin: auto 0;
  /* Центрирование по вертикали на десктопе */
}

.step-success__icon {
  width: 56px;
  height: 56px;
  margin-bottom: 24px;
}

.step-success__title {
  font-size: 24px;
  font-weight: 700;
  color: var(--marine-blue);
  margin-bottom: 12px;
}

.step-success__description {
  font-size: 16px;
  color: var(--cool-gray);
  line-height: 1.6;
}

/* Десктопные стили */
@media (min-width: 992px) {
  .step-success {
    padding: 40px 20px;
  }

  .step-success__icon {
    width: 80px;
    height: 80px;
    margin-bottom: 32px;
  }

  .step-success__title {
    font-size: 32px;
    margin-bottom: 16px;
  }
}
</style>
