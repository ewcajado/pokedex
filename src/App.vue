<template>
  <div id="app">
    <div class="pokemons">
      <div class="pokemons__input-group">
        <input id="search" class="pokemons__input-group__search" type="text" v-model="busca">
        <label for="search" class="pokemons__input-group__label">Buscar</label>
      </div>
      <div class="pokemons__pokemon" v-for="(poke,index) in resultadoBusca" :key="index">
        <Pokemon :name="poke.name" :img="poke.img" :id="poke.id"/>
      </div>
    </div>
    <Loading :showLoad="loading"/>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
import Loading from './components/Loading';

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: '',
      loading: true
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(results => {      
      results.data.results.forEach(element => {
        axios.get(element.url).then(res => {
          this.pokemons.push({name: res.data.name, img: res.data.sprites.front_default, id: res.data.id});
          this.pokemons = this.pokemons.sort((a,b) => (a.id > b.id) ? 1 : ((b.id > a.id) ? -1 : 0));
        })
      });
      setTimeout(() => {
        this.loading = false;
      }, 500);
    })
  },
  components:{
    Pokemon,
    Loading,
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => {
          return pokemon.name.toLowerCase().startsWith(this.busca.toLowerCase());
        });
      }

    }
  }
}
</script>

<style lang="scss">
/* Reset css */
body {
  margin: 0;
  
}

.pokemons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  background-color: rgb(224, 224, 224);
  padding: 30px;
  box-sizing: border-box;
  justify-content: center;
  width: 100%;

  &__input-group {
    position: relative;
    margin-bottom: 10px;
    width: 90%;

    &__label {
      position: absolute;
      pointer-events: none;
      left: 10px;
      top: 10px;
      transition: 0.2s ease all;
      color: rgb(77, 77, 77);
    }
    &__search {
      background-position: -100em 0;
      background-repeat: no-repeat;
      background-size: 100% 100%;
      background: linear-gradient(to bottom, rgba(255, 255, 255, 0) 96%, #50505050 4%);
      border: 0;
      border-bottom: 1px solid #50505050;
      display: block;
      line-height: 1.5em;
      padding: 0.75em 1em;
      transition: 0.3s cubic-bezier(.64,.09,.08,1) all;
      box-sizing: border-box;
      width: 100%;

      &:focus {
        background-position: 0 0;
        ~ label {
          color: rgb(77, 77, 77);
          font-size: 0.875em;
          top: -1.5em;
        }
      }
      &:focus {
          outline: none;
      }
    }
  }


}
</style>
