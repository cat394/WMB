<template>
  <div :class="['relative', 'border', 'rounded', containerClass]">
    <div :class="['block', 'absolute', 'top-0', titlePositionClass, sizeClass, 'translate-y-[-50%]', 'bg-[var(--dark-bg)]', 'z-1']">
      <b class="px-2">{{ title }}</b>
    </div>
    <slot />
  </div>
</template>

<script>
export default {
  name: 'ChartContainer',
  props: {
    title: {
      type: String,
      required: true,
    },
    titlePosition: {
      type: String,
      default: 'left',
    },
    titleSize: {
      type: String,
      default: 'medium',
      validator(value) {
        return ['small', 'medium', 'large'].includes(value);
      },
    },
    containerClass: {
      type: String,
      default: '',
    },
  },
  computed: {
    titlePositionClass() {
      let positionClass = '';
      switch (this.titlePosition) {
        case 'center':
          positionClass = 'left-[50%] translate-x-[-50%]';
          break;
        case 'right':
          positionClass = 'right-0 translate-x-0';
          break;
        case 'left':
          positionClass = 'left-[-10px] my-shadow';
          break;
      }
      return positionClass;
    },
    sizeClass() {
      switch (this.titleSize) {
        case 'large':
          return 'text-xl';
        case 'small':
          return 'text-sm';
        case 'medium':
        default:
          return 'text-base';
      }
    }
  }
};
</script>

<style scoped>
.my-shadow {
  box-shadow: -9px 8px 0 0px var(--dark-bg);
}
</style>