<script setup lang="ts">
import { SpeedInsights } from "@vercel/speed-insights/vue"
import { useDialog, useLoadingBar, useMessage, useThemeVars } from 'naive-ui'
window.$message = useMessage()
window.$loading = useLoadingBar()
window.$dialog = useDialog()

import { useStyleTag } from "@vueuse/core"
import { computed } from "vue"
const cssVars = useThemeVars()
const injectStyle = computed(() => {
  console.log(cssVars.value)
  let css = '*{\n'
  for (const key in cssVars.value) {
    const styleValue = cssVars.value[key as keyof typeof cssVars.value]
    const styleKey = `--nn-${key
      .replace(/([a-z0-9])([A-Z])/g, '$1-$2')
      .replace(/([A-Z])([A-Z][a-z])/g, '$1-$2')
      .replace(/([a-zA-Z])([0-9])/g, '$1-$2')
      .replace(/([0-9])([a-zA-Z])/g, '$1-$2')
      .toLowerCase()}`
    css += `${styleKey}: ${styleValue};\n`
  }
  css += '}'
  return css
})
useStyleTag(injectStyle)
</script>

<template>
  <SpeedInsights />
  <Suspense>
    <RouterView v-slot="{ Component, route }" :key="$route.fullPath">
      <div class="h-full w-full" :key="route.fullPath">
        <component :is="Component" />
      </div>
    </RouterView>
  </Suspense>
</template>