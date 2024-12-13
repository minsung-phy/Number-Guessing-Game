<template>
  <div id="app">
    <h1>숫자 맞추기 게임</h1>
    <p v-if="gameOver" class="game-over-message">
      🎉 축하합니다! 정답은 <span class="highlight">{{ targetNumber }}</span>입니다.
    </p>
    <p v-else class="instructions">
      숫자를 맞춰보세요! (1부터 100까지)
    </p>

    <div v-if="!gameOver" class="game-container">
      <input
        type="number"
        v-model.number="guess"
        placeholder="숫자를 입력하세요"
        class="guess-input"
      />
      <button @click="checkGuess" class="guess-button">확인</button>
      <p v-if="hint" class="hint-message">{{ hint }}</p>
    </div>

    <button v-if="gameOver" @click="restartGame" class="restart-button">
      다시 시작
    </button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  setup() {
    const targetNumber = ref(generateRandomNumber());
    const guess = ref(null);
    const hint = ref("");
    const gameOver = ref(false);

    function generateRandomNumber() {
      return Math.floor(Math.random() * 100) + 1;
    }

    function checkGuess() {
      if (guess.value === targetNumber.value) {
        gameOver.value = true;
      } else if (guess.value < targetNumber.value) {
        hint.value = "더 큰 숫자입니다!";
      } else {
        hint.value = "더 작은 숫자입니다!";
      }
    }

    function restartGame() {
      targetNumber.value = generateRandomNumber();
      guess.value = null;
      hint.value = "";
      gameOver.value = false;
    }

    return {
      targetNumber,
      guess,
      hint,
      gameOver,
      checkGuess,
      restartGame,
    };
  },
};
</script>

<style scoped>
/* 전체 배경 */
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 50px;
  background-color: #f0f4f8;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  max-width: 500px;
  margin: auto;
}

/* 헤더 */
h1 {
  color: #2c3e50;
  font-size: 36px;
  margin-bottom: 20px;
}

/* 지시문 및 메시지 */
.instructions,
.hint-message {
  font-size: 18px;
  color: #34495e;
}

.game-over-message {
  font-size: 20px;
  color: #27ae60;
  font-weight: bold;
}

.highlight {
  color: #e74c3c;
  font-weight: bold;
}

/* 입력창 */
.guess-input {
  font-size: 18px;
  padding: 10px;
  width: 60%;
  margin: 10px auto;
  border: 2px solid #bdc3c7;
  border-radius: 4px;
  outline: none;
  transition: border-color 0.3s ease;
}

.guess-input:focus {
  border-color: #3498db;
}

/* 버튼 */
.guess-button,
.restart-button {
  font-size: 18px;
  padding: 10px 20px;
  margin: 10px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

.guess-button:hover,
.restart-button:hover {
  background-color: #2980b9;
}

.guess-button:active,
.restart-button:active {
  transform: scale(0.98);
}

/* 게임 컨테이너 */
.game-container {
  margin-top: 20px;
}

/* 반응형 */
@media (max-width: 480px) {
  h1 {
    font-size: 28px;
  }

  .guess-input {
    width: 80%;
  }
}
</style>