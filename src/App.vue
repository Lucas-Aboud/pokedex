<template>
  <v-app>
    <v-conteiner>
      <v-conteiner>
        <v-row>
          <v-conteiner>
            <v-img
              :src="require('../src/assets/logo.png')"
              class="my-3"
              contain
              height="200"
            />
            <h1 class="text-center yellow--text mb-2" style="font-size: 6rem">
              Pokédex
            </h1>
          </v-conteiner>
        </v-row>

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
            <v-card v-on:click="show_dialog = !show_dialog" width="700">
              <v-container>
                <!-- {{ get_id(pokemon) }} -->
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

    <!-- <v-dialog v-model="show_dialog" width="700">
      <v-card v-if="selected_pokemon">
        <v-container>
        {{selected_pokemon}}
        </v-container>
      </v-card>
    </v-dialog> -->
    <div class="text-center">
      <v-dialog v-model="show_dialog" width="500">
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Privacy Policy
          </v-card-title>

          <v-card-text >
            
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do
            eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim
            ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut
            aliquip ex ea commodo consequat. Duis aute irure dolor in
            reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla
            pariatur. Excepteur sint occaecat cupidatat non proident, sunt in
            culpa qui officia deserunt mollit anim id est laborum.
          </v-card-text>

          <v-divider></v-divider>

          <v-card-actions>
            <v-spacer></v-spacer>
            <!-- <v-btn
            color="primary"
            text
            @click="dialog = false"
          >
            I accept
          </v-btn> -->
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </v-app>
</template>



<script>
// Importo a biblioteca axios para consumir a API
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      pokemons: [],
      search: "",
      show_dialog: false,
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

  // Aqui vou selecionar uma cor de acordo com o tiopo do pokemon
  //   color_selected(tipo_pokemon) {
  //     if (tipo_pokemon === elemento)
  //     color('red')
  //   }

  // Estou extraindo o id de cada pokemon para puxar a imagem de acordo e colocando a primeira letra em Uppercasez'
  methods: {
    get_id(pokemon) {
      return pokemon.url.split("/")[6];
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },

    show_pokemon(id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id} `).then((response) => {
        this.selected_pokemon = response.data;
      });
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
</style>
