<template>
<span>JAVIER:</span>
<table border="1">
  <thead>
    <tr>
      <th>Signal List Name</th>
      <th>Trigger Usability</th>
      <th>Timeframe</th>
      <th>Indicator</th>
      <th>Priority</th>
      <th>Signal Name</th>
      <th>Active</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(row, idx) in tableRows" :key="idx">
      <td>{{ row.signal_list_name }}</td>
      <td>{{ row.trigger_usability }}</td>
      <td>{{ row.timeframe }}</td>
      <td>{{ row.indicator }}</td>
      <td>{{ row.priority }}</td>
      <td>{{ row.signalname }}</td>
      <td>{{ row.active }}</td>
    </tr>
  </tbody>
</table>

<div>
  <h3>Response Data:</h3>
  <pre style="width:500px;">{{ respData }}</pre>
</div>
<hr />
<div v-if="respError">
  <h3>Error:</h3>
  <pre>{{ respError }}</pre>
</div>

</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';



interface TableRow {
  signal_list_name: string;
  trigger_usability: string;
  timeframe: string;
  indicator: string;
  priority: number;
  signalname: string;
  active: number;
}

interface ApiResponse {
  resultx: string;
  errorx: string;
  dtResult: TableRow[];
}

const tableRows = ref<TableRow[]>([]);
const respError = ref<string>('');
const respData = ref<string>('');

onMounted(async () => {
  try {
    const response = await axios.post<ApiResponse>(
      'https://fastapi-app-production-d82b.up.railway.app/getAll/',
      {
        signal_list_name: 'pls_usd',
        timeframe: '5m',
        trigger_usability: 'buy',
        signalname: 'impulse_macd_signal_white<bottom_red_line',
        is_signal_active: '1',
      }
    );
  respData.value = JSON.stringify(response.data);
  tableRows.value = response.data.dtResult || [];
  } catch (error) {
    console.error('API call failed:', error);
    respError.value = ''+error;
  }
});
</script>

<style scoped>
</style>
