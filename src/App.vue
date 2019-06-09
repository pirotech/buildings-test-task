<template>
  <div id="app">
    <h3>Стороны и объект договора</h3>
    <div class="form">
      <h4 class="client__title">Участник договора</h4>
      <div v-for="(client, index) in clients" :key="index" class="client">
        <UiTextLine
          class="client__name"
          :value="client.name"
          @change="(value) => nameChanged(value, index)"
        />
        <UiTextLine
          class="client__phone"
          :value="client.phone"
          :mask="phoneMask"
          @change="(value) => phoneChanged(value, index)"
        />
        <button class="button" @click="addClient">+</button>
      </div>
      <h4 class="type__title">Вид собственности</h4>
      <UiSelect
        class="type"
        :options="options"
        :value="value"
        @select="select"
      />
      <div class="line">
        <div class="object">
          <h4 class="object__title">Объект:</h4>
          <p class="object__value">O1</p>
        </div>
        <div class="apartment">
          <h4 class="apartment__title">Квартира:</h4>
          <p class="apartment__value">O1</p>
        </div>
        <div class="axis">
          <h4 class="axis__title">Строительные оси:</h4>
          <p class="axis__value">O1</p>
        </div>
      </div>
      <div class="square-title">
        <h4>Площадь</h4>
        <p class="ellipsis"></p>
        <p class="value">30m2</p>
      </div>
      <div class="square-table">
        <div class="column">
          <h4 class="column__header">Жилая</h4>
          <div class="column-line">
            <p>Площадь</p>
            <p class="ellipsis"></p>
            <p class="value">30m2</p>
          </div>
        </div>
        <div class="column">
          <h4 class="column__header">Нежилая</h4>
          <div class="column-line">
            <p>Площадь</p>
            <p class="ellipsis"></p>
            <p class="value">30m2</p>
          </div>
        </div>
        <div class="column">
          <h4 class="column__header">Площадь с пониж. кооф.</h4>
          <div class="column-line">
            <p>Площадь</p>
            <p class="ellipsis"></p>
            <p class="value">30m2</p>
          </div>
        </div>
      </div>
    </div>
    <button class="button submit-button">Отправить</button>
    <button class="button load-button">Загрузить</button>
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
      phoneMask: '\\+7 (111) 111-11-11',
      clients: [
        {
          name: '',
          phone: '',
        }
      ],
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

    };
  },
  methods: {
    nameChanged(value, index) {
      this.clients[index].name = value;
    },
    phoneChanged(value, index) {
      this.clients[index].phone = value;
    },
    addClient() {
      this.clients.push({
        name: '',
        phone: '',
      });
    },
    select(value) {
      this.value = value;
    },
  }
}
</script>

<style lang="scss">
@import "./shared/styles/app";

#app {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  flex: 0 1 1200px;
  margin: 50px;
  .form {
    padding: 20px;
    margin-top: 20px;
    border: 1px solid #ddd;
    border-radius: 2px;
    .client__title {}
    .client {
      margin-top: 10px;
      &__name, &__phone {
        margin-right: 20px;
      }
    }
    .type__title {
      margin-top: 40px;
    }
    .type {
      margin-top: 10px;
    }
    .line {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      margin-top: 40px;
      .object, .apartment, .axis {
        display: flex;
        flex-direction: row;
        align-items: center;
        &__title {
          margin-right: 10px;
        }
        &__value {}
      }
    }
    .square-title {
      display: flex;
      flex-direction: row;
      justify-content: stretch;
      align-items: center;
      width: 250px;
      margin-top: 40px;
      cursor: pointer;
      .ellipsis {
        flex-grow: 1;
        margin: 0 5px;
        border-bottom: 2px dotted #333;
      }
      &:after {
        content: '⏷';
      }
      &_hidden {
        &:after {
          content: '⏶';
        }
      }
    }
    .square-table {
      display: flex;
      flex-direction: row;
      width: 800px;
      margin-top: 10px;
      .column {
        display: flex;
        flex-direction: column;
        flex: 1 0 33%;
        &__header {
          display: flex;
          flex-direction: row;
          align-items: center;
          padding: 0 10px;
          line-height: 40px;
          background-color: $brown-100;
        }
        &-line {
          display: flex;
          flex-direction: row;
          align-items: center;
          padding: 0 10px;
          line-height: 40px;
          .ellipsis {
            flex-grow: 1;
            margin: 0 5px;
            border-bottom: 2px dotted #333;
          }
        }
      }
    }
  }
  .submit-button {
    margin-top: 20px;
  }
  .load-button {
    margin-top: 10px;
  }
}
</style>
