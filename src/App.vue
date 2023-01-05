<script setup>
import Wheel from "./components/Wheel.vue";
import Slices from "./components/Slices.vue";
import Slider from "./components/Slider.vue";
import { data as options } from "./data";

import "./styles.css";

const options2 = options
  .map((o) => o.options || [{ title: "..." }, { title: "..." }])
  .flat();

const options3 = options
  .map((o) =>
    (o.options || []).map(
      (o2) => o2.options || [{ title: "..." }, { title: "..." }]
    )
  )
  .flat(Infinity);

const selection = $ref({});
const onSelect = (sector) =>
  (selection[sector.title] = (selection[sector.title] || 0) + 1);

const rotation = $ref(0);
const showMessage = $ref(true);
</script>

<template>
  <div>
    <div class="grid grid-rows-[1fr_auto] h-screen h-screen-ios">
      <div class="flex items-center justify-center overflow-hidden">
        <Wheel
          class="w-[180vw] md:w-[60vw] -ml-[90vw] md:ml-0"
          :size="500"
          :style="{
            transform: 'rotate(' + (360 - rotation) + 'deg)',
          }"
        >
          <Slices
            @select="onSelect"
            :selection="selection"
            :options="options"
            :inner="0"
            :outer="80"
          />
          <Slices
            @select="onSelect"
            :selection="selection"
            :options="options2"
            :inner="80"
            :outer="160"
          />
          <Slices
            @select="onSelect"
            :selection="selection"
            :options="options3"
            :inner="160"
            :outer="240"
          />
        </Wheel>
      </div>
      <button
        class="fixed top-8 right-8 text-xl font-bold duration-1000 transition"
        :class="[showMessage ? 'opacity-[0.01] -translate-y-4' : 'opacity-30']"
        @click="showMessage = !showMessage"
      >
        ◯
      </button>
      <div
        @click="showMessage = false"
        class="duration-1000 transition fixed top-8 left-8 right-8 md:w-1/3 text-4xl font-bold opacity-70"
        :class="[
          showMessage
            ? 'opacity-80'
            : 'pointer-events-none opacity-0 -translate-y-8',
        ]"
      >
        Su tunded on sinu omad ja kõik on väärt mäletamist. Märka ja märgi
        kuidas sa ennast tunned.
      </div>
    </div>
    <div class="fixed right-4 bottom-12 left-4 flex justify-center">
      <Slider type="range" v-model="rotation" max="360" />
    </div>
  </div>
</template>
