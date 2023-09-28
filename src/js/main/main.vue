<script setup lang="ts">
import { onMounted, ref } from "vue";
import { os, path } from "../lib/cep/node";
import {
  subscribeBackgroundColor,
} from "../lib/utils/bolt";
import "../index.scss";
import { csi } from "../lib/utils/bolt";

// @ts-ignore - Since HostAdapter isn't explicitly typed, linting throws an error we'll ignore
import { AIEventAdapter, AIEvent } from "../../public/BoltHostAdapter.js";
// ^ Keep in mind this is a modified EventAdapter file including a short export {} line at bottom

const backgroundColor = ref("#282c34");

onMounted(() => {
  if (window.cep) {
    subscribeBackgroundColor((c: string) => (backgroundColor.value = c));
  }
  const adapter = AIEventAdapter.getInstance()
  adapter.addEventListener(
    AIEvent.ART_SELECTION_CHANGED,
    async (e: any) => {
      console.log("Host Adapter triggered:")
      console.log(e)
    }

  );
  console.log(AIEventAdapter)
});
</script>

<template>
  <div class="app" :style="{ backgroundColor: backgroundColor }">
    <header class="app-header">
      <img src="../assets/bolt-cep.svg" class="icon" />
    </header>
  </div>
</template>
