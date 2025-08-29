<script setup>
import { ref, onMounted } from "vue";
import response from "./assets/response.json";
import DropdownFilter from "./components/DropdownFilter.vue";
import ChannelSummary from "./components/ChannelSummary.vue";
import SentimentTabel from "./components/SentimentTabel.vue";
import PieChart from "./components/DoughnutChart.vue";
import BarChart from "./components/BarColumnChart.vue"; 

const selectedChannel = ref("all");

const isValidResponse = response.meta?.status === 1 && response.meta?.code === 200;
const errorMessage = response.meta?.message || "Unknown error";
const showPopup = ref(false);
const popupMessage = ref("");
const popupType = ref("success"); // "success" | "error"

onMounted(() => {
  if (isValidResponse) {
    popupMessage.value = "✅ Data berhasil dimuat";
    popupType.value = "success";
    console.log("✅ Data loaded successfully:", response.meta.message);
  } else {
    popupMessage.value = `❌ Gagal memuat data: ${errorMessage}`;
    popupType.value = "error";
    console.error("❌ Error loading data:", errorMessage);
  }
  showPopup.value = true;
  setTimeout(() => {
    showPopup.value = false;
  }, 2000);
});
</script>

<template>
  <div class="container-fluid min-vh-100 d-flex flex-column">
    <transition name="slide-fade">
      <div
        v-if="showPopup"
        class="popup-box"
        :class="popupType"
      >
        {{ popupMessage }}
      </div>
    </transition>

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

      <div class="col-12 col-md-6 d-flex flex-column">
  <div class="card shadow-sm p-3 d-flex align-items-center justify-content-center mt-3" style="flex:1">
    <PieChart :selectedChannel="selectedChannel" />
  </div>

  <div class="card shadow-sm p-3 mt-3 mb-3 d-flex align-items-center justify-content-center" style="flex:1">
    <BarChart 
      :selectedChannel="selectedChannel" 
      :responseData="response.data" 
    />
  </div>
</div>
    </div>
  </div>
</template>
