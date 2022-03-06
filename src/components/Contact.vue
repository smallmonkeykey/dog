<script setup>
import { ref, onUpdated, onMounted } from "vue"
import axios from "axios"

const count = ref(0)
const dogImages = ref()

const add = () => {
  count.value += 1
  meetDog()
}

let meetDog = async () => {await axios.get(
    `https://dog.ceo/api/breeds/image/random`
  )
  .then(function (response) {
    console.log(response.data)
    dogImages.value = response.data.message
  })
}


</script>

<template>
  <button type="button" @click="add">犬に出会った数 {{ count }}</button>
  <P><img :src="dogImages"></P>
</template>