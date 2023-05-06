<script setup>
import { ref, computed } from 'vue';
const nome = "Vue dinâmico";
const styleColor = "color: blue";
const arrayCores = ["blue", "red", "peru"];
const ativo = false;
const arrayFrutas = [
    {
        name: "Manzana",
        price: "$1.00",
        description: "Una manzana",
        stock: 0,
    },
    {
        name: "Pera",
        price: "$2.00",
        description: "Una pera",
        stock: 10,
    },
    {
        name: "Naranja",
        price: "$3.00",
        description: "Una naranja",
        stock: 20,
    },
    ];
const objetoFruta = arrayFrutas[0]

//metodo - methods
const handleClick = (texto) => { console.log(texto) }

let counter = 0;
const increment = () => {
  console.log('Aumentar contador')
  counter++
}

//Ref
let counterRef = ref(0);
const incrementRef = () => {
  console.log('Aumentar contador ref')
  counterRef.value++
}

const diminuirRef = () => {
  console.log('Diminuir contador ref')
  counterRef.value--
}

const resetarRef = () => {
  console.log('Resetar contador ref')
  counterRef.value = 0
}

//Computed
const classCounter = computed(() => {
  if(counterRef.value > 0) {
    return 'positive'
  } else if(counterRef.value < 0) {
    return 'negative'
  } else { return 'neutral' }
})

const favoritos = ref([])
const addArray = () => {
  favoritos.value.push(counterRef.value)
  console.log(favoritos.value)
}

const bloquearBtnAdd = computed(() => {
  return favoritos.value.includes(counterRef.value) ? true : false
})

</script>

<template>
  <div class="container text-center mt-3">
    <h1>Olá, {{ nome.toUpperCase() }}</h1>
  <h2 v-bind:style="styleColor">Sou azul</h2>
  <h2 :style="`color: ${arrayCores[2]}`">Sou {{ arrayCores[2] }}</h2>
  <h2>
    {{ ativo ? "Estou ativo" : "Estou inativo" }}
  </h2>
  <p v-if="ativo">Estou ativo</p>
  <p v-if="!ativo">Estou inativo</p>

  <h2 v-show="ativo">Estou ativo v-show</h2>

  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="fruta in arrayFrutas" :key="fruta.name">
      {{ fruta.name }} - {{ fruta.price }} - {{ fruta.description }}
    </li>
  </ul>

  {{ objetoFruta }}
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="(atributo, propriedade, id) in objetoFruta" :key="atributo">
      {{ id+1 }} : {{ propriedade }} - {{ atributo }}

    </li>
  </ul>

<!-- v-if tem prioridade ao v-for. Por isso, precisa colocar o v-for dentro do template para depois chamar o v-if -->
  <ul class="list-group mt-4">
    <template v-for="item in arrayFrutas" :key="item.name">
      <li class="list-group-item" v-if="item.stock > 0">
        {{ item.name }} - {{ item.price }} - {{ item.stock }}
      </li>
    </template>
  </ul>

  <h2>Testes: v-on</h2>
  <button v-on:click="handleClick('Texto 1')">Me ative 1!</button>
  <button @click="handleClick('Texto 2')">Me ative 2!</button>

  <p>Testes: v-on cliques diferentes do mouse</p>
  <button v-on:click.right.prevent="handleClick('Right click')">Right button</button>
  <button v-on:click.middle="handleClick('Middle click')">Middle button</button>
  <button v-on:click="handleClick('Left click')">Left button</button>

  <h2> Reatividade</h2>
  <h2> Contador: {{ counter }} - Não se altera (sem ref())</h2>
  <button @click="increment">Aumentar</button>

  <!-- <h2 v-bind:style="[ counterRef >= 0 ? counterRef > 0 ? 'color: green' : 'color: black' : 'color: red']"> Contador Ref: {{ counterRef }}</h2> -->
  <!-- <h2 v-bind:class="[ counterRef >= 0 ? counterRef > 0 ? 'positive' : 'neutral' : 'negative']"> Contador Ref: {{ counterRef }}</h2> -->
  <!-- Usando computed -->
  <h2 v-bind:class="classCounter"> Contador Ref: {{ counterRef }}</h2>
  <div class="btn-group">
    
  <button @click="incrementRef" class="btn btn-success">Aumentar ref</button>
  <button @click="diminuirRef" class="btn btn-danger">Diminuir ref</button>
  <button @click="resetarRef" class="btn btn-secondary">Resetar ref</button>
  <button @click="addArray"  class="btn btn-primary" :disabled="bloquearBtnAdd">Add</button>

  </div>

  <!-- <ul>
    <template v-for="numero in favoritos">
      <li >{{ numero }}</li>
    </template>
  </ul> -->

  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="(numero,index) in favoritos" :key="index">{{ numero }}</li>
  </ul>


  </div>
  

</template>

<style>
h1 {
  color: skyblue
}
.positive {
  color: green
}
.negative {
  color: red
}
.neutral {
  color: peru
}

</style>
