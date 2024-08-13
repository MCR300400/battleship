<template>
  <div class="game-container">
    <h1>Battaglia Navale</h1>

    <div v-if="!shipsPlaced">
      <h2>Disposizione Navi</h2>
      <p>Posiziona le tue navi sulla griglia:</p>
      <div class="grid-container">
        <div class="grid">
          <div
            v-for="(cell, index) in playerGrid"
            :key="'player-' + index"
            class="grid-cell"
            :class="{ ship: cell === 'S' }"
            @click="placeShip(index)"
          >
            {{ cell }}
          </div>
        </div>
      </div>
      <button @click="startGame" class="btn">Inizia il Gioco</button>
    </div>

    <div v-else>
      <div class="grid-container">
        <div class="grid">
          <div
            v-for="(cell, index) in playerGrid"
            :key="'player-' + index"
            class="grid-cell"
            :class="{ ship: cell === 'S' }"
          >
            {{ cell }}
          </div>
        </div>

        <div class="grid">
          <div
            v-for="(cell, index) in opponentGrid"
            :key="'opponent-' + index"
            class="grid-cell"
            :class="{ hit: cell === 'H', miss: cell === 'M' }"
            @click="makeMove(index)"
          >
            {{ cell }}
          </div>
        </div>
      </div>

      <button v-if="gameOver" @click="restartGame" class="btn">
        Ricomincia
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "GameView",
  data() {
    return {
      currentPlayer: "Giocatore",
      gameStatus: "In gioco",
      gameOver: false,
      shipsPlaced: false,
      playerGrid: Array(100).fill(""),
      opponentGrid: Array(100).fill(""),
      shipPositions: [],
      computerShipPositions: this.generateRandomShipPositions(),
      availableComputerMoves: Array.from({ length: 100 }, (_, i) => i),
    };
  },
  methods: {
    placeShip(index) {
      if (this.playerGrid[index] === "" && this.shipPositions.length < 10) {
        this.playerGrid[index] = "S";
        this.shipPositions.push(index);
        if (this.shipPositions.length === 10) {
          this.shipsPlaced = true;
        }
      }
    },
    startGame() {
      if (this.shipPositions.length < 10) {
        alert("Posiziona tutte le navi prima di iniziare!");
        return;
      }
      this.shipsPlaced = true;
      this.currentPlayer = "Giocatore";
    },
    makeMove(index) {
      if (this.gameOver || this.currentPlayer !== "Giocatore") return;

      if (this.opponentGrid[index] !== "") return;

      if (this.computerShipPositions.includes(index)) {
        this.opponentGrid[index] = "H";
      } else {
        this.opponentGrid[index] = "M";
      }

      this.checkGameStatus();
      this.currentPlayer = "Computer";
      this.startComputerTurn();
    },
    startComputerTurn() {
      let moveMade = false;

      // Seleziona una cella casuale tra tutte le celle disponibili (inclusi 'S')
      const availableMoves = this.availableComputerMoves.filter(
        (i) => this.playerGrid[i] === ""
      );
      const otherMoves = this.availableComputerMoves.filter(
        (i) => this.playerGrid[i] === "S"
      );
      const combinedMoves = Array.from(
        new Set([...availableMoves, ...otherMoves])
      );
      console.log(combinedMoves.length);
      if (combinedMoves.length > 0) {
        const move =
          combinedMoves[Math.floor(Math.random() * combinedMoves.length)];
        // Controlla se la cella contiene una nave o meno
        if (this.computerShipPositions.includes(move)) {
          this.playerGrid[move] = "H"; // Colpito
        } else {
          this.playerGrid[move] = "M"; // Mancato
        }
        this.availableComputerMoves = this.availableComputerMoves.filter(
          (i) => i !== move
        );
        moveMade = true;
      }

      // Aggiorna lo stato del gioco e cambia giocatore
      if (moveMade) {
        this.checkGameStatus();
        this.currentPlayer = "Giocatore";
      }
    },
    generateRandomShipPositions() {
      const positions = [];
      while (positions.length < 10) {
        const randomIndex = Math.floor(Math.random() * 100);
        if (!positions.includes(randomIndex)) {
          positions.push(randomIndex);
        }
      }
      return positions;
    },
    checkGameStatus() {
      const playerShipsSunk = this.computerShipPositions.every(
        (pos) => this.opponentGrid[pos] === "H"
      );
      const computerShipsSunk = this.shipPositions.every(
        (pos) => this.playerGrid[pos] === "H"
      );

      if (playerShipsSunk) {
        this.gameStatus = "Hai perso!";
        this.gameOver = true;
      } else if (computerShipsSunk) {
        this.gameStatus = "Hai vinto!";
        this.gameOver = true;
      } else {
        this.gameStatus = `Tocca a ${this.currentPlayer}`;
      }
    },
    restartGame() {
      this.playerGrid = Array(100).fill("");
      this.opponentGrid = Array(100).fill("");
      this.shipPositions = [];
      this.computerShipPositions = this.generateRandomShipPositions();
      this.availableComputerMoves = Array.from({ length: 100 }, (_, i) => i);
      this.currentPlayer = "Giocatore";
      this.gameStatus = "In gioco";
      this.gameOver = false;
      this.shipsPlaced = false;
    },
  },
};
</script>

<style scoped>
.game-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

.game-info {
  margin-bottom: 20px;
  font-size: 1.2rem;
}

.grid-container {
  display: flex;
  gap: 20px;
  margin-bottom: 20px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(10, 40px);
  grid-gap: 2px;
}

.grid-cell {
  width: 40px;
  height: 40px;
  border: 1px solid #000;
  text-align: center;
  line-height: 40px;
  cursor: pointer;
  background-color: #f0f0f0;
}

.grid-cell.ship {
  background-color: #444;
}

.grid-cell.hit {
  background-color: #f00;
}

.grid-cell.miss {
  background-color: #00f;
}

.grid-cell:hover {
  background-color: #ddd;
}

.btn {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.btn:hover {
  background-color: #3e8e41;
}
</style>
