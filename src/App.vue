<template>
  <div>
    <h1>Введите количество дней</h1>
    <input type="number" v-model.number="inputData">
    <h2 v-if="inputData <= 2147483647">{{ ageString }}</h2>
    <h2 v-if="inputData >= 2147483647" style="color: red;">Ваше число превышает максимальное значение</h2>
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

  const yearString = getWordForm(years, 'год', 'года', 'лет');
  const monthString = getWordForm(months, 'месяц', 'месяца', 'месяцев');
  const dayString = getWordForm(days, 'день', 'дня', 'дней');

  // Проверяем, есть ли месяцы или дни
  if (months > 0 || days > 0) {
    return `Мне ${years} ${yearString} ${months} ${monthString} и ${days} ${dayString}`;
  } else {
    return `Мне ${years} ${yearString}`;
  }
});

const getWordForm = (number, singular, genitive2, plural) => {
  const lastDigit = number % 10;
  const lastTwoDigits = number % 100;

  if (lastDigit === 1 && lastTwoDigits !== 11) {
    return singular;
  } else if (lastDigit >= 2 && lastDigit <= 4 && (lastTwoDigits < 11 || lastTwoDigits > 14)) {
    return genitive2;
  } else {
    return plural;
  }
};
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
