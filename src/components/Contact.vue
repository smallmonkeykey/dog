<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios";

const count = ref(0)
const dogImages = ref([])

const add = () => {
  count.value += 1
  meetDog()
};

let meetDog = async () => {
  await axios
    .get(`https://dog.ceo/api/breeds/image/random/3`)
    .then(function (response) {
      console.log(response.data.message);
      dogImages.value = response.data.message;
    });
};


</script>

<template>
<P><img :src="dogImages[0]"></P>
  <button type="button" @click="add">犬に出会った数 {{ count }}</button>
  <li v-for="dog in dogImages" id="main-img">
    <img :src="dog" alt="">
  </li>
</template>

<style>
img {
  width: 80px;
  height: 120px;
  border-radius: 4px;
}

#main-img{
  list-style: none;
}
</style>