<script setup lang="ts">
import {ref, onMounted} from 'vue';

import { readTextFile } from '@tauri-apps/api/fs';
import {downloadDir, normalize} from '@tauri-apps/api/path';


const file = ref<any>('');
const fileLoc = ref<any>(null);


onMounted(async () => {
  const downloadsDirectory = await downloadDir();
  fileLoc.value = await normalize(`${downloadsDirectory}/create-this-exact/.filename.txt`);
})

async function readFile() {
  try {
    file.value = await readTextFile(fileLoc.value);
    console.log('read file success');
  } catch(e:any) {
    file.value = e;
    console.error('read file failure');
    console.error(e);
  }
}
</script>

<template>
  <div class="container">
    <h1 @click="readFile">Click me to read file {{fileLoc}}</h1>
    {{file}}
  </div>
</template>
