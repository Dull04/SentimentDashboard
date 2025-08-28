<script setup>
import { computed } from "vue";
import jsonData from "../assets/response.json";

const props = defineProps({
  selectedChannel: { type: String, default: "" }, 
});

const channels = ["news", "instagram", "youtube", "facebook", "tiktok", "twitter"];

const summary = computed(() => {
  if (!props.selectedChannel || props.selectedChannel === "all") {
    return {
      positive: "-",
      neutral: "-",
      negative: "-",
      info: "Please select a channel to see the sentiment summary",
    };
  }

  const data = jsonData.data[props.selectedChannel];
  return {
    positive: data.pie.series[0],
    neutral: data.pie.series[1],
    negative: data.pie.series[2],
    info: "",
  };
});
</script>

<template>
  <div class="d-flex align-items-center mb-2 ms-3">
    <div class="me-3" :title="summary.info">Positive: {{ summary.positive }}</div>
    <div class="me-3" :title="summary.info">Neutral: {{ summary.neutral }}</div>
    <div class="me-3" :title="summary.info">Negative: {{ summary.negative }}</div>
  </div>

  <div v-if="summary.info" class="text-muted small ms-3 mb-3">
    {{ summary.info }}
  </div>
</template>
