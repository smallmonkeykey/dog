<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";
import VueKinesis from "vue-kinesis";
import { KinesisContainer, KinesisElement} from "vue-kinesis"

const count = ref(0);
const dogImages = ref([]);
const mainNumber = ref();
let sec = ref(5);
const score = ref(0);
const r = ref();
const display = ref(false)


const add = () => {
  count.value += 1;
  getMainNumber();
  meetDog();
  countStart()
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
      display.value = true;
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

<div class="top" event="move">
<kinesis-container>
  <kinesis-element 
   :strength="600"
   axis="x">
    <img src="public/y0593.png" id="topDogImage">
  </kinesis-element>
<h2>お題に出された犬を見つけだそう</h2>
<h1>百犬一首</h1>
 
 </kinesis-container>
</div>

  <div id="overlay" v-if="display">
    <div id="modal">
      <p>あなたのスコアは</p>
      <p>{{ score }}</p>
      <p><button>TOPへ戻る</button></p>
    </div>
  </div>

  <p class="timer">
    残り<span>{{ sec }}</span
    >秒
  </p>
  <p class="score">スコア  {{ score }}</p>

  <p><img :src="dogImages[mainNumber]"></p>
  <button type="button" @click="add">ゲームスタート</button>
  <!-- <button type="button" @click="add">犬に出会った数 {{ count }}</button> -->
  <div id="main">
    <li v-for="dog in dogImages" id="main-img" @click="judgement">
      <img :src="dog" :style="getRandomRotate() + getRandomPosition()">
    </li>
  </div>
</template>

<style scoped>

.top {
  background-color: antiquewhite;
}
.h2 {
  letter-spacing: 10px;
  margin-bottom: 10px;
}

h1 {
  text-shadow: 1px 1px 2px  #69b792 ;
  font-size: 50px;
  color:#00a78d;
  letter-spacing: 10px;
}

#topDogImage {
  width: 30%;
  height: 20%;
}

header {
   width: 100%;
   background-color: #FFFFCC;
   position: fixed;
   top: 0;
   padding: 3px;
}

img {
  width: 80px;
  height: 120px;
  border-radius: 4px;
  position: relative;
}

#main {
  width: 90%;
  height: 500px;
  /* background-color: pink; */
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

#overlay{
  z-index:1;
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  background-color:rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

#modal{
  z-index:2;
  height: 80%;
  width: 70%;
  padding: 1px;
  background:#fff;
}
</style>
