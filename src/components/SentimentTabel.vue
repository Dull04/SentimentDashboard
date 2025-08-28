<script setup>
import { computed } from "vue";
import jsonData from "../assets/response.json";

const channels = ["news", "instagram", "youtube", "facebook", "tiktok", "twitter"];

const tableData = computed(() => {
  return channels.map(channel => {
    const channelData = jsonData.data[channel];
    const pos = channelData.pie.series[0];
    const net = channelData.pie.series[1];
    const neg = channelData.pie.series[2];
    const total = channelData.total;

    return {
      channel,
      positive: pos,
      neutral: net,
      negative: neg,
      total,
      percentPositive: ((pos / total) * 100).toFixed(1),
      percentNeutral: ((net / total) * 100).toFixed(1),
      percentNegative: ((neg / total) * 100).toFixed(1),
    };
  });
});
</script>

<template>
  <div class="container-fluid">
    <div class="table-responsive rounded shadow-sm">
      <table class="table table-striped table-bordered table-hover mb-0">
        <thead class="table-dark">
          <tr>
            <th>Channel</th>
            <th>Positive</th>
            <th>Neutral</th>
            <th>Negative</th>
            <th>Total</th>
            <th>% Positive</th>
            <th>% Neutral</th>
            <th>% Negative</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="row in tableData" :key="row.channel">
            <td>{{ row.channel }}</td>
            <td>{{ row.positive }}</td>
            <td>{{ row.neutral }}</td>
            <td>{{ row.negative }}</td>
            <td>{{ row.total }}</td>
            <td>{{ row.percentPositive }}%</td>
            <td>{{ row.percentNeutral }}%</td>
            <td>{{ row.percentNegative }}%</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
