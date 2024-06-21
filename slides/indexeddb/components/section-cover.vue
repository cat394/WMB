<template>
  <h1 class="text-white">IndexedDB - Part <span :style="`color: ${getSectionColor()};`">{{ partNumber }}</span></h1>
  <p>{{ description }}</p>
</template>

<script setup>
  const props = defineProps({
    partNumber: Number,
    description: String
  });

  const Colors = {
    red: 'oklch(0.65 0.3 29.62)',
    blue: 'oklch(0.5 0.3 262.64)',
    green: 'oklch(0.85 0.36 144.85)',
    yellow: 'oklch(0.92 0.22 101.57)',
    purple: 'oklch(0.57 0.31 301.16)',
    pink: 'oklch(0.67 0.3 0.66)',
  };

  const colorArray = [
    Colors.red,
    Colors.blue,
    Colors.green,
    Colors.yellow,
    Colors.purple,
    Colors.pink
  ];

  function getSectionColor() {
    return colorArray[(props.partNumber - 1) % colorArray.length];
  } 
</script>
