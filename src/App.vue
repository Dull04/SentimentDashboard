<script setup>
import DropdownFilter from "./components/DropdownFilter.vue";
import ChannelSummary from "./components/ChannelSummary.vue";
import SentimentTabel from "./components/SentimentTabel.vue";

import { ref } from "vue";

const selectedChannel = ref("all");
</script>

<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <!-- Header -->
    <div class="row flex-grow-0">
      <div class="col font ms-3 mt-3">Sentiment Dashboard</div>
      <div class="col">Bulet2</div>
    </div>

    <!-- Dropdown + Welcome -->
    <div class="row flex-grow-0 mb-2 ms-1">
      <div class="col d-flex align-items-center justify-content-between">
          <DropdownFilter v-model:selectedChannel="selectedChannel" />
      </div>
    </div>

    <!-- Ringkasan kanal terpilih -->
    <ChannelSummary :selectedChannel="selectedChannel" />

    <!-- Tabel + Chart -->
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
