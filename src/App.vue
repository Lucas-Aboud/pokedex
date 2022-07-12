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
            <v-card v-on:click="toggle_modal(pokemon)" width="700">
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

    <div class="text-center">
      <v-dialog v-model="show_dialog" width="500">
        <v-card>
          <img
            class="img-card"
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
              modal_pokemon
            )}.png`"
          />
          <v-card>
            <h2 class="text-center">{{ get_name(modal_pokemon) }}</h2>
          </v-card>

          <v-card-text v-if="modal_pokemon" class="card-text">
            <p>Tipo: {{ modal_pokemon.type }}</p>
          </v-card-text>

          <v-divider></v-divider>
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
      modal_pokemon: null,
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

  // Estou extraindo o id de cada pokemon para puxar a imagem de acordo e colocando a primeira letra em Uppercasez'
  methods: {
    get_id(pokemon) {
      if (pokemon && pokemon.url) return pokemon.url.split("/")[6];
    },
    get_name(pokemon) {
      if (pokemon && pokemon.name)
        return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },

    toggle_modal(pokemon) {
      this.modal_pokemon = pokemon;

      axios
        .get(`https://pokeapi.co/api/v2/pokemon/${pokemon.name}`)
        .then((response) => {
          this.modal_pokemon.type = response.data.types[0].type.name;
          this.modal_pokemon.type_url = response.data.types[0].type.url;
          this.show_dialog = true;
        });
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

.card-text {
  text-align: center;
  color: #0000;
  font-size: 24px;
  font-weight: bold;
  margin-top: 16px;
}

.img-card {
  width: 80%;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
