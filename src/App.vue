<template>
  <div>
    <div>
      <label for="array-size" class="label">Размер массива:</label>
      <input type="number" id="array-size" v-model.number="arraySize" min="10" max="10000">
    </div>
    <div>
      <label for="range-min" class="label">Минимальное значение:</label>
      <input type="number" id="range-min" v-model.number="rangeMin" min="-100">
    </div>
    <div>
      <label for="range-max" class="label">Максимальное значение:</label>
      <input type="number" id="range-max" v-model.number="rangeMax" max="100">
    </div>
    <button @click="generateArray">Сгенерировать массив</button>
    <div v-if="array.length > 0">
      <h3>Исходный массив:</h3>
      <ul class="array-list">
        <li v-for="item in array" :key="item" class="array-item">{{ item }}</li>
      </ul>
      <div v-for="(algorithm, name) in algorithms" :key="name" class="algorithm-section">
        <h4>{{ name }}:</h4>
        <button @click="sortArray(algorithm, name)" class="sort-button">Сортировать</button>
        <ul v-if="sortedArrays[name]" class="array-list">
          <h3>Отсортированный массив:</h3>
          <li v-for="item in sortedArrays[name]" :key="item" class="array-item">{{ item }}</li>
        </ul>
        <div v-if="sortedArrays[name]">
          <p>Время сортировки: {{ sortedTimes[name] }} мс</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arraySize: 100,
      rangeMin: -100,
      rangeMax: 100,
      array: [],
      sortedArrays: {},
      sortedTimes: {},
      algorithms: {
        'Пузырьковая сортировка': this.bubbleSort,
        'Быстрая сортировка': this.quickSort,
        'Сортировка слиянием': this.mergeSort,
        'Сортировка вставками': this.insertionSort
      },
    };
  },
  methods: {
    generateArray() {
      this.array = Array.from({ length: this.arraySize }, () =>
        Math.floor(Math.random() * (this.rangeMax - this.rangeMin + 1)) + this.rangeMin
      );
      this.sortedArrays = {};
      this.sortedTimes = {};
    },
    sortArray(algorithm, name) {
      const startTime = performance.now();

      // Создание копии массива перед сортировкой
      const arrToSort = [...this.array]; // Используем spread operator для создания копии

      const sortedArray = algorithm(arrToSort); 
      const endTime = performance.now();
      this.sortedArrays[name] = sortedArray; 
      this.sortedTimes[name] = endTime - startTime;

      this.$nextTick(() => {
        // Обновление DOM после завершения обработки данных
      });
    },
    bubbleSort(arr) {
      // Пузырьковая сортировка:
      for (let i = 0; i < arr.length - 1; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
          if (arr[j] > arr[j + 1]) {
            [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // Обмен элементов, если они в неправильном порядке
          }
        }
      }
      return arr; 
    },
    quickSort(arr) {
      // Быстрая сортировка O(nlogn):
      if (arr.length <= 1) {
        return arr;
      }
      const midIndex = Math.floor(arr.length / 2); // Выбираем опорный элемент (mid)
      const mid = arr[midIndex];
      let i = 0;
      let j = arr.length - 1;
      while (i < j) {
        // Двигаем i влево пока не найдем элемент больше mid
        while (arr[i] < mid) {
          i++;
        }
        // Двигаем j вправо пока не найдем элемент меньше mid
        while (arr[j] > mid) {
          j--;
        }
        // Если i < j, меняем элементы местами
        if (i < j) {
          [arr[i], arr[j]] = [arr[j], arr[i]];
          i++;
          j--;
        }
      }
      // Рекурсивно сортируем левую и правую части
      return [...this.quickSort(arr.slice(0, i)), mid, ...this.quickSort(arr.slice(i + 1))];
    },
    mergeSort(arr) {
      // Сортировка слиянием  O(N log N:
      if (arr.length <= 1) {
        return arr;
      }
      const mid = Math.floor(arr.length / 2); 
      const left = this.mergeSort(arr.slice(0, mid));
      const right = this.mergeSort(arr.slice(mid));
      return this.merge(left, right);
    },
    merge(left, right) {
      // Слияние двух отсортированных массивов:
      const result = [];
      let i = 0;
      let j = 0;
      while (i < left.length && j < right.length) {
        if (left[i] <= right[j]) {
          result.push(left[i]); 
          i++; 
        } else {
          result.push(right[j]); 
          j++; 
        }
      }
      return result.concat(left.slice(i)).concat(right.slice(j)); 
    },

    insertionSort(arr){
        for (let i = 1; i < arr.length; i++) {    /* [5, 2, 4, 6, 1, 3], [2, 5, 4, 6, 1, 3]  O(n^2)*/ 
        let compare = arr[i];  /* compare = 2  compare = 4*/
        let j = i - 1;  /* j = 0 j = 1*/
        while (j >= 0 && arr[j] > compare) { /* 0 >= 0 && 5 > 2,  */
          arr[j + 1] = arr[j]; /* arr[j + 1] = 5 [2, 5, 4, 6, 1, 3]*/
          j--;  /* j = -1 */
        }
        arr[j + 1] = compare; /* arr[0] = 2 */
      }
      return arr;
    }
  },
};
</script>

<style scoped>
.label{
  margin-right: 10px;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 600px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
}

.sort-button {
  padding: 10px 20px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.array-list {
  list-style: none;
  padding: 0;
  margin-bottom: 15px;
  overflow-wrap: break-word; /* Разрешает переносы слов внутри строки */
  word-break: break-all; /* Разрешает разрыв слов на границе слова */
  overflow-x: auto;
  max-width: 500px;
  overflow-y: auto;
  height: 200px;

}

.array-list li {
  display: inline-block; /* Отображает элементы списка в строке */
  margin-right: 5px; 
}

.array-item {
  display: inline-block;
  margin-right: 5px;
}

.algorithm-section {
  margin-bottom: 20px;
  border: 1px solid #eee;
  padding: 10px;
  border-radius: 5px;
}
</style>