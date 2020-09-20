<template>
  <div class="home font-body">
    <PokemonList/>
  </div>
</template>

<script>
import PokemonList from '@/components/PokemonList'

export default {
  name: 'Home',
  components: {
    PokemonList,
  }
}
</script>
