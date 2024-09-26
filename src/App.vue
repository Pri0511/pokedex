<template>
  <div class="contenedor">
    <div class="parte1">
      <label id="pide1" for="numero">PokeSearch:</label>
      <input id="pide2" type="text" v-model="numero" placeholder="Ex: Charizard or 6" @input="listarPokemones">

    </div>
    <div class="parte2">
      <!-- <button @click="listarPokemones()">Traer</button>
    <h1>{{ nombre }}</h1>
    <img :src="image" alt="">-->
      <div class="contenedorparte2">
        <div class="cont2_1">
          <div class="nom">
            <h1 :class="{ 'not-found': nombre === 'Pokémon no encontrado' }">
              {{ nombre !== 'Pokémon no encontrado' ? nombre : 'Pokémon no encontrado' }}
            </h1>
          </div>
          <div class="num">
            <h1>{{ num }}</h1>
          </div>
        </div>
        <div class="cont2_2">


          <div class="cara1" :class="{ 'default-image': nombre === 'Pokémon no encontrado' }"
            :style="{ backgroundColor: tipos.length > 0 ? typeColors[tipos[0]] : 'rgba(224, 217, 217, 0.363)' }">

            <img :src="image" alt="">
          </div>
          <div class="cara2">
            <div class="tipo1">Tipo:</div>
            <div class="tipo2">
              <button v-for="tipo in tipos" :key="tipo" class="btn-tipo" :style="{ backgroundColor: typeColors[tipo] }">
                {{ tipo }}</button>
            </div>
          </div>
          <div class="cara3">
            <div class="debilidades1">Debilidad:</div>
            <div class="debilidades2">
              <button v-for="debilidad in debilidades" :key="debilidad" class="btn-debilidad"
                :style="{ backgroundColor: weaknessColors[debilidad] }">{{ debilidad }}</button>
            </div>
          </div>

        </div>
        <div class="cont2_3">

          <div class="column1">
            <div class="fila1">Base</div>
            <div class="fila2">HP:</div>
            <div class="fila3">Attack:</div>
            <div class="fila4">Defense:</div>
            <div class="fila5">Sp.Attack:</div>
            <div class="fila6">Sp.Defense:</div>
            <div class="fila7">Speed:</div>
          </div>
          <div class="column2">
            <div class="fila8">Stats</div>
            <div class="fila9">{{ hp }}</div>
            <div class="fila10">{{ Attack }}</div>
            <div class="fila11">{{ Defense }}</div>
            <div class="fila12">{{ SpAttack }}</div>
            <div class="fila13">{{ SpDefense }}</div>
            <div class="fila14">{{ Speed }}</div>
          </div>


        </div>

      </div>
    </div>

  </div>
</template>

<script setup>

const typeColors = {
  normal: '#A8A878',
  fire: '#F08030',
  water: '#6890F0',
  electric: '#F8D030',
  grass: '#78C850',
  ice: '#98D8D8',
  fighting: '#C03028',
  poison: '#A040A0',
  ground: '#E0C068',
  flying: '#A890F0',
  psychic: '#F85888',
  bug: '#A8B820',
  rock: '#B8A038',
  ghost: '#705898',
  dragon: '#7038F8',
  dark: '#705848',
  steel: '#B8B8D0',
  fairy: '#EE99AC'
};

const weaknessColors = {
  normal: '#A8A878',
  fire: '#F08030',
  water: '#6890F0',
  electric: '#F8D030',
  grass: '#78C850',
  ice: '#98D8D8',
  fighting: '#C03028',
  poison: '#A040A0',
  ground: '#E0C068',
  flying: '#A890F0',
  psychic: '#F85888',
  bug: '#A8B820',
  rock: '#B8A038',
  ghost: '#705898',
  dragon: '#7038F8',
  dark: '#705848',
  steel: '#B8B8D0',
  fairy: '#EE99AC'
};

import axios from "axios"
import { ref } from "vue";

let tipos = ref([]);
let debilidades = ref([]);

let nombre = ref("");
let image = ref("");
let num = ref("");
let numero = ref("");
let hp = ref("");
let Attack = ref("");
let Defense = ref("");
let SpAttack = ref("");
let SpDefense = ref("");
let Speed = ref("");

async function listarPokemones() {
  if (numero.value.trim() === "") {

    nombre.value = "";
    image.value = "";
    num.value = "";
    hp.value = "";
    Attack.value = "";
    Defense.value = "";
    SpAttack.value = "";
    SpDefense.value = "";
    Speed.value = "";
    tipos.value = [];
    debilidades.value = [];
    return;
  }

  try {
    let url = `https://pokeapi.co/api/v2/pokemon/${numero.value.toLowerCase()}`;
    let { data } = await axios.get(url);

    nombre.value = data.name;
    image.value = data.sprites.front_default;
    num.value = data.id;
    hp.value = data.stats[0].base_stat;
    Attack.value = data.stats[1].base_stat;
    Defense.value = data.stats[2].base_stat;
    SpAttack.value = data.stats[3].base_stat;
    SpDefense.value = data.stats[4].base_stat;
    Speed.value = data.stats[5].base_stat;
    tipos.value = data.types.map(tipo => tipo.type.name);

    debilidades.value = [];
    for (let tipo of tipos.value) {
      let tipoData = await axios.get(`https://pokeapi.co/api/v2/type/${tipo}`);
      tipoData.data.damage_relations.double_damage_from.forEach(deb => {
        if (!debilidades.value.includes(deb.name)) {
          debilidades.value.push(deb.name);
        }
      });
    }
  } catch (error) {

    console.error(error);
    nombre.value = "Pokémon no encontrado";
    image.value = "https://bcdn.lanetaneta.com/wp-content/uploads/2022/02/Pokemon-como-jugar-a-Nuzlocke-Run-desafio-personalizado.jpg";
    num.value = "";
    hp.value = "";
    Attack.value = "";
    Defense.value = "";
    SpAttack.value = "";
    SpDefense.value = "";
    Speed.value = "";
    tipos.value = [];
    debilidades.value = [];
  }
}



