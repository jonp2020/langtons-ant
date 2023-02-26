<template>
  <div class="intro">
    <h1>Langton's Ant</h1>
    <p>
      Langton's ant is simulation developed by Chris Langton in 1986
      <a href="https://en.wikipedia.org/wiki/Langton%27s_ant"
        >Wikipedia link.</a
      >
      It follows some simple rules:
    </p>

    <ul>
      <li>
        An ant moves across a grid of squares one step at a time, either up,
        down, left or right.
      </li>
      <li>
        At a white square, turn 90° clockwise, flip the color of the square,
        move forward one unit.
      </li>
      <li>
        At a black square, turn 90° counter-clockwise, flip the color of the
        square, move forward one unit.
      </li>
      <li>
        In this version, if the ant attempts to move outside of the existing
        grid, the grid will expand to accomodate the movements of the ant.
      </li>
    </ul>
  </div>

  <div class="start-btn">
    <button @click="startSimulation">Start Simulation</button>
    <button @click="pauseSimulation">Pause Simulation</button>
  </div>
  <h3>Count: {{ counter }}</h3>
  <section class="grid-container">
    <div
      :style="{
        display: 'grid',
        gridTemplateColumns: `repeat(${rowsAndColumnsNumbers.length}, 20px)`,
        gridTemplateRows: `repeat(${rowsAndColumnsNumbers.length}, 20px)`,
      }"
    >
      <div v-for="row in rowsAndColumnsNumbers" :key="'row' + row" class="row">
        <div
          v-for="column in rowsAndColumnsNumbers"
          :key="'columnRow' + column + row"
          class="cell squareWhite"
          :class="`${column}_${row}`"
          :ref="`${column}_${row}`"
        >
          <!-- <p style="color: green; font-size: 5px">{{ column }}_{{ row }}</p> -->

          <div
            v-if="antCurrentPosition === `${column}_${row}`"
            style="
              background-color: red;
              width: 100%;
              height: 100%;
              display: flex;
              justify-content: center;
              position: absolute;
            "
          >
            <img
              class="ant"
              :class="`antDirection-${antDirection}`"
              :src="`src/assets/ant_${antDirection}.png`"
              alt="ant"
              ref="antImage"
            />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      // Number of squares in all directions from center the square 0 in the middle of the grid
      gridProportionFromCenter: 1,
      antCurrentPosition: "",
      // antDirection 0 = up; 1 = right; 2 = down; 3 = left
      antDirection: 0,
      antStartingPosition: "0_0",
      startSim: false,
      pauseSim: false,
      start: null,
      counter: 0,
    };
  },
  methods: {
    startSimulation() {
      if (this.start) return;
      this.start = setInterval(() => {
        this.moveAnt();
        this.counter++;
      }, 500);
    },
    pauseSimulation() {
      clearInterval(this.start);
      this.start = null;
    },
    getCellBackgroundColour(column, row) {
      if (this.antCurrentPosition === this.getGridCoordinate(column, row)) {
        return { backgroundColor: "red" };
      }
    },
    moveAnt(antCurrentPosition) {
      if (antCurrentPosition === "") return;

      const cellRef = this.$refs[this.antCurrentPosition];

      if (cellRef[0].classList.contains("squareWhite")) {
        cellRef[0].classList.remove("squareWhite");
        cellRef[0].classList.add("squareBlack");

        if (this.antDirection < 3) {
          this.antDirection++;
        } else {
          this.antDirection = 0;
        }
      } else if (cellRef[0].classList.contains("squareBlack")) {
        cellRef[0].classList.remove("squareBlack");
        cellRef[0].classList.add("squareWhite");

        if (this.antDirection > 0) {
          this.antDirection--;
        } else {
          this.antDirection = 3;
        }
      }

      this.updateAntPosition();
    },
    updateAntPosition() {
      const currentCell = this.antCurrentPosition.split("_");
      const columnNumber = parseInt(currentCell[0]);
      const cellNumber = parseInt(currentCell[1]);

      switch (this.antDirection) {
        case 0:
          const column0 = (columnNumber - 1).toString();
          const number0 = cellNumber.toString();
          this.checkCellExists(`${column0}_${number0}`);
          this.antCurrentPosition = `${column0}_${number0}`;

          break;
        case 1:
          const column1 = columnNumber.toString();
          const number1 = (cellNumber + 1).toString();
          this.checkCellExists(`${column1}_${number1}`);
          this.antCurrentPosition = `${column1}_${number1}`;

          break;
        case 2:
          const column2 = (columnNumber + 1).toString();
          const number2 = cellNumber.toString();
          this.checkCellExists(`${column2}_${number2}`);
          this.antCurrentPosition = `${column2}_${number2}`;

          break;
        case 3:
          const column3 = columnNumber.toString();
          const number3 = (cellNumber - 1).toString();
          this.checkCellExists(`${column3}_${number3}`);
          this.antCurrentPosition = `${column3}_${number3}`;

          break;
      }
    },
    checkCellExists(cellCordinate) {
      if (!this.$refs[cellCordinate]) {
        this.gridProportionFromCenter++;
      }
    },
  },
  computed: {
    rowsAndColumnsNumbers() {
      const size = this.gridProportionFromCenter;
      let arr = [];
      for (let i = -size; i <= size; i++) {
        arr.push(i);
      }
      return arr;
    },
  },
  mounted() {
    this.antCurrentPosition = this.antStartingPosition;
  },
};
</script>

<style>
.intro {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
h1 {
  text-align: center;
}
.start-btn {
  display: flex;
  justify-content: center;
  margin: 1rem;
}

.start-btn > * {
  margin-right: 0.5rem;
  margin-left: 0.5rem;
}

h3 {
  text-align: center;
  margin: 0.5rem;
}
.grid-container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.rules-text {
  margin-bottom: 0;
}

.cell {
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 20px;
  position: relative;
}

.antInSquare {
  background: red;
}

.squareBlack {
  background: black;
}

.squareWhite {
  background: white;
}
#squareRed {
  background: red;
}
</style>
