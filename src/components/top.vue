<script setup>
import { ref, onUpdated, onMounted, reactive } from "vue";
import axios from "axios"
import VueKinesis from "vue-kinesis";
import { KinesisContainer, KinesisElement} from "vue-kinesis"
import Main from './main-dog.vue'

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
<button @click="add">実験</button>
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
</style>