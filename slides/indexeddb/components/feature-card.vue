<template>
  <div class="card float-from-bottom">
    <div class="card-icon">
      <slot />
    </div>
    <h2 class="card-title">{{ title }}</h2>
    <p class="card-description">{{ description }}</p>
  </div>
</template>

<script>
export default {
  name: 'FeatureCard',
  props: {
    title: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.card {
  text-align: center;
  box-shadow: 5px 5px 10px 0px rgb(60, 60, 60);
  border-radius: 8px;
  padding: 1rem;
  max-width: 300px;
}

.card-icon {
  display: flex;
  justify-content: center;

  & > * {
    font-size: 2rem;
  }
}

.card-title {
  font-size: 1.5em;
  letter-spacing: 0.2em;
  margin: 0.5rem 0;
}

.card-description {
  color: #858383;
  text-wrap: balance;
}
</style>
