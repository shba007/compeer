<script setup lang="ts">
import confetti from 'canvas-confetti';

const userStore = useUserStore()
const testStore = useTestStore()

const { data, pending, error } = await useFetch(`/api/test`, {
  method: 'post', body: testStore.getResult(), onRequest: authInterceptor
})

onMounted(() => {
  confetti({
    particleCount: 100,
    spread: 70,
    origin: { y: 0.6 }
  })
})
</script>

<template>
  <main class="flex flex-col gap-8 items-center justify-center h-screen">
    <section class="mt-8">
      <Avatar :name="userStore.name ?? ''" />
    </section>
    <h2 class="text-xl">{{ userStore.name?.split(' ')[0] }}'s Report</h2>
    <section v-if="data" class="mb-auto flex justify-between items-start w-full">
      <PointCard topic="correct" :point="data.correctCount" color="blue" />
      <PointCard topic="wrong" :point="data.incorrectCount" color="red" />
      <PointCard topic="duration" :point="data.duration" color="green" />
    </section>
    <section v-else class="mb-auto flex justify-between items-start w-full">
      <PointCard topic="correct" :point="testStore.stats.correctCount" color="blue" />
      <PointCard topic="wrong" :point="testStore.stats.incorrectCount" color="red" />
      <PointCard topic="duration" :point="testStore.stats.duration" color="green" />
    </section>
    <AppButton title="Go to Leaderboard" href="/leaderboard" class="mt-auto" />
  </main>
</template>