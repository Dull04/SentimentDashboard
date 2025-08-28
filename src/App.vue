<script setup>
import DropdownFilter from "./components/DropdownFilter.vue";
import ChannelSummary from "./components/ChannelSummary.vue";
import SentimentTabel from "./components/SentimentTabel.vue";
import { ref } from "vue";
const selectedChannel = ref("all");

</script>
<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <div class="row flex-grow-0 mb-2">
      <div class="col font ms-3 mt-3">Sentiment Dashboard</div>
      <div class="col">Bulet2Bulet2Bulet2...et2Bulet2...et2Bulet2...</div>
    </div>
    <div class="row flex-grow-0">
      <div class="col-6 d-flex flex-column">
        <div class="d-flex align-items-center mb-2">
          <DropdownFilter v-model:selectedChannel="selectedChannel" />
        </div>
        <ChannelSummary :selectedChannel="selectedChannel" />
        <div class="mt-2">
          <SentimentTabel />
        </div>
      </div>
      <div class="col-6">
        chart
      </div>
    </div>
  </div>
</template>
