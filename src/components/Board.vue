<template>
  <div class="board-wrapper">
    <div class="board">
      <BoardItem
        v-for="field in fields"
        :key="'field-' + field.id"
        :field="field"
        :gameStatus="gameStatus"
        @selectField="selectField($event)"
      >
      </BoardItem>
    </div>
    <p class="difficult">
      Сложность: <strong>{{ difficult }}</strong> Время:
      <strong>{{ time_out }}</strong>
    </p>

    <p v-if="gameStatus === 4">Вы проиграли. Нажмите заново старт</p>
    <button class="btn" @click="start" :disabled="isDisabled">Старт</button>
    <button class="btn" @click="restart" :disabled="isDisabled">реСтарт</button>
  </div>
</template>

<script>
import { ref, onBeforeMount } from "vue";
import BoardItem from "./BoardItem.vue";
export default {
  name: "Board",
  components: {
    BoardItem,
  },
  setup() {
    const number_fields = 25;
    let isDisabled = ref(false);
    let gameStatus = ref(0);
    let difficult = ref(3);
    let fields = ref([]);
    let game_speed = 2000;
    let time_out = ref(12);
    let total_click = 0;

    const init = () => {
      fields.value = [];
      for (let i = 0; i < number_fields; i++) {
        fields.value.push({
          id: i,
          clicked: false,
          value: 0,
        });
      }
    };

    const start = () => {
      init();
      prepareGame();
      timer();
    };
    const restart = () => {
      difficult.value = 3;
      start();
    };

    const prepareGame = () => {
      isDisabled.value = true;
      gameStatus.value = 1;
      for (let i = 0; i < difficult.value; i++) {
        const index = rand(0, number_fields - 1);
        if (fields.value[index].value !== 1) {
          fields.value[index].value = 1;
        } else {
          i--;
        }
      }

      setTimeout(() => {
        isDisabled.value = false;
        gameStatus.value = 2;
      }, game_speed);
    };

    const rand = (min, max) => {
      return Math.floor(Math.random() * (max - min) + min);
    };

    const selectField = (id) => {
      const index = fields.value.findIndex((field) => {
        return field.id === id;
      });

      if (index > -1 && !fields.value[index].clicked) {
        fields.value[index].clicked = true;
        checkGame();
      }
    };

    const checkGame = () => {
      const errorIndex = fields.value.findIndex((field) => {
        return field.clicked && field.value === 0;
      });

      if (errorIndex > -1) {
        setGameOver();
        return;
      }

      const notFoundItemIndex = fields.value.findIndex((field) => {
        return !field.clicked && field.value === 1;
      });

      if (notFoundItemIndex === -1) {
        setWin();
      }
    };

    // timeout for levels

    const timer = () => {
      time_out.value = 12;
      total_click++;
      if (total_click == 1) {
        const timer2 = setInterval(() => {
          time_out.value -= 1;

          if (time_out.value == 0) {
            setGameOver();
            clearInterval(timer2);
            time_out.value = 12;
            total_click = 0;
          }
          if (gameStatus.value == 4) {
            clearInterval(timer2);
            total_click = 0;
          }
        }, 1000);
      }
    };

    const setGameOver = () => {
      gameStatus.value = 4;
      setTimeout(() => {
        difficult.value = 3;
      }, game_speed);
    };

    const setWin = () => {
      gameStatus.value = 3;
      setTimeout(() => {
        difficult.value++;
        if (difficult.value > 10) {
          difficult.value = 10;
        }
        start();
      }, game_speed);
    };

    onBeforeMount(init);

    return {
      number_fields,
      time_out,
      fields,
      difficult,
      isDisabled,
      gameStatus,
      game_speed,
      init,
      start,
      selectField,
      restart,
    };
  },
};
</script>

<style scoped>
.board-wrapper {
  margin-bottom: 100px;
}
.board {
  width: 300px;
  background: #ededed;
  margin: 0 auto;
}

.btn {
  background-color: #42b983cc;
  color: white;
  border-radius: 3px;
  padding: 10px;
  margin: 10px 0;
  cursor: pointer;
  outline: none;
  border: none;
}

.btn:disabled {
  opacity: 0.5;
}

.btn:hover {
  background-color: #42b983;
}
</style>
