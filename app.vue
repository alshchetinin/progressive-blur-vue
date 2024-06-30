<script setup lang="ts">
useSeoMeta({
  title: "Progressive Blur Component | Vue 3",
  description:
    "This Vue 3 component creates a customizable progressive blur effect that can be applied to any side of a container. It uses multiple layers with varying blur intensities to create a smooth transition from clear to blurred.",
  ogTitle: "Vue Progressive Blur Component",
  ogDescription:
    "This Vue 3 component creates a customizable progressive blur effect that can be applied to any side of a container. It uses multiple layers with varying blur intensities to create a smooth transition from clear to blurred.",
  twitterCard: "summary_large_image",
});

const { metaSymbol } = useShortcuts();
import { useClipboard } from "@vueuse/core";
const toast = useToast();

const optionDirections = ["top", "bottom", "left", "right"];
const direction = ref("top");

const intensity = ref(20);
const size = ref(228);
const layersCount = ref(10);

const textProps = computed(() => {
  return `<ProgressiveBlur class="absolute z-50" direction="${direction.value}" :intensity="${intensity.value}" :size="${size.value}" :layers-count="${layersCount.value}" />`;
});
const { text, copy, copied, isSupported } = useClipboard({ textProps });

const copyToClipboard = () => {
  copy(textProps.value);

  toast.add({
    title: "Copied props",
  });
};

defineShortcuts({
  meta_p: {
    usingInput: true,
    handler: () => {
      copyToClipboard();
    },
  },
});
</script>

<template>
  <UContainer class="flex justify-center">
    <UCard class="my-3 max-w-[600px] w-full">
      <template #header>
        <div class="flex justify-between items-center mb-4">
          <h1 class="text-3xl font-bold">Progressive Blur</h1>
          <div class="flex gap-2">
            <NuxtLink
              external
              to="https://github.com/alshchetinin/progressive-blur-vue"
            >
              <UIcon name="i-simple-icons-github" />
            </NuxtLink>
            <NuxtLink external to="https://x.com/al_schetinin">
              <UIcon name="i-simple-icons-x" />
            </NuxtLink>
          </div>
        </div>
        <div class="space-y-4">
          <URadioGroup
            :ui="{
              fieldset: 'flex gap-7',
              container: 'flex gap-4',
            }"
            v-model="direction"
            :options="optionDirections"
          />
          <div>
            <div class="flex justify-between items-center">
              <p>Intensity</p>
              <p>{{ intensity }}</p>
            </div>
            <URange size="sm" v-model="intensity" min="0" max="100" />
          </div>

          <div>
            <div class="flex justify-between items-center">
              <p>Size</p>
              <p>{{ size }}</p>
            </div>
            <URange size="sm" v-model="size" min="64" max="512" />
          </div>

          <div>
            <div class="flex justify-between items-center">
              <p>Layers count</p>
              <p>{{ layersCount }}</p>
            </div>
            <URange size="sm" v-model="layersCount" min="1" max="20" />
          </div>
          <div>
            <UButton
              @click="copyToClipboard()"
              variant="outline"
              class="flex items-center gap-2"
            >
              <small>Copy props</small>
              <div class="flex items-center gap-0.5">
                <UKbd>{{ metaSymbol }}</UKbd>
                <UKbd>P</UKbd>
              </div>
            </UButton>
          </div>
        </div>
      </template>
      <div class="relative rounded-xl overflow-hidden">
        <NuxtImg src="gk.jpg" class="w-full h-auto" />
        <ProgressiveBlur
          class="absolute z-50"
          :direction="direction"
          :intensity="intensity"
          :size="size"
          :layers-count="layersCount"
        />
      </div>
    </UCard>
  </UContainer>
  <UNotifications />
</template>
