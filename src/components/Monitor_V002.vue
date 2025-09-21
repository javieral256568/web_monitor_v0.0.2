<template>
<span>JAVIER:</span>
<table border="1">
  <thead>
    <tr>
      <th>Signal List Name</th>
      <th>Timeframe</th>
      <th>Trigger Usability</th>
      <th>Signal Name</th>
      <th>Is Signal Active</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>{{ tableData.signal_list_name }}</td>
      <td>{{ tableData.timeframe }}</td>
      <td>{{ tableData.trigger_usability }}</td>
      <td>{{ tableData.signalname }}</td>
      <td>{{ tableData.is_signal_active }}</td>
    </tr>
  </tbody>
</table>

<div>
  <h3>Response Data:</h3>
  <pre>{{ respData }}</pre>
</div>

<div v-if="respError">
  <h3>Error:</h3>
  <pre>{{ respError }}</pre>
</div>

</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface InParams {
  signal_list_name: string;
  timeframe: string;
  trigger_usability: string;
  signalname: string;
  is_signal_active: string;
}

interface ApiResponse {
  resultx: string;
  errorx: string;
  inParams: InParams;
}


const tableData = ref<InParams>({
  signal_list_name: '',
  timeframe: '',
  trigger_usability: '',
  signalname: '',
  is_signal_active: '',
});

const respError = ref<string>('');
const respData = ref<string>('');

onMounted(async () => {
  try {
    const response = await axios.post<ApiResponse>(
      'https://fastapi-app-production-d82b.up.railway.app/items/',
      {
        signal_list_name: 'pls_usd',
        timeframe: '5m',
        trigger_usability: 'buy',
        signalname: 'impulse_macd_signal_white<bottom_red_line',
        is_signal_active: '1',
      }
    );
    respData.value = JSON.stringify(response.data);
    tableData.value = response.data.inParams;
  } catch (error) {
    console.error('API call failed:', error);
    respError.value = ''+error;
  }
});
</script>

<style scoped>
</style>
