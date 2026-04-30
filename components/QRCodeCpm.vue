<template>
  <qrcode-vue
    :value="value"
    :size="size"
    :margin="margin"
    :level="level"
    :render-as="renderAs"
    :background="background"
    :foreground="foreground"
    :gradient="gradient"
    :gradient-type="gradientType"
    :gradient-start-color="gradientStartColor"
    :gradient-end-color="gradientEndColor"
    :image-settings="imageSettings"
  />
</template>

<script setup lang="ts">
  import { ref, computed } from "vue";
  import QrcodeVue from "qrcode.vue";
  import type { Level, RenderAs, GradientType, ImageSettings } from "qrcode.vue";

  const props = defineProps({ toastUrl: String });
  const value:any = computed(() => {
    return props.toastUrl;
  })
  const size:number = 150;
  const margin:number = 1;
  const level = ref<Level>("M");
  const renderAs = ref<RenderAs>("svg");
  const background = ref("#ffffff");
  const foreground = ref("#000000");

  const imageSettings = ref<ImageSettings>({
    src: "https://cdn.shopify.com/s/files/1/0581/8872/6331/files/logo_512x512_06066816-6f2a-4ae6-8c6b-3c7f15f29000.png?v=1663293229",
    width: 26,
    height: 26,
    excavate: true,
  });

  const gradient = ref(false);
  const gradientType = ref<GradientType>("linear");
  const gradientStartColor = ref("#000000");
  const gradientEndColor = ref("#38bdf8");
</script>

<style lang="scss" scoped>
  svg {
    border-radius: 4px;
  }
</style>
