<script setup>
import useGames from '@/stores/gameStore'
import GameCard from '@/components/GameCard.vue'
import GamePagination from '@/components/GamePagination.vue'
import GameFilters from '@/components/GameFilters.vue'

const { gameSlice } = useGames()
</script>
<template>
  <main>
    <h1>Games</h1>
    <GameFilters />
    <GamePagination />
    <GameCard v-for="game of gameSlice" :key="game?.id" :game="game" />
  </main>
</template>