<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";
import VueKinesis from "vue-kinesis";
import { KinesisContainer, KinesisElement } from "vue-kinesis";

const count = ref(0);
const dogImages = ref([]);
const mainNumber = ref();
let sec = ref(30);
const score = ref(0);
const r = ref();
const display = ref(false);
const displayTop = ref(true);
const displayGame = ref(false);
const displaybtn = ref(true);

const gameStart = () => {
  displayTop.value = false;
  displayGame.value = true;
  displaybtn.value = false;
  count.value += 1;
  getMainNumber();
  meetDog();
  countStart();
};

let meetDog = async () => {
  await axios.get(`https://dog.ceo/api/breeds/image/random/50`).then(function (response) {
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

const judgement = (e) => {
  console.log(e.target.src);
  console.log(dogImages.value[mainNumber.value]);
  if (e.target.src === dogImages.value[mainNumber.value]) {
    score.value++;
    getMainNumber();
    meetDog();
  }
};

const reloadPage = () => {
  window.location.reload();
};
</script>

<template>
  <div class="top" event="move" v-show="displayTop">
    <kinesis-container>
      <kinesis-element :strength="600" axis="x">
        <img src="../assets/y0593.png" id="topDogImage" />
      </kinesis-element>
      <h2>お題に出された犬を見つけだそう</h2>
      <h1>百犬一首</h1>
    </kinesis-container>
  </div>

  <div id="overlay" v-if="display">
    <div id="modal">
      <img src="../assets/g0429.png" />
      <p>あなたのスコアは</p>
      <p>{{ score }}</p>
      <p><button @click="reloadPage" class="btn">TOPへ戻る</button></p>
    </div>
  </div>

  <button type="button" @click="gameStart" v-if="displaybtn" class="gameStartBtn">
    GAME START
  </button>

  <div class="game" v-if="displayGame">
    <div class="box">
      <img src="../assets/dogfoot.png" class="foot" />
      <img src="../assets/dogfoot.png" class="foot foot1" />
      <p class="timer">{{ sec }}秒<span>残り</span></p>
      <p><img :src="dogImages[mainNumber]" /></p>
      <p class="score">{{ score }} SCORE</p>
      <img src="../assets/dogfoot.png" class="foot foot1" />
      <img src="../assets/dogfoot.png" class="foot" />
    </div>
    <div id="main">
      <li v-for="dog in dogImages" id="main-img" @click="judgement">
        <img :src="dog" :style="getRandomRotate() + getRandomPosition()" />
      </li>
    </div>
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
  text-shadow: 1px 1px 2px #69b792;
  font-size: 50px;
  color: #00a78d;
  letter-spacing: 10px;
}

#topDogImage {
  width: 30%;
  height: 20%;
}

header {
  width: 100%;
  background-color: #ffffcc;
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
  opacity: 0.3;
  color: white;
}

#overlay {
  z-index: 1;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

#modal {
  z-index: 2;
  height: 80%;
  width: 70%;
  padding: 1px;
  background: #fff;
}

.gameStartBtn {
  background: rgba(160, 238, 199, 0.5);
  border-radius: 30px;
  font-size: 30px;
  color: #00a78d;
  padding: 10px 30px;
  border-color: #fff;
  border-bottom: 3px solid #a9b6af;
  position: relative;
  transition-property: opacity;
  transition-duration: 0.5s;
}
.gameStartBtn:hover {
  opacity: 0.7;
  transform: translate(0, -10px);
  box-shadow: 5px 5px 3px 0 rgba(12, 12, 12, 0.2);
}

.timer {
  width: 50px;
  height: 50px;
  line-height: 50px;
  background-color: white;
  border-radius: 50%;
  font-size: 20px;
  color: rgb(78, 85, 82);
}
.score {
  width: 50px;
  height: 50px;
  line-height: 50px;
  background-color: white;
  border-radius: 50%;
  font-size: 20px;
  color: rgb(78, 85, 82);
}

.box {
  display: flex;
  justify-content: space-between;
}

#modal {
  font-size: 50px;
}

.btn {
  background: rgba(160, 238, 199, 0.5);
  border-radius: 30px;
  font-size: 30px;
  color: #00a78d;
  padding: 10px 30px;
  border-color: #fff;
  border-bottom: 3px solid #a9b6af;
  position: relative;
  transition-property: opacity;
  transition-duration: 0.5s;
}

@media screen and (max-width: 540px) {
  #topDogImage {
    width: 80%;
    height: 70%;
  }

  .foot1 {
    display: none;
  }

  #modal p {
    font-size: 40px;
  }
}
</style>
