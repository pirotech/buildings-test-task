<template>
  <div id="app">
    <h3>Стороны и объект договора</h3>
    <div class="form">
      <h4 class="client__title">Участник договора</h4>
      <div v-for="(client, index) in clients" :key="index" class="client">
        <UiTextLine
          class="client__name"
          :isValid="client.isValid"
          :value="client.name"
          @change="(value) => nameChanged(value, index)"
        />
        <UiTextLine
          class="client__phone"
          :isValid="client.isValid"
          :value="client.phone"
          :mask="phoneMask"
          @change="(value) => phoneChanged(value, index)"
        />
        <button class="button" @click="addClient">+</button>
      </div>
      <h4 class="ownership__title">Вид собственности</h4>
      <UiSelect
        class="ownership"
        :options="ownerships"
        :value="ownership"
        @select="ownershipSelected"
      />
      <div class="line">
        <div class="object">
          <h4 class="object__title">Объект:</h4>
          <p class="object__value">{{object || '-'}}</p>
        </div>
        <div class="apartment">
          <h4 class="apartment__title">Квартира:</h4>
          <p class="apartment__value">{{apartment || '-'}}</p>
        </div>
        <div class="axis">
          <h4 class="axis__title">Строительные оси:</h4>
          <p class="axis__value">{{axis || '-'}}</p>
        </div>
      </div>
      <div
        class="square-title"
        :class="{'square-title_hidden': tableHidden}"
        @click="toggleTable"
      >
        <h4>Площадь</h4>
        <p class="ellipsis"></p>
        <p class="value">{{summary || '-'}}m2</p>
      </div>
      <div v-show="!tableHidden" class="square-table">
        <div class="column">
          <h4 class="column__header">Жилая</h4>
          <div v-for="(item, index) in live" :key="index" class="column-line">
            <p>{{item.name || '-'}}</p>
            <p class="ellipsis"></p>
            <p class="value">{{item.value || '-'}}</p>
          </div>
        </div>
        <div class="column">
          <h4 class="column__header">Нежилая</h4>
          <div v-for="(item, index) in noneLive" :key="index" class="column-line">
            <p>{{item.name || '-'}}</p>
            <p class="ellipsis"></p>
            <p class="value">{{item.value || '-'}}</p>
          </div>
        </div>
        <div class="column">
          <h4 class="column__header">Площадь с пониж. кооф.</h4>
          <div v-for="(item, index) in noneLive" :key="index" class="column-line">
            <p>{{item.name || '-'}}</p>
            <p class="ellipsis"></p>
            <p class="value">{{item.value || '-'}}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="buttons">
      <button class="button submit-button" @click="submit">Отправить</button>
      <button class="button load-button">Загрузить</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
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
          isValid: true,
        }
      ],
      ownerships: [],
      ownership: null,
      tableHidden: false,
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
        isValid: true,
      });
    },
    ownershipSelected(value) {
      this.value = value;
    },
    toggleTable() {
      this.tableHidden = !this.tableHidden;
    },
    submit() {
      this.clients = this.clients.map(item => {
        const isValid = (item.name !== '') && (item.phone !== '') && !item.phone.includes('_');
        return {
          ...item,
          isValid: isValid,
        };
      });

      let formValid = this.clients.every(item => item.isValid);
      if (!formValid) {
        return;
      }

      axios({
        method: 'post',
        url: '/path/to/submit',
        data: {
          clients: this.clients,
          ownership: this.ownership,
        },
      });
    }
  },
  computed: {
    object() {
      return this.ownership && this.ownership.object;
    },
    apartment() {
      return this.ownership && this.ownership.apartment;
    },
    axis() {
      return this.ownership && this.ownership.axis;
    },
    summary() {
      return this.ownership && this.ownership.squares && this.ownership.squares.summary;
    },
    live() {
      const hasList = this.ownership && this.ownership.squares && this.ownership.squares.live;
      return hasList && this.ownership.squares.live || [];
    },
    noneLive() {
      const hasList = this.ownership && this.ownership.squares && this.ownership.squares.noneLive;
      return hasList && this.ownership.squares.noneLive || [];
    },
    lowLive() {
      const hasList = this.ownership && this.ownership.squares && this.ownership.squares.lowLive;
      return hasList && this.ownership.squares.lowLive || [];
    },
  },
  mounted() {
    axios({
      method: 'get',
      url: '/ownerships.json'
    }).then(response => {
      this.ownerships = response.data.map((item, index) => ({
        ...item,
        value: index,
        label: item.name || '',
      }));
      this.ownership = this.ownerships.length > 0 ? this.ownerships[0] : null;
    }).catch(error => {
      console.error(error);
    });
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
    width: 840px;
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
    .ownership__title {
      margin-top: 40px;
    }
    .ownership {
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
        content: '⏶';
        margin-top: -4px;
        color: #333;
      }
      &_hidden {
        &:after {
          content: '⏷';
        }
      }
    }
    .square-table {
      display: flex;
      flex-direction: row;
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
  .buttons {
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    width: 840px;
    .submit-button {
      margin-top: 20px;
    }
    .load-button {
      margin-top: 10px;
    }
  }
}
</style>
