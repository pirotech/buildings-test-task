<template>
  <div id="app">
    <UiSelect
      :options="options"
      :value="value"
      @select="select"
    />
    <UiTextLine
      :value="phone"
      :mask="mask"
      @change="(value) => phoneChanged(value, 1)"
    />
    <UiTextLine
      :value="name"
      @change="(value) => nameChanged(value, 1)"
    />
    <button class="button">Hello</button>
  </div>
</template>

<script>
import UiSelect from './shared/components/UiSelect';
import UiTextLine from './shared/components/UiTextLine';

export default {
  name: 'app',
  components: {
    UiSelect,
    UiTextLine,
  },
  data() {
    return {
      options: [
        {
          value: 'a',
          label: 'a',
        },
        {
          value: 'b',
          label: 'b',
        }
      ],
      value: {
        value: 'a',
        label: 'a',
      },
      mask: '\\+7 (111) 111-11-11',
      phone: '',
      name: '',
    };
  },
  methods: {
    select(value) {
      this.value = value;
    },
    phoneChanged(value, number) {
      this.phone = value;
    },
    nameChanged(value, number) {
      this.name = value;
    }
  }
}
</script>

<style lang="scss">
@import "./shared/styles/app";

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
