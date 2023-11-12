<template>
  <div
    class="flex flex-col items-center justify-center bg-neutral-200 h-screen gap-5"
  >
    <div class="flex flex-col items-center bg-white p-3 rounded-lg shadow-xl">
      <h2>Array Desordenado:</h2>
      <p>{{ unsortedArray }}</p>
    </div>
    <div class="flex flex-col items-center bg-white p-3 rounded-lg shadow-xl">
      <h2>Array Ordenado:</h2>
      <p class="font-medium">{{ sortedArray }}</p>
    </div>

    <button
      class="bg-white p-3 rounded-2xl shadow-xl font-semibold hover:bg-gray-300"
      @click="sortArray"
    >
      Ordenar
    </button>
  </div>
</template>

<script>
export default {
  name: "desafio2",
  data() {
    return {
      unsortedArray: [4, 2, 7, 1, 9, 3],
      sortedArray: [],
    };
  },
  methods: {
    sortArray() {
      this.sortedArray = this.quicksort([...this.unsortedArray]);
    },
    quicksort(arr) {
      if (arr.length <= 1) {
        return arr;
      }

      const pivot = arr[0];
      const left = [];
      const right = [];

      for (let i = 1; i < arr.length; i++) {
        arr[i] < pivot ? left.push(arr[i]) : right.push(arr[i]);
      }

      return [...this.quicksort(left), pivot, ...this.quicksort(right)];
    },
  },
};
</script>
