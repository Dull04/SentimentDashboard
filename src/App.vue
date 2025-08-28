<script setup>
import DropdownFilter from "./components/DropdownFilter.vue";
import ChannelSummary from "./components/ChannelSummary.vue";
import SentimentTabel from "./components/SentimentTabel.vue";
import PieChart from "./components/DoughnutChart.vue";

import { ref } from "vue";
const selectedChannel = ref("all");
</script>

<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <div class="row flex-grow-1">
      <div class="col-12 col-md-6 d-flex flex-column">
        <div class="h4 ms-3 mt-3 mb-3">Sentiment Dashboard</div>

        <div class="d-flex align-items-center mb-2">
          <DropdownFilter v-model:selectedChannel="selectedChannel" />
        </div>

        <ChannelSummary :selectedChannel="selectedChannel" />

        <div class="mt-2 flex-grow-1">
          <SentimentTabel />
        </div>
      </div>
      <div class="col-12 col-md-6 d-flex flex-column gap-3">
        <div class="card shadow-sm p-3 flex-grow-0">
          <PieChart :selectedChannel="selectedChannel" />
        </div>

        <div class="card shadow-sm p-3 flex-grow-1 d-flex align-items-center justify-content-center">
          aaaa
        </div>
      </div>
    </div>
  </div>
</template>
