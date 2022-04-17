<template>
  <v-app>
    <v-toolbar
      dark
      flat
      color="primary"
      style="flex: initial"
    >
      <v-toolbar-title>Amnesia Configurations Generator</v-toolbar-title>
      <v-spacer></v-spacer>
      <template v-slot:extension>
        <v-tabs v-model="selectedTab">
          <v-tabs-slider color="yellow"></v-tabs-slider>
          <v-tab
            v-for="(tab, index) in tabs"
            :key="`tab-${index}`"
          >
            {{ tab.title }}
          </v-tab>
        </v-tabs>
      </template>
    </v-toolbar>
    <v-tabs-items
      v-model="selectedTab"
      class="pa-4"
    >
      <v-tab-item
        v-for="(tab, index) in tabs"
        :key="`tab-item-${index}`"
      >
        <v-compontent :is="tab.component"></v-compontent>
      </v-tab-item>
    </v-tabs-items>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  components: {},
  data: () => ({
    selectedTab: 0,
    tabs: [
      {
        title: 'Architecture',
        component: () => import('./components/Architecture.vue'),
      },
      {
        title: 'Trace',
        component: () => import('./components/Trace.vue'),
      }
    ]
  }),
};
</script>
