<template>
  <section>
    <p class="font-weight-bold" style="opacity: 0.7">Processor</p>
    <v-row>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Processor contains"
          type="number"
          v-model="configurations.processor.processorContains"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Create trace file"
          type="number"
          v-model="configurations.processor.createTraceFile"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
    </v-row>

    <p class="mt-10 font-weight-bold" style="opacity: 0.7">Trace</p>
    <v-row>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Word size"
          type="number"
          v-model="configurations.trace.wordSize"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
    </v-row>

    <p class="mt-10 font-weight-bold" style="opacity: 0.7">CPU</p>
    <v-row>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Word size"
          type="number"
          v-model="configurations.cpu.wordSize"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
    </v-row>

    <p class="mt-10 font-weight-bold" style="opacity: 0.7">Main Memory</p>
    <v-row>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Block size"
          type="number"
          v-model="configurations.mainMemory.blockSize"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Memory size"
          type="number"
          v-model="configurations.mainMemory.memorySize"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Cicles per access read"
          type="number"
          v-model="configurations.mainMemory.ciclesPerAccessRead"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Cicles per access write"
          type="number"
          v-model="configurations.mainMemory.ciclesPerAccessWrite"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
      <v-col
        md="4"
        cols="12"
      >
        <v-text-field
          label="Time cicle"
          type="number"
          v-model="configurations.mainMemory.timeCicle"
          outlined
          hide-details
        >
        </v-text-field>
      </v-col>
    </v-row>

    <p class="mt-10 font-weight-bold" style="opacity: 0.7">Cache</p>
    <div
      v-for="(cache, index) in configurations.cache"
      :key="`cache-${index}`"
    >
      <p>Cache L{{ index + 1 }}</p>
      <v-row class="mb-1">
        <v-col
          md="4"
          cols="12"
        >
          <v-select
            label="Type"
            v-model="configurations.cache[index].type"
            :items="cacheTypeList"
            outlined
            hide-details
          ></v-select>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Line size"
            type="number"
            v-model="configurations.cache[index].lineSize"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Cicles per access read"
            type="number"
            v-model="configurations.cache[index].ciclesPerAccessRead"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Cicles per access write"
            type="number"
            v-model="configurations.cache[index].ciclesPerAccessWrite"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Time cicle"
            type="number"
            v-model="configurations.cache[index].timeCicle"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Memory size"
            type="number"
            v-model="configurations.cache[index].memorySize"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-text-field
            label="Associativity level"
            type="number"
            v-model="configurations.cache[index].associativityLevel"
            outlined
            hide-details
          ></v-text-field>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-select
            label="Write policy"
            v-model="configurations.cache[index].writePolicy"
            :items="writePolicyList"
            outlined
            hide-details
          ></v-select>
        </v-col>
        <v-col
          md="4"
          cols="12"
        >
          <v-select
            label="Replacement algorithm"
            v-model="configurations.cache[index].replacementAlgorithm"
            :items="replacementAlgorithmList"
            outlined
            hide-details
          ></v-select>
        </v-col>
      </v-row>
    </div>
    <v-row>
      <v-col>
        <v-btn
          dark
          color="default"
          @click="cacheAdd"
        >
          +
        </v-btn>
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
import { getCurrentDateWithHour } from '../helpers';

export default {
  name: 'tab-architecture',
  data: () => ({
    cacheTypeList: ['Unified'],
    writePolicyList: ['WriteThrough', 'WriteBack'],
    replacementAlgorithmList: ['LRU', 'FIFO'],
    configurations: {
      processor: {
        processorContains: 0,
        createTraceFile: 0,
      },
      trace: {
        wordSize: 0,
      },
      cpu: {
        wordSize: 0,
      },
      mainMemory: {
        blockSize: 0,
        memorySize: 0,
        ciclesPerAccessRead: 0,
        ciclesPerAccessWrite: 0,
        timeCicle: 0,
      },
      cache: [],
    },
  }),
  methods: {
    cacheAdd() {
      this.configurations.cache.push({
        type: 'Unified',
        lineSize: 0,
        ciclesPerAccessRead: 0,
        ciclesPerAccessWrite: 0,
        timeCicle: 0,
        memorySize: 0,
        associativityLevel: 0,
        writePolicy: 0,
        replacementAlgorithm: 0,
      });
    },
    cacheRemove(index) {
      this.configurations.cache.splice(index, 1);
    },
    generate() {
      const fileData = this.getFileData();
      const fileName = `architecture-${getCurrentDateWithHour()}.xml`;
      const blob = new Blob([fileData], {type: "text/xml;charset=utf-8"});
      saveAs(blob, fileName);
    },
    getFileData() {
      const caches = this.configurations.cache.reduce(
        (accumulator, cache) => `
          ${accumulator}
          <Cache>
            <cacheType>${cache.type}</cacheType>
            <unifiedCache>
              <lineSize>${cache.lineSize}</lineSize>
              <ciclesPerAccessRead>${cache.ciclesPerAccessRead}</ciclesPerAccessRead>
              <ciclesPerAccessWrite>${cache.ciclesPerAccessWrite}</ciclesPerAccessWrite>
              <timeCicle>${cache.timeCicle}</timeCicle>
              <memorySize>${cache.memorySize}</memorySize>
              <associativityLevel>${cache.associativityLevel}</associativityLevel>
              <writePolicy>${cache.writePolicy}</writePolicy>
              <replacementAlgorithm>${cache.replacementAlgorithm}</replacementAlgorithm>
            </unifiedCache>
          </Cache>
        `,
        ''
      );

      const data = `
        <?xml version="1.0" encoding="ISO-8859-1"?> 
        <!DOCTYPE AmnesiaConfiguration SYSTEM "Configuration/amnesia.dtd">
        <?xml-stylesheet type="text/css" href="teste.css"?>
        <AmnesiaConfiguration>
          <Processor>
            <processorContains>${this.configurations.processor.processorContains}</processorContains>
            <createTraceFile>${this.configurations.processor.createTraceFile}</createTraceFile>
          </Processor>
          <Trace>
            <wordSize>${this.configurations.trace.wordSize}</wordSize>
          </Trace>
          <CPU>
            <wordSize>${this.configurations.cpu.wordSize}</wordSize>
          </CPU>
          <MainMemory>
            <blockSize>${this.configurations.mainMemory.blockSize}</blockSize>
            <memorySize>${this.configurations.mainMemory.memorySize}</memorySize>
            <ciclesPerAccessRead>${this.configurations.mainMemory.ciclesPerAccessRead}</ciclesPerAccessRead>
            <ciclesPerAccessWrite>${this.configurations.mainMemory.ciclesPerAccessWrite}</ciclesPerAccessWrite>
            <timeCicle>${this.configurations.mainMemory.timeCicle}</timeCicle>
          </MainMemory>
          ${caches}
        </AmnesiaConfiguration>
      `;

      return data.replaceAll('        ', '').trim();
    },
  },
}
</script>
