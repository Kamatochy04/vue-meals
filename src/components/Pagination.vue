<template>
  <div class="flex items-center justify-end gap-2 py-8">
    <button
      class="block px-3 py-1 disabled:opacity-50"
      @click="goToPage(currentPage - 1)"
      :disabled="currentPage === 1"
    >
      &lt;
    </button>
    <div
      class="block"
      :class="{ 'bg-orange-500 text-white': currentPage === 1 }"
      @click="goToPage(1)"
    >
      1
    </div>
    <div v-if="currentPage > 3" class="px-2">...</div>
    <template v-for="page in middlePages">
      <div
        v-if="page > 1 && page < totalPages"
        class="block"
        :class="{ 'bg-orange-500 text-white': currentPage === page }"
        @click="goToPage(page)"
        :key="page"
      >
        {{ page }}
      </div>
    </template>
    <div v-if="currentPage < totalPages - 2" class="px-2">...</div>
    <div
      v-if="totalPages > 1"
      class="block"
      :class="{ 'bg-orange-500 text-white': currentPage === totalPages }"
      @click="goToPage(totalPages)"
    >
      {{ totalPages }}
    </div>
    <button
      class="block px-3 py-1 disabled:opacity-50"
      @click="goToPage(currentPage + 1)"
      :disabled="currentPage === totalPages"
    >
      &gt;
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import { useRoute, useRouter } from "vue-router";

const router = useRouter();
const route = useRoute();
const totalPages = ref(100);

const currentPage = computed(() => {
  return Number(route.params.pageNumber) || 1;
});

const middlePages = computed(() => {
  const pages = [];
  const start = Math.max(2, currentPage.value - 1);
  const end = Math.min(totalPages.value - 1, currentPage.value + 1);

  for (let i = start; i <= end; i++) {
    pages.push(i);
  }
  return pages;
});

function goToPage(pageNumber) {
  if (pageNumber < 1 || pageNumber > totalPages.value) return;

  router.push({
    name: "home",
    params: { pageNumber },
  });
}
</script>

<style scoped>
.block {
  background: #43434329;
  padding: 5px 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 40px;
  text-align: center;
}

.block:hover {
  background: #43434349;
}
</style>
