<script setup>
import HelloWorld from "./components/HelloWorld.vue";
import SentimentTabel from "./components/SentimentTabel.vue";
</script>

<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <div class="row flex-grow-0">
      <div class="col font ms-3 mt-3">
        Sentiment Dashboard
      </div>
      <div class="col">
        Bulet2
      </div>
    </div>
    <div class="row flex-grow-0">
      <div class="col">
        <SentimentTabel />
      </div>
      <div class="col">
        chart
      </div>
    </div>
  </div>
</template>
