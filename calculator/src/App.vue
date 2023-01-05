<template>

  <div>
    <h1>VueJS Calculator</h1>    
  </div>

  <div class="calculator">
    
    <screen-component :screen-output="currentScreenValue"></screen-component>
    
    <div class="bottom-portion">

      <div class="main-buttons">
        
        <div class="btn-row len-3">
          <calc-button @calc-button-click="handleClick" :faceValue="'C'"></calc-button>
        </div>

        <div class="btn-row">
          <calc-button @calc-button-click="handleClick" :faceValue='7'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='8'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='9'></calc-button>
        </div>

        <div class="btn-row">
          <calc-button @calc-button-click="handleClick" :faceValue='4'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='5'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='6'></calc-button>
        </div>

        <div class="btn-row">
          <calc-button @calc-button-click="handleClick" :faceValue='1'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='2'></calc-button>
          <calc-button @calc-button-click="handleClick" :faceValue='3'></calc-button>
        </div>

        <div class="btn-row len-3">
          <calc-button @calc-button-click="handleClick" :faceValue='0'></calc-button>
        </div>
      </div>

      
      <div class="operators">
        <calc-button @calc-button-click="handleClick" :faceValue="'\u00F7'"></calc-button>
        <calc-button @calc-button-click="handleClick" :faceValue="'\u00D7'"></calc-button>
        <calc-button @calc-button-click="handleClick" :faceValue="'\u2212'"></calc-button>
        <calc-button @calc-button-click="handleClick" :faceValue="'\u002B'"></calc-button>
        <calc-button @calc-button-click="handleClick" :faceValue="'\u003D'"></calc-button>
      </div>

    </div>
  
  </div>

</template>

<script>

import ScreenComponent from './components/ScreenComponent.vue';
import CalcButton from './components/CalcButton.vue';

export default {
  components: {
    ScreenComponent,
    CalcButton
  },

  data() {
    return {
      currentScreenValue: '0',
      buffer: [],
      initialized: true,
      runningTotal: 0
    }
  },

  methods: {
    handleClick(faceValue) {

      // If the calculator has been cleared, then the screen should be set to zero.
      if(this.initialized == true) {
        this.initialized = false;
        this.currentScreenValue = '';
      }

      // Any facevalue that passes this check is an operator of some sort.
      if(isNaN(faceValue)) {
        switch(faceValue) {
          case "\u002B":
            this.handleOperator("+");
            break;
          case "\u2212":
            this.handleOperator("-");
            break;
          case "\u00F7":
            this.handleOperator("/");
            break;
          case "\u00D7":
            this.handleOperator("*");
            break;
          case "C":
            this.buffer = [];
            this.currentScreenValue = '0';
            this.initialized = true;
            console.log('cleared');
            break;
          case "\u003D":
            this.evaluate()
            break;
        }
      } else {
        // This is a numeric value so add it to the running total.
        this.currentScreenValue += faceValue;
      }

      // Anything that did not pass the above check IS A NUMBER.

      // This is to check the state of the buffer after each button click.
      console.log(this.buffer);
    },

    handleOperator(faceValue) {
      // This function is to minimize repeated code for arithmetic operations.
      // Add the running number to the buffer
      this.buffer.push(this.currentScreenValue);

      // Then add the operation symbol to the buffer.
      this.buffer.push(faceValue);

      // Re-initialize the screen.
      this.currentScreenValue = '0';
      this.initialized = true;
    },

    evaluate() {

      //
      // STILL TRYING TO FIGURE OUT THE BEST WAY TO EVALUATE 
      //
      console.log('Evaluating...');

      // Push the latest screen value to the buffer
      this.buffer.push(this.currentScreenValue);

      let bufferString = '';


      for(let i = 0; i < this.buffer.length; i++) {
        bufferString += this.buffer[i];
      }

      // Convert the running total to a string to evaluate
      this.runningTotal = eval(bufferString);

      // Clear the buffer, reassign the first value of the buffer to the running total, and typecast the total back to a number.
      this.buffer = [];
      this.currentScreenValue = String(this.runningTotal);
      this.runningTotal = Number(this.runningTotal)
    }
  }
  


}
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  background-color: #4158D0;
  background-image: linear-gradient(43deg, #4158D0 0%, #C850C0 46%, #FFCC70 100%);
  background-repeat: no-repeat;
  background-size: cover;
  height: 100vh;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.calculator {
  background-color: white;
  border-radius: 10px;
  height: 60vh;
  width: 500px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
}

.btn-row {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  width: 100%;
  padding: 1rem 0.5rem;
  margin-bottom: 1rem;
  flex-grow: 3;
}

.len-3 > button {
  width: 100%;
}

.bottom-portion {
  display: flex;
  flex-direction: row;
  justify-content: center;
  margin-top: 2rem;
}

.operators {
  display: flex;
  flex-direction: column;
  gap: 3.0rem;
  padding: 1rem 0.5rem;
}
</style>
