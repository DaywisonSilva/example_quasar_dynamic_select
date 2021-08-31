<template>
  <q-page class="q-pa-md q-gutter-md">
    <q-select
      :optios="items[0]"
      v-model="items[0].value"
      :label="items[0].label"
      :options="items[0].filtro"
      @input="getNextField(items[0])"
      filled
    />
    <div v-for="(item, i) in nextFields" :key="i">
      <q-select
        :optios="items"
        v-model="item.value"
        :label="item.label"
        :options="item.filtro"
        @input="getNextField(item)"
        filled
        v-if="!(item.tipo === 'Final')"
      />
    </div>
  </q-page>
</template>

<script>
import data from "src/API/data.json";
export default {
  name: "PageIndex",
  data() {
    return {
      items: [],
      nextFields: []
    };
  },
  methods: {
    getNextField(field) {
      let objFiltered = this.items.filter(item => {
        return item.id == field.value;
      });

      let alreadyHasField = this.nextFields.some(field => field.id === objFiltered[0].id)
      console.log(alreadyHasField)

      if(!alreadyHasField) this.nextFields.push(objFiltered[0]);
    }
  },
  created() {
    // mapeando o json para colocar o atributo value
    let mapData = data.map(obj => {
      if (typeof obj.filtro != "object") {
        obj.filtro = [obj.filtro];
      }
      return {
        ...obj,
        value: null
      };
    });
    this.items = mapData;
  }
};
</script>
