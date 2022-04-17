<template>
  <section>
    <v-row>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Number of instructions"
          type="number"
          v-model="configurations.instructionCount"
          outlined
          hide-details
        ></v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-select
          v-model="configurations.instructionList"
          :items="instructionList"
          item-text="label"
          item-value="value"
          label="Instructions"
          multiple
          outlined
          hide-details
        >
          <template v-slot:selection="{ item, index }">
            <span v-if="index === 0">{{ item.label }}</span>
            <span
              v-if="index === 1"
              class="ml-2 grey--text text-caption"
            >
              (+{{ configurations.instructionList.length - 1 }})
            </span>
          </template>
        </v-select>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Memory size"
          type="number"
          v-model="configurations.memorySize"
          outlined
          hide-details
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn
          dark
          color="primary"
          @click="generate"
        >
          OK
        </v-btn>
      </v-col>
    </v-row>
  </section>
</template>

<script>
import { saveAs } from 'file-saver';
import { getCurrentDateWithHour, decimalToHex, getRandomInt } from '../helpers';

export default {
  name: 'tab-trace',
  data: () => ({
    instructionList: [
      {
        value: 0,
        label: '0 - Leitura de dados',
      },
      {
        value: 1,
        label: '1 - Gravação de dados',
      },
      {
        value: 2,
        label: '2 - Busca de instrução',
      },
      {
        value: 3,
        label: '3 - Registro escape (tratado como tipo de acesso desconhecido)',
      },
      {
        value: 4,
        label: '4 - Registro escape (operação de cache flush)',
      },
    ],
    configurations: {
      instructionCount: 0,
      instructionList: [],
      memorySize: 0,
    },
  }),
  methods: {
    generate() {
      const fileData = this.getFileData();
      const fileName = `trace-${getCurrentDateWithHour()}.txt`;
      const blob = new Blob([fileData], {type: "text/plain;charset=utf-8"});
      saveAs(blob, fileName);
    },
    getFileData() {
      let data = '';
      for(let i = 0; i < this.configurations.instructionCount; i++) {
        const instructionPosition = getRandomInt(0, this.configurations.instructionList.length - 1);
        const instruction = this.configurations.instructionList[instructionPosition];
        const randomMemoryAddress = getRandomInt(0, this.configurations.memorySize - 1);
        data += `${instruction} ${decimalToHex(randomMemoryAddress)}\n`;
      }
      return data;
    },
  },
}
</script>
