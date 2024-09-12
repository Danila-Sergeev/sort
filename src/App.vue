<template>
  <div>
    <h1>Введите количество дней</h1>
    <input type="number" v-model.number="inputData">
    <h2>{{ ageString }}</h2>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const inputData = ref(0);

const ageString = computed(() => {
  const years = Math.floor(inputData.value / 365);
  const remainingDays = inputData.value % 365;
  const months = Math.floor(remainingDays / 30);
  const days = remainingDays % 30;

  const yearString = years === 1 ? 'год' : (years >= 2 && years <= 4) ? 'года' : 'лет';
  const monthString = months === 1 ? 'месяц' : (months >= 2 && months <= 4) ? 'месяца' : 'месяцев';
  const dayString = days === 1 ? 'день' : (days >= 2 && days <= 4) ? 'дня' : 'дней';

  return `Мне ${years} ${yearString} ${months} ${monthString} и ${days} ${dayString}`;
});

const calculateAge = () => {
  // Можно добавить здесь проверку ввода, например, на отрицательное число
};
</script>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
