<template>
  <div>
    <Search @update="getPokemons" />
    <Loader v-if="loading" />
    <div v-if="!loading" class="w-full h-32 grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-3">
      <div
        :class="[`border-${getColor(pokemon.types[0].type.name)}-400`]"
        class="flex justify-between bg-white rounded-sm shadow-2xl relative border-b-4 w-full"
        v-for="(pokemon,index) in ordersPokemon"
        :key="index"
      >
        <div class="flex flex-col justify-between py-4 px-4 items-start">
          <p class="text-xl text-blue-600 font-bold">{{firstLetterUppercase(pokemon.name)}}</p>
          <div>
            <Type
              v-for="(type,index) in pokemon.types"
              :key="index"
              :color="getColor(type.type.name)"
              :texto="firstLetterUppercase(type.type.name)"
            />
          </div>
        </div>
        <img class="w-5/12 py-4" :src="pokemon.img.front_default" alt="Not Image Found" />
        <span
          class="text-sm absolute top-0 right-0 py-1 px-2 text-gray-400 font-bold"
        >#{{pokemon.id}}</span>
      </div>
    </div>
  </div>
</template>


<script>
import Type from "@/components/Type";
import Loader from "@/components/Loader";
import axios from "axios";

import Search from "@/components/Search";

export default {
  components: {
    Type,
    Loader,
    Search,
  },
  data() {
    return {
      pokemons: [],
      loading: true,
      typesColors: {
        grass: "green",
        fire: "orange",
        water: "blue",
        poison: "purple",
        bug: "yellow",
        flying: "teal",
      },
      color: "red",
      type: "normal",
    };
  },
  computed: {
    ordersPokemon: function () {
      this.pokemons.sort((a, b) => {
        return a.id - b.id;
      });
      return this.pokemons || [];
    },
  },
  methods: {
    firstLetterUppercase(value) {
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
    getColor(value) {
      let color = null;
      Object.keys(this.typesColors).map((element) => {
        if (value === element) {
          color = this.typesColors[element];
        }
      });
      return color || "gray";
    },
    async getPokemons(type) {
      this.loading = true;
      this.pokemons = [];
      try {
        const res = await axios.get(`https://pokeapi.co/api/v2/type/${type}/`);
        res.data.pokemon.forEach(async ({ pokemon }) => {
          const res = await axios.get(pokemon.url);
          const img = res.data.sprites.other["official-artwork"];
          if (img.front_default) {
            this.pokemons.push({
              id: res.data.order,
              name: res.data.name,
              img: res.data.sprites.other["official-artwork"],
              types: res.data.types,
            });
          }
        });
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
  },
  created: async function () {
    this.getPokemons("normal");
  },
};
</script>