</script>

<style>
body {
  height: 100%;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url(https://i.pinimg.com/736x/55/4b/57/554b572b7b1a9b88f0dcbc4c48a8b989.jpg);
  background-size: 100%;
  background-position: center;
  background-repeat: no-repeat;
  
}

.contenedor {
  position: relative;
  width: 450px;
  height: 600px;
  margin-top: -14px;
  margin-bottom: -18px;
  margin-left: -50px;
  display: grid;
  grid-template-rows: 4fr, 1fr;



}

.btn-tipo,
.btn-debilidad {

  max-height: 20px;
  width: 55px;
  margin: 1px;

  padding: 0px 2px;

  background-color: #ccc;

  color: rgb(255, 255, 255);
  max-width: 50px;
  font-size: 0.8rem;
}




div.cara1.default-image img {
  width: 100%;
  height: 100%;
  background-size: 20px;
  margin-top: 0px;
  border-radius: 10px;
}




h1.not-found{
  text-align: left;
  font-size: 0.7rem;
  margin-left: 25%;
  margin-top: 1px;
  color: rgb(49, 49, 49);
}

.contenedorparte2 {
  position: relative;
  width: 320px;
  height: 480px;
  margin-top: 14px;
  margin-bottom: -18px;
  margin-left: 65px;

  display: grid;
  grid-template-rows: 10%, 40%, 50%;
}

.cont2_1 {
  width: 320px;
  height: 40px;
  border: 1px solid #dfd9d9;
  color: rgb(0, 0, 0);
  display: grid;
  grid-template-columns: 1fr 1fr;
  background-color: rgba(224, 217, 217, 0.363);
}

.cont2_2 {
  display: grid;
  grid-template-rows: 60% 15% 25%;
}

.cara1 img {
  margin-top: -10px;
  width: 40%;
  height: auto;
  object-fit: contain;
}



.cara2,
.cara3 {
  font-size: 1rem;
  margin-left: 0%;
  color: rgb(49, 49, 49);
  border: 1px solid #dfd9d9;
  display: grid;
  grid-template-columns: 30% 70%;
  background-color: rgba(224, 217, 217, 0.363);
}

.nom {
  width: 160px;
  height: 40px;
  border: 1px solid #dfd9d9;
}

.nom h1 {

  text-align: left;
  font-size: 0.9rem;
  margin-left: 25%;
  color: rgb(49, 49, 49);
}

.num {
  width: 157px;
  height: 40px;
  border: 1px solid #dfd9d9;

}

.num h1 {
  text-align: left;
  font-size: 1rem;
  margin-left: 40%;
  color: rgb(49, 49, 49);
}

.cont2_2 {
  position: relative;
  width: 320px;
  height: 200px;
  border: 1px solid #dfd9d9;
  color: rgb(0, 0, 0);
  background-color: rgba(224, 217, 217, 0.363);
}

.cont2_3 {
  position: relative;
  width: 320px;
  height: 240px;
  border: 1px solid #dfd9d9;
  color: rgb(0, 0, 0);
  display: grid;
  grid-template-columns: 1fr 1fr;
}

.parte1 {
  position: relative;
  width: 450px;
  height: 80px;
  background-color: rgba(232, 51, 42, 0.8);
  border: 1px solid #dfd9d9;
  color: rgb(0, 0, 0);

  border-top-left-radius: 15px;

  border-top-right-radius: 15px;

}

.parte2 {
  position: relative;
  width: 450px;
  height: 520px;
  border: 1px solid #dfd9d9;
  background-color: rgba(169, 169, 169, 0.7);
  color: black;

  border-bottom-left-radius: 15px;

  border-bottom-right-radius: 15px;
}

#pide2 {
  margin-left: 4px;
  width: 150px;
  height: 30px;
  border: 1px solid rgb(216, 212, 212);
  background-color: aliceblue;
  color: black;
}

#pide2::placeholder {
  text-indent: 4px;
}

#pide1,
#pide2 {
  margin-top: 25px;
}

.column1 {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;

}

.column2 {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr 1fr 1fr 1fr 1fr;

}

.fila1,
.fila2,
.fila3,
.fila4,
.fila5,
.fila6,
.fila7,
.fila8,
.fila9,
.fila10,
.fila11,
.fila12,
.fila13,
.fila14 {
  font-size: 1rem;
  margin-left: 0%;
  color: rgb(49, 49, 49);
  border: 1px solid #dfd9d9;
  background-color: rgba(224, 217, 217, 0.363);
}



@media (max-width: 900px) {
.contenedor {
  position: relative;
  width: 350px;
  height: 300px;
  margin-top: -200px;
  margin-bottom: -18px;
  margin-left: -50px;
  display: grid;
  grid-template-rows: 4fr, 1fr;

}

body {
  height: 100%;  
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: none;  
  background-size: 100%;
  background-position: center;
  background-repeat: no-repeat;
}

}
</style>
