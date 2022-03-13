<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";

const count = ref(0);
const dogImages = ref([]);
const mainNumber = ref();
let sec = ref(60);

const add = () => {
  count.value += 1;
  getMainNumber();
  meetDog();
  intervalId();
};

// const countdown = () => {
// const now = new Date()
// const minuteLater = new Date(now.getFullYear(),now.getMonth(),now.getDate(),now.getHours(),now.getMinutes()+1)
// const differ = minuteLater.getTime()-now.getTime()
// sec.value = Math.floor(differ/1000)%60
// setTimeout(countdown,1000)
// console.log(now.getMinutes)
// 

const countdown = () =>{
    console.log(sec.value--);
  }

const intervalId = setInterval(() =>{
    if(sec.value > 0){
      clearInterval(intervalId) //intervalIdをclearIntervalで指定している
    }}, 1000);

let meetDog = async () => {
  await axios
    .get(`https://dog.ceo/api/breeds/image/random/50`)
    .then(function (response) {
      console.log(response.data.message);
      dogImages.value = response.data.message;
    });
};

const getMainNumber = () => {
  mainNumber.value = Math.floor(Math.random() * 50);
  console.log(mainNumber.value);
};

const getRandomRotate = () => {
  const r = Math.floor(Math.random() * 180);
  return `transform: rotate(${r}deg);`;
};

const getRandomPosition = () => {
  const n = Math.floor(Math.random() * 100);
  return `top:${n}px;`;
};
</script>

<template>
  <p class="timer">
    残り<span>{{ sec }}</span
    >秒
  </p>

  <p><img :src="dogImages[mainNumber]" /></p>
  <button type="button" @click="add">犬に出会った数 {{ count }}</button>
  <div id="main">
    <li v-for="dog in dogImages" id="main-img">
      <img :src="dog" :style="getRandomRotate() + getRandomPosition()" />
    </li>
  </div>
</template>

<style scoped>
img {
  width: 80px;
  height: 120px;
  border-radius: 4px;
  position: relative;
}

#main {
  width: 90%;
  height: 500px;
  background-color: pink;
  margin: auto;
}

#main-img {
  list-style: none;
  display: inline-block;
  margin: 0;
}

#main-img:hover {
  border: 3px solid #f56c6c;
  border-radius: 4px;
}
</style>