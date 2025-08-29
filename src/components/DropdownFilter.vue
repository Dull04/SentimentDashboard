<script setup>
import { ref, defineEmits } from "vue";
const emits = defineEmits(["update:selectedChannel"]);

const channels = ["all", "news", "instagram", "youtube", "facebook", "tiktok", "twitter"];
const selected = ref("all");

const handleChange = () => {
  emits("update:selectedChannel", selected.value);
};

handleChange();
</script>

<template>
  <div class="row w-100 g-3 px-3">
    <div class="col-12 col-md-6">
      <div class="card shadow-sm p-3">
        <select
          id="channelSelect"
          class="form-select"
          v-model="selected"
          @change="handleChange"
        >
          <option
            v-for="channel in channels"
            :key="channel"
            :value="channel"
          >
            {{ channel === "all" ? "Semua Channel" : channel.charAt(0).toUpperCase() + channel.slice(1) }}
          </option>
        </select>
      </div>
    </div>
    <div class="col-12 col-md-6">
      <div class="card shadow-sm p-3 mt1setengah d-flex align-items-center justify-content-center welcome-card">
        <span class="fw-bold fs-5 text-white">Welcome, User 👋</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.welcome-card {
  background: linear-gradient(135deg, #6fb1fc, #4364f7); /* light blue → deep blue */
  border: none;
  color: white;
}
</style>
