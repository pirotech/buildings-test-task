<template>
  <VueMaskedInput
    v-if="mask"
    class="text-line"
    :value="value"
    :mask="mask"
    @input="changed"
  />
  <input
    v-else
    class="text-line"
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
}
</style>
