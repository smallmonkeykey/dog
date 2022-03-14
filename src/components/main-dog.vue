<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";

const count = ref(0);
const dogImages = ref([]);
const mainNumber = ref();
let sec = ref(30);
const score = ref(0);
const r = ref();


const add = () => {
  count.value += 1;
  getMainNumber();
  meetDog();
};

let meetDog = async () => {
  await axios.get(`https://dog.ceo/api/breeds/image/random/50`).then(function (response) {
    console.log(response.data.message);
    dogImages.value = response.data.message
  });
};

const getMainNumber = () => {
  mainNumber.value = Math.floor(Math.random() * 50)
  console.log(mainNumber.value)
};

const getRandomRotate = () => {
  const r = Math.floor(Math.random() * 180);
  return `transform: rotate(${r}deg);`
};


const getRandomPosition = () => {
  const n = Math.floor(Math.random() * 100)
  return `top:${n}px;`
};

// タイマーカウント用途
const countStart = () => {
  const intervalId = setInterval(() => {
    
    decrementNumber();
    if (sec.value == 0) {
      clearInterval(intervalId);
    }
  }, 1000);
};

const decrementNumber = () => {
  sec.value--;
};

const judgement = (e)=> {
  console.log(e.target.src)  
  console.log(dogImages.value[mainNumber.value]) 
  if (e.target.src === dogImages.value[mainNumber.value]){
    score.value++
    getMainNumber();
    meetDog();
  }

}

</script>

<template>
  <p class="timer">
    残り<span>{{ sec }}</span
    >秒
  </p>
  <p class="score">スコア  {{ score }}</p>

  <p><img :src="dogImages[mainNumber]"></p>
  <button type="button" @click="countStart">ゲームスタート</button>
  <button type="button" @click="add">犬に出会った数 {{ count }}</button>
  <div id="main">
    <li v-for="dog in dogImages" id="main-img" @click="judgement">
      <img :src="dog" :style="getRandomRotate() + getRandomPosition()">
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
