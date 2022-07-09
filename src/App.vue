// aqui rendenizo aqui a landing page
<template>
  <v-app>
    <v-conteiner>
      <v-conteiner>
        <v-text-field
          v-model="search"
          label="Busca"
          placeholder="Procure seu Pokémon aqui pelo nome"
          solo
        ></v-text-field>
        <v-row>
          <v-col
            cols="3"
            v-for="pokemon in pokemons_filtrados"
            :key="pokemon.name"
          >
            <v-card @click="show_dialog = !show_dialog">
              <v-container>
                {{ get_id(pokemon) }}
                <v-row class="mx-0 dflex justify-center">
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                      pokemon
                    )}.png`"
                    :alt="pokemon.name"
                    width="80%"
                  />
                </v-row>
                <h3 class="text-center">{{ get_name(pokemon) }}</h3>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-conteiner>
    </v-conteiner>

    <v-dialog v-model="show_dialog" width="500">
      <v-card>
       <v-container>
        
       </v-container>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      pokemons: [],
      search: "",
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
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
  },
  computed: {
    pokemons_filtrados() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
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
