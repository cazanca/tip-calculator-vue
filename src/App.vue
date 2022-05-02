<script setup>
import { ref } from "@vue/reactivity";
 let bill = ref('')
    let tip = ref('')
    let numberOfPeople = ref('')

    let tipPerPerson = ref(0)
    let totalPerPerson = ref(0)

    const updateTip = (value) => {
      tip.value = value
      if (bill.value && numberOfPeople.value) {
        result()
      }
    }

    const activeTip = (value) => {
      return tip.value == value ? 'active' : ''
    }

    const hasError = () => {
      return !numberOfPeople.value ? 'has-error' : ''
    }

    const tipAmount = () => {
       if (numberOfPeople.value) {
          tipPerPerson.value = (tip.value / 100 * bill.value) / numberOfPeople.value
          tipPerPerson.value = tipPerPerson.value.toFixed(2)
        }
    }

    const total = () => {
       if (numberOfPeople.value) {
      totalPerPerson.value = (bill.value + (tip.value / 100 * bill.value)) / numberOfPeople.value
      totalPerPerson.value = totalPerPerson.value.toFixed(2)
  }
    }

    const result = () => {
      tipAmount()
      total()
    }

    const reset = () => {
      tip.value = ''
      bill.value = ''
      totalPerPerson.value = '0'
      tipPerPerson.value = '0'
      numberOfPeople.value = ''
    }

/*export default {
  setup(){
   
    return {
      bill,
      tip,
      numberOfPeople,
      tipPerPerson,
      totalPerPerson,
      updateTip,
      tipAmount,
      activeTip,
      hasError,
      total,
      result,
      reset
    }
  }
}*/
</script>

<template>
    <h1 class="title">
      <span>SPLI</span>
      <span>TTER</span>
    </h1>

    <div class="card">
      <form @submit.prevent="result()">
        <div class="container">
          <div class="group">
            <label for="bill">Bill</label>
            <div class="input-group">
              <span>$</span>
              <input type="number" id="bill" v-model="bill" placeholder="0.00">
            </div>
          </div>
          <div class="group">
            <label for="tip">Select Tip %</label>
            <div class="tip" id="tip">
              <button type="button" @click="updateTip(5)" :class="activeTip(5)">5%</button>
              <button type="button" @click="updateTip(10)" :class="activeTip(10)">10%</button>
              <button type="button" @click="updateTip(15)" :class="activeTip(15)">15%</button>
              <button type="button" @click="updateTip(25)" :class="activeTip(25)">25%</button>
              <button type="button" @click="updateTip(50)" :class="activeTip(50)">50%</button>
              <input type="number" @keyup="updateTip($event.target.value)" placeholder="Custom">
            </div>
          </div>
           <div class="group">
            <label for="people" class="flex"><span>Number of people</span><span v-if="!numberOfPeople">Can't be zero</span></label>
            <div class="input-group">
              <span><img src="./assets/images/icon-person.svg" alt="icon person"></span>
              <input type="number" v-model="numberOfPeople" id="people" @keyup="result()" @change="result()" min="0" :class="hasError()" placeholder="0">
            </div>
          </div>
        </div>
        <div class="result">
            <div class="result__details">
              <p>Tip Amount <br> <span>/ person</span></p>
              <p>${{tipPerPerson}}</p>
            </div>
            <div class="result__details">
              <p>Total <br> <span>/ person</span></p>
              <p>${{totalPerPerson}}</p>
            </div>
            <button type="reset" @click="reset()">reset</button>
          </div>
      </form>
    </div>
</template>

