<template>
  <div :class="$options.name">
    <form-date v-model="date"/>
  </div>
</template>

<script>
import FormDate from './FormDate.vue';

export default {
  name: `App`,
  components: {
    FormDate,
  },
  data() {
    return {
      date: '',
    };
  },
};
</script>

<style lang="scss">
@import '~normalize.css';
@import '~reset-css/_reset.scss';

* {
  box-sizing: border-box;
}

body {
  padding: 1em;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}
</style>
