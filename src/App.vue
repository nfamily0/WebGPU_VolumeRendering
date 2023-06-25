<script setup lang="ts">
import { darkTheme } from 'naive-ui'
import { NConfigProvider } from 'naive-ui'
import { NGrid, NGi } from 'naive-ui'
import { NButton } from 'naive-ui'

import vueHeader from './components/Header.vue'
import Viewport from './components/Viewport.vue'
</script>

<template>
  <n-config-provider :theme="darkTheme">
    <n-grid x-gap="12" :cols="4">
      <n-gi :span="4">
        <vueHeader />
      </n-gi>
      <n-gi :span="1" />
      <n-gi :span="2">
        <Viewport />
      </n-gi>
      <n-gi :span="1" />
    </n-grid>
  </n-config-provider>
</template>

<style scoped></style>
