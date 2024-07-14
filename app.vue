<script setup>
import { onMounted } from 'vue';

    let sell = []



  const qwe = () =>{

    for (let i = 0; i < 15; i++){
      sell.push(i)
    }
    console.log(sell.length)
  }
  onMounted(qwe)
</script>

<template>
  <div class="container">
    <div v-for="item in sell" class="grid-row">
        <div v-for="cell in item" class="grid-cell">

        </div>
    </div>
  </div>
</template>

<style scoped>
  .container{
    max-width: 1080px;
    margin: 0 auto;
  }
  .grid-cell {
  width: 50px;
  height: 50px;
  border: 1px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
}
.grid-row{
  display: flex;
}
</style>
