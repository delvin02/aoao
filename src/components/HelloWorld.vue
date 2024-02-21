<script setup lang="ts">
import { Input } from '@/components/ui/input'
import { Button } from '@/components/ui/button'

</script>

<template>
  <div>
    <div v-if="eligible">
      <h1> {{ highestSum }}</h1>
    </div>
    <div v-else>
      <h1>gg</h1>
    </div>
    <div class="grid grid-cols-6 mx-auto gap-1 mt-3">
        <Input v-model="input1" @focus="clearInput('input1')" class="col-start-2 col-span-2 mb-2"/>
        <Input v-model="input2" @focus="clearInput('input2')" class="col-span-2"/>
    </div>
    <div class="grid grid-cols-3 gap-1">
      <Input v-model="input3" @focus="clearInput('input3')"/>
      <Input v-model="input4" @focus="clearInput('input4')"/>
      <Input v-model="input5" @focus="clearInput('input5')"/>
    </div>
    <Button @click=calculate() class="mt-3">Check</Button>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

<script lang="ts">
export default {
  data() {
    return {
      input1: 0,
      input2: 0,
      input3: 0,
      input4: 0,
      input5: 0,
      eligible: false,
      highestSum: 0,
      combinations: [],
    };
  },
  methods: {
    calculate() {

      const inputs = [this.input1, this.input2, this.input3, this.input4, this.input5].map(Number);

      this.combinations = [];
      this.highestSum = 0;

      const targetSum = 10;
      let allNumCombos = this.generateAllCombinations(inputs);
      
      allNumCombos.forEach(comboSet => {
        // Now comboSet is an array of numbers from the original with 3 and 6 swapped accordingly
        for (let i = 0; i < comboSet.length; i++) {
          for (let j = i + 1; j < comboSet.length; j++) {
            for (let k = j + 1; k < comboSet.length; k++) {
              if ((comboSet[i] + comboSet[j] + comboSet[k]) % targetSum === 0) {
                this.eligible = true;
                this.combinations.push([comboSet[i], comboSet[j], comboSet[k]]);

                let remainingNums = comboSet.filter((_, idx) => idx !== i && idx !== j && idx !== k);

                let highestOfRemaining = (remainingNums.reduce((accumulator, currentValue) => accumulator + currentValue, 0))% 10;
                console.log(highestOfRemaining);
                if (highestOfRemaining > this.highestSum) {
                  this.highestSum = highestOfRemaining;
                }
              }
            }
          }
        }
      });

      console.log("Combinations:", this.combinations, "Highest Remaining:", this.highestSum);

    },
    generateAllCombinations(nums) {
      let c = [nums];


      for (let i = 0; i < nums.length; i++)
      {
        if (nums[i] === 3) {
          c.push(6);
        }
        else if (nums[i] === 6) {
          c.push(3);
        }
      }

      return c;
    },
    clearInput(inputName) {
      this[inputName] = null;
    }
  }
};

</script>
