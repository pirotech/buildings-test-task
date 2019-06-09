<template>
  <VueMaskedInput
    v-if="mask"
    class="text-line"
    :class="{'text-line_invalid': !isValid}"
    :value="value"
    :mask="mask"
    @input="changed"
  />
  <input
    v-else
    class="text-line"
    :class="{'text-line_invalid': !isValid}"
    type="text"
    :value="value"
    @input="changed($event.target.value)"
  />
</template>

<script>
import VueMaskedInput from 'vue-masked-input';

export default {
  name: "UiTextLine",
  components: {
    VueMaskedInput,
  },
  props: {
    isValid: {
      type: Boolean,
      required: false,
      default() {
        return true;
      },
    },
    value: {
      type: String,
      required: true,
    },
    mask: {
      type: String,
      required: false,
    },
  },
  methods: {
    changed(value) {
      this.$emit('change', value);
    },
  },
}
</script>

<style lang="scss" scoped>
@import "../styles/variables";

.text-line {
  width: 300px;
  height: 40px;
  padding: 0 15px;
  border: none;
  border-bottom: 1px solid $brown;
  background-color: $brown-100;
  font-size: 16px;
  color: $brown;
  transition: all 0.5s;
  &_invalid {
    background-color: $red-300;
    border-color: $red;
  }
}
</style>
