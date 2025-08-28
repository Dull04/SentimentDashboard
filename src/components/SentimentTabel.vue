<script setup>
import { ref, computed } from "vue";
import jsonData from "../assets/response.json";
const searchQuery = ref("");
const sortKey = ref("");
const sortOrder = ref("asc");

const channels = ["news", "instagram", "youtube", "facebook", "tiktok", "twitter"];

const props = defineProps({
  selectedChannel: { type: String, default: "all" },
});

const tableData = computed(() => {
  let rows = channels.map(channel => {
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

  if (props.selectedChannel !== "all") {
    rows = rows.filter(row => row.channel === props.selectedChannel);
  }

  if (searchQuery.value) {
    rows = rows.filter(item =>
      item.channel.toLowerCase().includes(searchQuery.value.toLowerCase())
    );
  }

  if (sortKey.value) {
    rows.sort((a, b) => {
      let valA = a[sortKey.value];
      let valB = b[sortKey.value];
      valA = isNaN(valA) ? valA : Number(valA);
      valB = isNaN(valB) ? valB : Number(valB);

      if (valA < valB) return sortOrder.value === "asc" ? -1 : 1;
      if (valA > valB) return sortOrder.value === "asc" ? 1 : -1;
      return 0;
    });
  }

  return rows;
});

const setSort = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = sortOrder.value === "asc" ? "desc" : "asc";
  } else {
    sortKey.value = key;
    sortOrder.value = "asc";
  }
};
</script>

<template>
    <div class="mb-2 ms-3 mt-5 subTitle">
        Sentiment Tabel
    </div>
  <div class="container-fluid">
    <input
      v-model="searchQuery"
      class="form-control mb-3 w-50"
      placeholder="Search channel..."
    />
    <div class="table-responsive rounded shadow-sm">
      <table class="table table-striped table-bordered table-hover mb-0">
        <thead class="table-dark">
          <tr>
            <th>Channel</th>
            <th>
              Positive
              <i
                :class="[ sortKey === 'positive' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('positive')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              Neutral
              <i
                :class="[ sortKey === 'neutral' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('neutral')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              Negative
              <i
                :class="[ sortKey === 'negative' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('negative')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              Total
              <i
                :class="[ sortKey === 'total' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('total')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              % Positive
              <i
                :class="[ sortKey === 'percentPositive' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('percentPositive')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              % Neutral
              <i
                :class="[ sortKey === 'percentNeutral' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('percentNeutral')" style="cursor: pointer;"
              ></i>
            </th>
            <th>
              % Negative
              <i
                :class="[ sortKey === 'percentNegative' ? (sortOrder === 'asc' ? 'bi bi-arrow-up' : 'bi bi-arrow-down') : 'bi bi-arrow-down-up']"
                @click="setSort('percentNegative')" style="cursor: pointer;"
              ></i>
            </th>
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
