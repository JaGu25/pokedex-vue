<template>
  <div class="flex justify-center items-center m-4">
    <div class="relative flex bg-gray-200 px-1 rounded-sm shadow-2xl border-2 border-gray-400">
      <svg
        version="1.1"
        viewBox="0 0 512.005 512.005"
        class="fill-current text-gray-600 w-4"
        style="enable-background:new 0 0 512.005 512.005;"
        xml:space="preserve"
      >
        <g>
          <g>
            <path
              d="M505.749,475.587l-145.6-145.6c28.203-34.837,45.184-79.104,45.184-127.317c0-111.744-90.923-202.667-202.667-202.667
			S0,90.925,0,202.669s90.923,202.667,202.667,202.667c48.213,0,92.48-16.981,127.317-45.184l145.6,145.6
			c4.16,4.16,9.621,6.251,15.083,6.251s10.923-2.091,15.083-6.251C514.091,497.411,514.091,483.928,505.749,475.587z
			 M202.667,362.669c-88.235,0-160-71.765-160-160s71.765-160,160-160s160,71.765,160,160S290.901,362.669,202.667,362.669z"
            />
          </g>
        </g>
      </svg>
      <select
        name="cars"
        id="cars"
        class="outline-none bg-gray-200 text-gray-700 py-1 px-2 shadow-2xl rounded-sm border-gray-600"
        v-model="type"
        @change="updateList"
      >
        <option :value="object.name" v-for="(object,index) in types" :key="index">{{object.name}}</option>
      </select>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  computed: {},
  data() {
    return {
      types: [],
      type: "normal",
    };
  },
  methods: {
    updateList() {
        console.log("update")
      this.$emit('update', this.type);
    },
  },
  created: async function () {
    try {
      const res = await axios.get("https://pokeapi.co/api/v2/type");
      this.types = res.data.results;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>


