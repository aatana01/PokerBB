<template>
  <div class="poker-table">
    <div class="control-panel">
      <h1>Poker Chip Calculator</h1>
      <div class="input-group">
        <label for="startingChips">Starting Chips:</label>
        <input id="startingChips" v-model.number="startingChips" type="number">
      </div>
      <div class="input-group">
        <label for="bigBlind">Big Blind:</label>
        <input id="bigBlind" v-model.number="bigBlind" type="number">
      </div>
      <div class="input-group">
        <label for="currentChips">Current Chips:</label>
        <input id="currentChips" v-model.number="currentChips" type="number">
      </div>
      
      <div class="calculation-controls">
        <select v-model="calculationType">
          <option value="add">Add</option>
          <option value="remove">Remove</option>
        </select>
        
        <input v-model.number="calculationAmount" type="number" placeholder="Amount">
        
        <select v-model="calculationUnit">
          <option value="chips">Chips</option>
          <option value="bb">BB</option>
        </select>
        
        <button @click="performCalculation">Calculate</button>
      </div>
      
      <div class="quick-actions">
        <button @click="quickAction('add', 1, 'bb')">+1 BB</button>
        <button @click="quickAction('add', 5, 'bb')">+5 BB</button>
        <button @click="quickAction('remove', 1, 'bb')">-1 BB</button>
        <button @click="quickAction('remove', 5, 'bb')">-5 BB</button>
      </div>
    </div>
    
    <div class="bb-display">
      <span class="bb-count">{{ currentBBCount.toFixed(2) }}</span>
      <span class="bb-label">BBs</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PokerBB',
  data() {
    return {
      startingChips: 1000,
      bigBlind: 10,
      currentChips: 1000,
      calculationType: 'add',
      calculationAmount: 0,
      calculationUnit: 'chips'
    }
  },
  computed: {
    currentBBCount() {
      return this.currentChips / this.bigBlind
    }
  },
  methods: {
    performCalculation() {
      let amountInChips = this.calculationAmount;
      if (this.calculationUnit === 'bb') {
        amountInChips *= this.bigBlind;
      }
      
      if (this.calculationType === 'add') {
        this.currentChips += amountInChips;
      } else {
        this.currentChips = Math.max(0, this.currentChips - amountInChips);
      }
    },
    quickAction(type, amount, unit) {
      this.calculationType = type;
      this.calculationAmount = amount;
      this.calculationUnit = unit;
      this.performCalculation();
    }
  }
}
</script>

<style scoped>
.poker-table {
  background-image: url('@/assets/poker-table.jpg'); /* Replace with your image path */
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px;
  box-sizing: border-box;
}

.control-panel {
  background: rgba(0, 0, 0, 0.7);
  padding: 20px;
  border-radius: 10px;
  color: white;
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
}

.input-group, .calculation-controls {
  margin-bottom: 15px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input, select, button {
  width: 100%;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  border: none;
  font-size: 16px;
}

.quick-actions {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.bb-display {
  background: rgba(0, 0, 0, 0.7);
  padding: 15px;
  border-radius: 10px;
  color: white;
  font-weight: bold;
  text-align: center;
  width: 100%;
  max-width: 200px;
  margin: 20px auto 0;
}

.bb-count {
  font-size: 36px;
  display: block;
}

.bb-label {
  font-size: 24px;
}

@media (min-width: 768px) {
  .poker-table {
    flex-direction: row;
    align-items: center;
  }

  .control-panel {
    margin: 0;
  }

  .bb-display {
    margin: 0;
  }
}
</style>