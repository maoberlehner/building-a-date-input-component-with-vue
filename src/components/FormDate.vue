<template>
  <div
    class="FormDate"
    @keyup.capture="updateValue"
  >
    <input
      v-if="showDay"
      ref="day"
      v-model="day"
      class="FormDate__input FormDate__input--day"
      type="number"
      placeholder="dd"
      @input="updateDay"
      @blur="day = day.padStart(2, 0)">
    <span
      v-if="showDay && showMonth"
      class="FormDate__divider"
    >/</span>
    <input
      v-if="showMonth"
      ref="month"
      v-model="month"
      class="FormDate__input FormDate__input--month"
      type="number"
      placeholder="mm"
      @input="updateMonth"
      @blur="month = month.padStart(2, 0)">
    <span
      v-if="showYear && (showDay || showMonth)"
      class="FormDate__divider"
    >/</span>
    <input
      v-if="showYear"
      ref="year"
      v-model="year"
      class="FormDate__input FormDate__input--year"
      type="number"
      placeholder="yyyy"
      @blur="year = year.padStart(4, 0)">
  </div>
</template>

<script>
export default {
  name: `FormDate`,
  props: {
    value: {
      type: [Number, String],
      required: true,
    },
    showDay: {
      type: Boolean,
      default: true,
    },
    showMonth: {
      type: Boolean,
      default: true,
    },
    showYear: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      day: `${this.value ? new Date(this.value).getDate() : ``}`,
      month: `${this.value ? new Date(this.value).getMonth() + 1 : ``}`,
      year: `${this.value ? new Date(this.value).getFullYear(): ``}`,
    };
  },
  watch: {
    year(current, prev) {
      if (current > 9999) this.year = prev;
    },
  },
  methods: {
    updateDay() {
      if (!this.day.length || parseInt(this.day, 10) < 4) return;
      if (this.showMonth) this.$refs.month.select();
      else if (this.showYear) this.$refs.year.select();
    },
    updateMonth() {
      if (!this.month.length || parseInt(this.month, 10) < 2) return;
      if (this.showYear) this.$refs.year.select();
    },
    updateValue() {
      const timestamp = Date.parse(`${this.year.padStart(4, 0)}-${this.month}-${this.day}`);

      if (Number.isNaN(timestamp)) return;

      this.$emit(`input`, timestamp);
    },
  },
};
</script>

<style lang="scss">
.FormDate {
  $spacing: 0.75em;

  display: inline-flex;
  position: relative;
  overflow: hidden;
  border: 1px solid #888;
  border-radius: 0.25em;

  // 1. Hide the spinner button in Chrome, Safari and Firefox.
  &__input {
    padding: $spacing;
    padding-right: $spacing / 2;
    padding-left: $spacing / 2;
    border: none;
    text-align: center;
    /* stylelint-disable-next-line property-no-vendor-prefix */
    -moz-appearance: textfield; // 1

    &::-webkit-inner-spin-button {
      display: none; // 1
    }

    &:first-child {
      padding-left: $spacing;
    }

    &:last-child {
      padding-right: $spacing;
    }

    &:focus {
      outline: none;
    }

    &--day,
    &--month {
      width: 3em;
    }

    &--year {
      width: 4em;
    }
  }

  &__divider {
    padding-top: $spacing;
    padding-bottom: $spacing;
    pointer-events: none;
  }
}
</style>
