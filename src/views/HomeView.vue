<template>
  <div>
    <h2>User Logs</h2>
    <template v-if="isLoading">
      <p>Loading...</p>
    </template>
    <template v-else-if="logs.length > 0">
      <ul>
        <li v-for="log in logs" :key="log.id">
          <p>Log ID: {{ log.id }}</p>
          <p>User: {{ log.user }}</p>
          <p>Country: {{ log.country }}</p>
        </li>
      </ul>
    </template>
    <template v-else>
      <p>No logs found for this user.</p>
    </template>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

export default {
  name: 'UserLogs',
  setup() {
    const route = useRoute();

    const logs = reactive([]);
    const isLoading = ref(true);

    const fetchLogs = async () => {
      try {
        const response = await axios.get(`https://api.josipdev.openseed.com.au/api/users/${route.params.userId}/logs`);
        logs.splice(0, logs.length, ...response.data.logs);
        console.log(logs)
        isLoading.value = false;
      } catch (error) {
        console.error(error);
      }
    };

    onMounted(() => {
      fetchLogs();
    });

    return {
      logs,
      isLoading
    };
  }
};
</script>
