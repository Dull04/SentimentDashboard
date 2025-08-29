<script setup>
import { ref, defineEmits } from "vue";

const emits = defineEmits(["update:selectedChannel"]);

// daftar channel
const channels = ["all", "news", "instagram", "youtube", "facebook", "tiktok", "twitter"];

// 🔥 default langsung "Semua Channel"
const selected = ref("all"); 

const handleChange = () => {
  emits("update:selectedChannel", selected.value);
};

// emit value pertama kali load (biar App.vue langsung sync)
handleChange();
</script>

<template>
  <div class="d-flex align-items-center mb-2 w-100">
    <div class="d-flex align-items-center">
      <label for="channelSelect" class="me-2 ms-3">Filter Channel</label>
      <select
        id="channelSelect"
        class="form-select w-auto"
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
    <div class="ms-auto me-5">
      Welcome, User
    </div>
  </div>
</template>
