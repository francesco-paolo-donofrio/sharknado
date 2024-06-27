<template>
  <h1 class="text-center">Ciao</h1>
  <i class="fa fa-solid fa-home"></i>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      boardSize: 20, // dimensioni della griglia di gioco
      snake: [{ x: 10, y: 10 }], // posizione iniziale del serpente
      direction: { x: 0, y: 1 }, // direzione iniziale (verso il basso)
      food: { x: 15, y: 15 }, // posizione iniziale del cibo
      interval: null,
    }
  }
}
</script>

<style lang="scss" scoped></style>