<script setup>
import SentimentTabel from "./components/SentimentTabel.vue";
import DropdownFilter from "./components/DropdownFilter.vue";
import { ref } from "vue";

// State untuk kanal yang dipilih
const selectedChannel = ref("all");
</script>

<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <!-- Header -->
    <div class="row flex-grow-0">
      <div class="col font ms-3 mt-3">
        Sentiment Dashboard
      </div>
      <div class="col">
        Bulet2
      </div>
    </div>

    <!-- Dropdown Filter di atas tabel -->
    <div class="row flex-grow-0 mb-2 ms-2">
      <div class="col">
        <DropdownFilter v-model:selectedChannel="selectedChannel" />
      </div>
    </div>

    <!-- Tabel + Chart -->
    <div class="row flex-grow-0">
      <div class="col">
        <SentimentTabel/>
      </div>
      <div class="col">
        chart
      </div>
    </div>
  </div>
</template>
