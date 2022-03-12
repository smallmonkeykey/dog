<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";
import drag from "@branu-jp/v-drag";

const count = ref(0)
const dogImages = ref([])
const mainNumber = ref()

const add = () => {
  count.value += 1
  getMainNumber()
  meetDog()
};

let meetDog = async () => {
  await axios
    .get(`https://dog.ceo/api/breeds/image/random/50`)
    .then(function (response) {
      console.log(response.data.message);
      dogImages.value = response.data.message;
    });
};

const getMainNumber = () => {
   mainNumber.value = Math.floor(Math.random() * 50)
  console.log(mainNumber.value)
}

const getRandomRotate = ()=> {
  const r = Math.floor(Math.random() * 180)
  return `transform: rotate(${r}deg);`
}

const getRandomPosition = ()=> {
  const n = Math.floor(Math.random() * 100)
  return `top:${n}px;`
}

</script>

<template>
  <P><img :src="dogImages[mainNumber]"></P>
  <button type="button" @click="add">犬に出会った数 {{ count }}</button>
    <div id="main">
      <li v-for="dog in dogImages" id="main-img">
        <img :src="dog" :style="getRandomRotate()+getRandomPosition()" v-drag>
      </li>
    </div>
</template>

<style scoped>
img {
  width: 80px;
  height: 120px;
  border-radius: 4px;
  position:relative;
}

#main{
  width: 90%;
  height: 500px;
  background-color: pink;
  margin: auto;
}

#main-img{
  list-style: none;
  display: inline-block;
  margin: 0;
  position: absolute;
}

#main-img:hover{
  	border:3px solid #f56c6c;
    border-radius: 4px;
}
</style>