<template>
  <div>
    <div class="image">
      <a class="arrow left" @click="imgBack"></a>
      <img :src="currentImg" :alt="name + ' image'">
      <a class="arrow right" @click="imgFront"></a>
    </div>
    <div class="content">
      <p class="name">{{name | upper}}</p>
      <p class="type">{{pokemon.type | upper}}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      isFront: true,
      currentImg: '',
      pokemon: {
        type: '',
        front: '',
        back: '',
      },
    }
  },
  created: function() {
    axios.get(this.url).then(response => {
      this.pokemon.type = response.data.types[0].type.name;
      this.pokemon.front = response.data.sprites.front_default;
      this.pokemon.back = response.data.sprites.back_default;
      this.currentImg = this.pokemon.front;
    }).catch(error => {
      console.log(error);
    });
  },
  props: {
    num: Number,
    name: String,
    url: String,
  },
  filters: {
    upper: function(value) {
      return value[0].toUpperCase() + value.slice(1);
    }
  },
  methods: {
    imgFront: function() {
      if(this.isFront) {
        this.currentImg = this.pokemon.back;
        this.isFront = false;
      }
    },
    imgBack: function() {
      if(!this.isFront) {
        this.currentImg = this.pokemon.front;
        this.isFront = true;
      }
    }
  }
}
</script>

<style>
  .image {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .image img {
    width: 70%;
  }

  .arrow {
    border: solid #999;
    border-width: 0 3px 3px 0;
    display: inline-block;
    padding: 5px;
    cursor: pointer;
    transition: 200ms;
  }

  .arrow:hover {
    border-color: #2a75bb;
  }

  .right {
    transform: rotate(-45deg);
    -webkit-transform: rotate(-45deg);
  }

  .left {
    transform: rotate(135deg);
    -webkit-transform: rotate(135deg);
  }

  .name {
    font-family: Arial, Helvetica, sans-serif;
    font-weight: bold;
    font-size: 18px;
  }

  .type {
    font-family: Arial, Helvetica, sans-serif;
    font-size: 15px;
    margin-bottom: 10px;
  }
</style>