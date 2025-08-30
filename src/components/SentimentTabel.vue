<script setup>
import { ref, computed } from "vue";
import jsonData from "../assets/response.json";

const searchQuery = ref("");
const sortKey = ref("");
const sortOrder = ref("asc");
const channels = [
  "news",
  "instagram",
  "youtube",
  "facebook",
  "tiktok",
  "twitter",
];

const props = defineProps({
  selectedChannel: { type: String, default: "all" },
});

const tableData = computed(() => {
  let rows = channels.map((channel) => {
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
    rows = rows.filter((row) => row.channel === props.selectedChannel);
  }

  if (searchQuery.value) {
    rows = rows.filter((item) =>
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
  <div class="card p-0 ms-3">
    <div class="card-body p-3">
      <div class="mb-2 ms-3 subTitle">Sentiment Tabel</div>
      <div class="container-fluid">
        <input
          v-model="searchQuery"
          class="form-control mb-3 w-50 keep-border"
          placeholder="Search channel..."
        />

        <div class="table-responsive">
          <table class="table align-middle mb-0 modern-table">
            <thead>
              <tr>
                <th>Channel</th>
                <th>
                  Positive
                  <i
                    :class="[
                      sortKey === 'positive'
                        ? sortOrder === 'asc'
                          ? 'bi bi-arrow-up'
                          : 'bi bi-arrow-down'
                        : 'bi bi-arrow-down-up',
                        'sort-icon'
                    ]"
                    @click="setSort('positive')"

                  ></i>
                </th>
                <th>
                  Neutral
                  <i
                    :class="[
                      sortKey === 'neutral'
                        ? sortOrder === 'asc'
                          ? 'bi bi-arrow-up'
                          : 'bi bi-arrow-down'
                        : 'bi bi-arrow-down-up',
                        'sort-icon'
                    ]"
                    @click="setSort('neutral')"
                  ></i>
                </th>
                <th>
                  Negative
                  <i
                    :class="[
                      sortKey === 'negative'
                        ? sortOrder === 'asc'
                          ? 'bi bi-arrow-up'
                          : 'bi bi-arrow-down'
                        : 'bi bi-arrow-down-up',
                        'sort-icon'
                    ]"
                    @click="setSort('negative')"
                  ></i>
                </th>
                <th>
                  Total
                  <i
                    :class="[
                      sortKey === 'total'
                        ? sortOrder === 'asc'
                          ? 'bi bi-arrow-up'
                          : 'bi bi-arrow-down'
                        : 'bi bi-arrow-down-up',
                        'sort-icon'
                    ]"
                    @click="setSort('total')"

                  ></i>
                </th>
                <th>% Positive</th>
                <th>% Neutral</th>
                <th>% Negative</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="row in tableData" :key="row.channel">
                <td class="fw-semibold">{{ row.channel }}</td>
                <td class="text-success fw-semibold">
                  {{ row.positive.toLocaleString() }}
                </td>
                <td>{{ row.neutral.toLocaleString() }}</td>
                <td class="text-danger fw-semibold">
                  {{ row.negative.toLocaleString() }}
                </td>
                <td class="fw-semibold">{{ row.total.toLocaleString() }}</td>
                <td>
                  {{ Number(row.percentPositive).toLocaleString("id-ID") }}%
                </td>
                <td>
                  {{ Number(row.percentNeutral).toLocaleString("id-ID") }}%
                </td>
                <td>
                  {{ Number(row.percentNegative).toLocaleString("id-ID") }}%
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
