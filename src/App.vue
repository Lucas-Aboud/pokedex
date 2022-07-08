
// aqui rendenizo aqui a landing page
<template>
  <v-app>
    <v-conteiner>
      <v-card>
        <v-conteiner>
          <v-row>
            <v-col
              cols="3"
              v-for="pokemon in pokemons.slice(0, 12)"
              :key="pokemon.name"
            >
              <v-card>
                <v-container>
                  {{ get_id(pokemon) }}
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                      pokemon
                    )}.png`"
                    :alt="pokemon.name"
                    width="100%"
                  />
                  <h3 class="text-center">{{ pokemon.name }}</h3>
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

  // Estou extraindo o id de cada pokemon para que ele puxe a imagem de acordo
  methods: {
    get_id(pokemon) {
      return pokemon.url.split("/")[6];
    },
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
</styl>
