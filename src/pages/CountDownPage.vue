<template>
  <q-page padding>
    <h5>Countdown</h5>

    <div class="text-center">
      <span class="digit">{{ parseInt(remain_time / 1000 / 60 / 60) }} </span>
      <span class="seperator"> : </span>
      <span class="digit">{{ parseInt((remain_time / 1000 / 60) % 60) }} </span>
      <span class="seperator"> : </span>
      <span class="digit">{{ parseInt(remain_time / 1000) % 60 }} </span>
    </div>
    <div class="text-center">elapsed: {{ elapsed / 1000 }}</div>
    <div class="text-center">target_time: {{ target_time / 1000 }}</div>
    <div class="text-center">remain_time: {{ remain_time / 1000 }}</div>

    <div class="text-center q-mt-md">
      <q-btn label="시작" color="secondary" @click="onClickStart()" />
    </div>
  </q-page>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const hour = ref(2);
const min = ref(30);
const sec = ref(20);
const remain_time = ref(0);

let interval_id;

onMounted(() => {
  target_time.value = (hour.value * 3600 + min.value * 60 + sec.value) * 1000;
  remain_time.value = target_time.value;
});
onUnmounted(() => {
  if (interval_id != null) clearInterval(interval_id);
});

let started;
const elapsed = ref(0);
const target_time = ref(0);
const onClickStart = () => {
  target_time.value = (hour.value * 3600 + min.value * 60 + sec.value) * 1000;
  started = new Date().getTime();
  if (interval_id == null)
    interval_id = setInterval(() => {
      elapsed.value = new Date().getTime() - started;
      remain_time.value = target_time.value - elapsed.value;
    }, 20);
};
</script>

<style>
.digit {
  border: 1px solid red;
  margin: 3px;
  padding: 3px;
  background-color: orange;
  font-size: 36px;
}
.seperator {
  font-size: 36px;
}
</style>
