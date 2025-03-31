<template>
  <div class="p-8 pb-0 text-orange-500">
    <h1 class="text-4xl font-bold mb-4">Random Meals</h1>
  </div>
  <div
    v-if="isError"
    class="text-3xl font-bold text-center text-lg py-8 text-red-500"
  >
    Something went wrong
  </div>
  <div v-else>
    <Meals :meals="meals" />
    <Pagination />
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import Meals from "../components/Meals.vue";
import Pagination from "../components/Pagination.vue";
import axiosClient from "../axiosClient.js";
import { useRoute } from "vue-router";

const meals = ref([]);
const router = useRoute();

watch(
  () => router.params.pageNumber,
  (newPage) => {
    if (newPage) {
      getNewMealList();
    }
  },
  { immediate: true }
);

function getNewMealList() {
  debugger;
  meals.value = [];
  for (let i = 0; i < 10; i++) {
    console.log(i);
    console.log(meals.value);
    axiosClient.get(`random.php`).then(({ data }) => {
      meals.value.push(data.meals[0]);
    });
  }
}

onMounted(async () => {
  debugger;
  getNewMealList();
});
</script>
