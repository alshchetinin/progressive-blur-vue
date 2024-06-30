<template>
  <div :class="[positionClass]" :style="containerStyle">
    <div
      style="
        display: block;
        flex: 0 0 auto;
        pointer-events: none;
        position: relative;
        overflow: visible;
        height: 100%;
        width: 100%;
      "
    >
      <div
        v-for="(layer, index) in blurLayers"
        :key="index"
        :style="getLayerStyle(layer, index)"
      ></div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  direction: {
    type: String,
    default: "top",
    validator: (value) => ["top", "bottom", "left", "right"].includes(value),
  },
  intensity: {
    type: Number,
    default: 16.67,
  },
  size: {
    type: Number,
    default: 128,
  },
  layersCount: {
    type: Number,
    default: 10,
  },
});

const isVertical = computed(() => ["top", "bottom"].includes(props.direction));

const positionClass = computed(() => {
  switch (props.direction) {
    case "top":
      return "top-0 left-0 right-0";
    case "bottom":
      return "bottom-0 left-0 right-0";
    case "left":
      return "top-0 bottom-0 left-0";
    case "right":
      return "top-0 bottom-0 right-0";
  }
});

const containerStyle = computed(() => ({
  [isVertical.value ? "height" : "width"]: `${props.size}px`,
  [isVertical.value ? "width" : "height"]: "100%",
}));

const blurLayers = computed(() => {
  const layers = [];
  for (let i = 0; i < props.layersCount; i++) {
    const percentage = i / (props.layersCount - 1);
    layers.push({
      blur: props.intensity * percentage,
      maskStart: percentage * 100,
      maskEnd: (percentage + 0.2) * 100,
    });
  }
  return layers;
});

const getGradientDirection = (direction) => {
  switch (direction) {
    case "top":
      return "to top";
    case "bottom":
      return "to bottom";
    case "left":
      return "to left";
    case "right":
      return "to right";
  }
};

const getLayerStyle = (layer, index) => {
  const gradientDirection = getGradientDirection(props.direction);

  return {
    position: "absolute",
    inset: 0,
    height: "100%",
    width: "100%",
    backdropFilter: `blur(${layer.blur}px)`,
    WebkitBackdropFilter: `blur(${layer.blur}px)`,
    maskImage: `linear-gradient(${gradientDirection}, rgba(0, 0, 0, 0) ${layer.maskStart}%, rgba(0, 0, 0, 1) ${layer.maskEnd}%)`,
    WebkitMaskImage: `linear-gradient(${gradientDirection}, rgba(0, 0, 0, 0) ${layer.maskStart}%, rgba(0, 0, 0, 1) ${layer.maskEnd}%)`,
    zIndex: index + 1,
    borderRadius: "0px",
  };
};
</script>
