<template>
  <div id="app">
    <div class="d-flex flex-column align-items-center justify-content-center">
      <h1 class="text-white text-center">Sharknado</h1>
      <button style="width: 190px; height: 100px" @click="startGame" class="shark-button">
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
        <span class="teeth"></span>
      </button>
      <p class="text-white text-center">
        Click the mouth to let the Shark eat Yotobi!
      </p>
      <p class="f-d-eating" id="appearParagraph">
        Gnam gnam gnam gnam gnam 
      </p>
    </div>
    <div id="game-board" ref="gameBoard"></div>

  </div>
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
    appearParagraph() {
      document.getElementById('appearParagraph').style.display = 'block';
      setTimeout(() => {
        document.getElementById('appearParagraph').style.display = 'none';
      }, 2000); // 2000 millisecondi = 2 secondi
    },
    startGame() {
      clearInterval(this.interval); // Pulisci qualsiasi intervallo precedente
      this.snake = [{ x: 10, y: 10 }]; // Reimposta la posizione iniziale del serpente
      this.direction = { x: 0, y: 1 }; // Reimposta la direzione iniziale
      this.food = this.getRandomFoodPosition(); // Genera una nuova posizione del cibo
      this.interval = setInterval(this.moveSnake, 100);
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
      }
      // Controlla collisioni con il corpo del serpente
      for (let segment of this.snake) {
        if (head.x === segment.x && head.y === segment.y) {
          alert('Game Over!');
          clearInterval(this.interval);
          return;
        }
      }
      this.snake.unshift(head);

      // Controlla se il serpente ha mangiato il cibo
      if (head.x === this.food.x && head.y === this.food.y) {
        this.food = this.getRandomFoodPosition(); // Genera una nuova posizione del cibo
        this.appearParagraph();
      } else {
        this.snake.pop(); // Rimuovi l'ultimo segmento del serpente
      }

      this.drawGame();
    },
    changeDirection(event) {
      switch (event.key) {
        case 'ArrowUp':
          if (this.direction.y === 0) this.direction = { x: 0, y: -1 };
          break;
        case 'ArrowDown':
          if (this.direction.y === 0) this.direction = { x: 0, y: 1 };
          break;
        case 'ArrowLeft':
          if (this.direction.x === 0) this.direction = { x: -1, y: 0 };
          break;
        case 'ArrowRight':
          if (this.direction.x === 0) this.direction = { x: 1, y: 0 };
          break;
      }
    },
    getRandomFoodPosition() {
      let newFoodPosition;
      do {
        newFoodPosition = {
          x: Math.floor(Math.random() * this.boardSize),
          y: Math.floor(Math.random() * this.boardSize),
        };
      } while (this.snake.some(segment => segment.x === newFoodPosition.x && segment.y === newFoodPosition.y));
      return newFoodPosition;
    },
    drawGame() {
      this.$nextTick(() => {
        const gameBoard = this.$refs.gameBoard;
        gameBoard.innerHTML = ''; // Pulisce il tabellone

        for (let i = 0; i < this.boardSize; i++) {
          for (let j = 0; j < this.boardSize; j++) {
            const cell = document.createElement('div');
            cell.classList.add('grid-cell');

            if (this.snake.some(segment => segment.x === j && segment.y === i)) {
              cell.classList.add('snake');
            } else if (this.food.x === j && this.food.y === i) {
              cell.classList.add('food');
            }

            gameBoard.appendChild(cell);
          }
        }
      });
    },
  },
      mounted() {
    this.$nextTick(() => {
      this.drawGame();
    });
  },
}
</script>

<style lang="scss" scoped></style>