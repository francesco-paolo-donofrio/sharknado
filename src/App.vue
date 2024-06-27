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
  },
  methods: {
    startGame() {
      this.interval = setInterval(this.moveSnake, 200);
      window.addEventListener('keydown', this.changeDirection);
      clearInterval(this.interval); // Pulisci qualsiasi intervallo precedente
      this.snake = [{ x: 10, y: 10 }]; // Reimposta la posizione iniziale del serpente
      this.direction = { x: 0, y: 1 }; // Reimposta la direzione iniziale
      this.food = this.getRandomFoodPosition(); // Genera una nuova posizione del cibo
      this.interval = setInterval(this.moveSnake, 200);
      window.addEventListener('keydown', this.changeDirection);
    },
    moveSnake() {
      const head = {
        x: this.snake[0].x + this.direction.x,
        y: this.snake[0].y + this.direction.y,
      };

      // Controlla collisioni con i bordi del gioco
      if (head.x < 0 || head.x >= this.boardSize || head.y < 0 || head.y >= this.boardSize) {
        alert('Game Over!');
        clearInterval(this.interval);
        return;
      };
      // Controlla collisioni con il corpo del serpente
      for (let segment of this.snake) {
        if (head.x === segment.x && head.y === segment.y) {
          alert('Game Over!');
          clearInterval(this.interval);
          return;
        }
      };
      this.snake.unshift(head);

      // Controlla se il serpente ha mangiato il cibo
      if (head.x === this.food.x && head.y === this.food.y) {
        this.food = this.getRandomFoodPosition(); // Genera una nuova posizione del cibo
      } else {
        this.snake.pop(); // Rimuovi l'ultimo segmento del serpente
      }

      this.drawGame();
    },
  },

},

changeDirection(event) {
  // Logica per cambiare direzione
},
drawGame() {
  // Logica per disegnare il gioco
},
  },
mounted() {
  this.drawGame();
}
}
</script>

<style lang="scss" scoped></style>