<script setup>
import { ref, watch } from 'vue';
import { nanoid } from 'nanoid'
import Dice from './components/Dice.vue'
import ConfettiExplosion from "vue-confetti-explosion";

const dices = ref(allnewDice());
const tenzies = ref(false);
const bestScore = ref(localStorage.getItem('bestScore'))
const rollsNumber = ref(0)

function allnewDice(){
  let diceArray = []
  for(let i = 0; i < 10; i++){
    diceArray.push( generateNewDie())
  }
  return diceArray;
}

watch(dices, (newDice)=>{
  let firstValue = newDice[0].value;
  if(newDice.every(die => die.isHeld && die.value === firstValue)){
    tenzies.value = true
    if(rollsNumber.value < bestScore.value || bestScore.value == null)
      setNewBestScore(rollsNumber.value)
    console.log("You won!")
  }
})

function holdDice(id){
  let newArr = dices.value.map(prevDice => {
      return prevDice.id === id ? {...prevDice, isHeld: !prevDice.isHeld} : prevDice 
  })
  console.log(newArr)
  dices.value = newArr
}

function generateNewDie(){
  return {
    value: Math.ceil(Math.random() * 6 ),
    isHeld: false,
    id: nanoid()
  }
}

function rollDice(){
  rollsNumber.value++
  if(tenzies.value){
    dices.value = allnewDice()
    tenzies.value = false
    rollsNumber.value = 0
    
  }
  else
    dices.value = dices.value.map(dice => {
      return dice.isHeld ? 
        dice : 
        generateNewDie()
    })
}


function setNewBestScore(score){
  localStorage.setItem('bestScore', score)
  bestScore.value = score
}

console.log("MAIN FUNCTION DICES", dices.value)
</script>

<template>
  <ConfettiExplosion v-if="tenzies"/>
  <main>
    <h1 className='title'>Tenzies</h1>
    <p className='description'>Roll until all dice are the same. Click each die to freeze it at its current value between rolls.</p>
    <div className='dice-grid'>
      <Dice  
        v-for="(dice, index) in dices"
        :dice="dice"
        :index="index"
        :key="dice.id"
        @holdThisDice="holdDice(dice.id)"
      />
    </div>
    <button className='roll-btn' @click="rollDice">{{tenzies ? "New Game" : "Roll"}}</button>
  </main>
  <div className='scoreboard'>
      <h1 className='title'>Rolls number: {{rollsNumber}}</h1>
      <h1 v-if="bestScore" className='title'>Best score: {{bestScore}}</h1>
  </div>
</template>

<style>
* {
  font-family: 'Karla', sans-serif;
}

body {
  margin: 0;
  background-color: #0B2434;
}

#app {
  gap: 50px;
  padding: 50px;
  display: flex;
  justify-content: center;
}

main {
  width: 400px;
  height: 400px;  
  background-color: #F5F5F5;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
  justify-self: center;
}

.scoreboard {
  width: 400px;
  height: 150px;  
  border-radius: 10px;
  background-color: #F5F5F5;
  padding: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px
}

.dice-grid {
  display: grid;
  grid-template: auto auto / repeat(5, 1fr);
  gap: 20px;
  margin-bottom: 20px;
}

.dice {
  width: 50px;
  height: 50px;
  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.15);
  border-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.dice > h2 {
  font-family: 'Karla', sans-serif;
  font-weight: 700;
  color: #2B283A;
  font-size: 2rem;
}

.roll-btn {
  width: 100px;
  height: 50px;
  background: #5035FF;
  box-shadow: 0px 3.2px 7.68px rgba(0, 0, 0, 0.18);
  border-radius: 4px;
  color: #FFFFFF;
  font-weight: 700;
  font-size: 16px;
  border: none;
  cursor: pointer;
}

.roll-btn:active {
  box-shadow: inset 5px 5px 10px -3px rgba(0, 0, 0, 0.7);
}

.title {
  color: #2B283A;
  font-size: 40px;
  margin: 0;
}

.description {
  font-family: 'Inter', sans-serif;
  font-weight: 400;
  margin-top: 0;
  text-align: center;
}
</style>
