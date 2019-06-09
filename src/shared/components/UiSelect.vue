<template>
  <VueMultiselect
    class="ui-select"
    :options="options"
    :value="value"
    trackBy="value"
    label="label"
    :searchable="false"
    placeholder=""
    selectLabel="Выбрать"
    deselectLabel="Удалить"
    selectedLabel=""
    @select="select"
  />
</template>

<script>
import VueMultiselect from 'vue-multiselect';

export default {
  name: "UiSelect",
  components: {
    VueMultiselect,
  },
  props: {
    options: {
      type: Array,
      required: true,
    },
    value: {
      type: Object,
      required: false,
    },
  },
  methods: {
    select(value) {
      this.$emit('select', value);
    },
  },
}
</script>

<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style lang="scss">
@import "../styles/variables";

.ui-select {
  width: 300px;
  .multiselect {
    &__select {
      &::before {
        color: $brown;
      }
    }
    &__tags {
      border: none;
      border-bottom: 1px solid $brown-400;
      background-color: $brown-100;
      border-radius: 0;
      color: $brown;
    }
    &__single {
      background-color: $brown-100;
      font-size: 16px;
    }
    &__content-wrapper {
      border: 1px solid $brown-100;
      border-top: none;
      border-radius: 0;
    }
    &__option {
      background: floralwhite;
      color: $brown;
      &::after {
        display: none;
      }
      &--highlight {
        background: darken($brown-100, 5%);
      }
    }
    &__placeholder {
    }
  }
}
</style>
