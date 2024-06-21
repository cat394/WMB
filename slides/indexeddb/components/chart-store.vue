<template>
  <table id="store">
    <thead>
      <tr>
        <th class="title">{{ leftTitle }}</th>
        <th class="title">{{ rightTitle }}</th>
      </tr>
    </thead>
    <tbody>
      <tr class="text-center" v-for="key in keys">
        <td>{{ key }}</td>
        <td>{...}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'ChartStore',
  props: {
    leftTitle: {
      type: String,
      default: 'Key',
    },
    rightTitle: {
      type: String,
      default: 'Value',
    },
    keys: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
  #store {
    border: 1px solid currentColor;

    & tr {
      border: none;
    }

    & th, td {
      padding-block: 10px;
      border: 1px solid currentColor;

    }

    & th {
      text-align: center;
      color: currentColor;
    }
  }
</style>