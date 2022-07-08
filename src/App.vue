
// aqui rendenizo aqui a landing page
<template>
  <v-app>
    <v-conteiner>
      <v-card>
        <v-conteiner>
          <v-row>
            <v-col cols="2" v-for="pokemon in pokemons" :key="pokemon.name">
              <v-card>
                <v-container> 
                  <h3>{{pokemon.name}}</h3>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-conteiner>
      </v-card>
    </v-conteiner>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      pokemons: [],
    };
  },
  //  Aqui eu chamo os 151 pokemons inicais que quero apresentar na tela
  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },
};
</script>

<style>
#app {
  background: linear-gradient(
      to bottom right,
      rgba(10, 10, 10, 1),
      rgba(12, 39, 63, 1)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  background-position: center;
  min-height: 100vh;
}
</style>
