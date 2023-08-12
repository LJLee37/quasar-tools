<template>
  <q-page padding>
    <q-list bordered seperator>
      <q-item>
        <q-item-section class="text-bold">StopWatch</q-item-section>
      </q-item>
      <q-item v-for="(time, timeIndex) in lapTimes" :key="timeIndex">
        <q-item-section avatar>
          <q-chip color="red-3">{{ timeIndex + 1 }}</q-chip>
        </q-item-section>
        <q-item-section>
          {{ time }}
        </q-item-section>
      </q-item>
      <q-item>
        <q-item-section
          ><div class="text-h6">elapsed: {{ elapsed }}</div></q-item-section
        >
      </q-item>
    </q-list>

    <q-btn label="시작" color="blue" @click="startOnClick()" />
    <q-btn label="멈춤" color="orange" @click="stopOnClick()" />
    <q-btn label="랩타임" color="green" @click="laptimeOnClick()" />
  </q-page>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
const abc = 0;
const elapsed = ref(0);
let started;
let interval = null;

const lapTimes = ref([]);
onMounted(() => {
  console.log('StopWatch onMounted');
});

onUnmounted(() => {
  stopOnClick();
});

const startOnClick = () => {
  elapsed.value = 0;
  started = new Date().getTime();
  if (interval == null) {
    interval = setInterval(() => {
      elapsed.value = (new Date().getTime() - started) / 1000;
    }, 10);
  }
};
const stopOnClick = () => {
  if (interval != null) clearInterval(interval);
  interval = null;
};
const laptimeOnClick = () => {
  if (interval == null) return;
  lapTimes.value.push(elapsed.value);
};
</script>
