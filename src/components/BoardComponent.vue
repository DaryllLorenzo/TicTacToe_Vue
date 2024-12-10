<template>
    <div class="body">
    <div class="game-container">
        <div class="board">
            <div v-for="(cell, index) in board" :key="index" 
                 @click="clickEvent(index)" class="cell">
                <span v-if="cell === 'x'" style="color: red;">X</span>
                <span v-if="cell === 'o'" style="color: blue;">O</span>
            </div>
        </div>
        <div v-if="winner" class="result">{{ winner }} ha ganado!</div>
        <div v-if="isDraw" class="result">¡Empate!</div>
        <button @click="resetGame" class="reset-button">Reiniciar Juego</button>
    </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const board = ref(Array(9).fill(null));
const currentPlayer = ref('x');
const winner = ref(null);
const isDraw = ref(false);

// Combinaciones ganadoras
const winningCombinations = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
];

const checkWinner = () => {
    for (const combination of winningCombinations) {
        const [a, b, c] = combination;
        if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {
            winner.value = board.value[a];
            return;
        }
    }
    // Verificar empate
    if (board.value.every(cell => cell !== null)) {
        isDraw.value = true;
    }
};

const clickEvent = (index) => {
    if (!board.value[index] && !winner.value && !isDraw.value) {
        board.value[index] = currentPlayer.value;
        checkWinner();
        currentPlayer.value = currentPlayer.value === 'x' ? 'o' : 'x';
    }
};

const resetGame = () => {
    board.value = Array(9).fill(null);
    currentPlayer.value = 'x';
    winner.value = null;
    isDraw.value = false;
};
</script>

<style scoped>

.body {
    display: flex;
    justify-content: center;
    height: 100vh;
    align-items: center;
}

.game-container {
    text-align: center;
}

.board {
    display: grid;
    grid-template-columns: repeat(3, 0fr);
    gap: 0; /* Sin espacio entre celdas */
}

.cell {
    width: 50px;
    height: 50px;
    border: 1px solid #ccc;
    display: flex; /* Para centrar el contenido */
    justify-content: center;
    align-items: center;
}

span {
    font-size: 24px;
}

.result {
    margin-top: 20px;
    font-size: 20px;
}

.reset-button {
    margin-top: 10px;
}
</style>
