<template>
  <q-layout view="hHh Lpr lff" style="padding: 10px 10px;">
    <menu-toolbar-layout>
      <router-view/>
    </menu-toolbar-layout>
  </q-layout>
</template>

<script setup>
import MenuToolbarLayout from './MenuToolbarLayout.vue';
import { watch } from 'vue';

</script>
