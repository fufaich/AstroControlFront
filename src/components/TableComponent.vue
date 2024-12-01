<template>
  <div class="p-6">
    <!-- Заголовок -->
    <h2 class="text-2xl font-bold mb-4">Users</h2>

    <!-- Таблица -->
    <div class="overflow-x-auto">
      <table class="min-w-full border border-gray-300 text-left">
        <thead class="bg-gray-100">
        <tr>
          <!-- Генерируем заголовки таблицы -->
          <th
            v-for="header in headers_table"
            :key="header"
            class="px-4 py-2 border-b"
          >
            {{ header }}
          </th>
          <th class="px-4 py-2 border-b text-center">ACTIONS</th>
        </tr>
        </thead>
        <tbody>
        <!-- Генерируем строки таблицы -->
        <tr
          v-for="(row, rowIndex) in rows"
          :key="row[0]"
          class="hover:bg-gray-50"
        >
          <td
            v-for="(header, colIndex) in headers_table"
            :key="colIndex"
            class="px-4 py-2 border-b"
          >
            {{ row[colIndex] }}
          </td>
          <td class="px-4 py-2 border-b text-center">
            <button
              class="text-red-500 hover:text-red-700"
              @click="deleteUser(rowIndex)"
            >
              ✖
            </button>
          </td>
        </tr>
        </tbody>
      </table>
    </div>

    <!-- Кнопка добавления -->
    <button
      class="mt-4 flex items-center bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded focus:outline-none"
      @click="addUser"
    >
      <span class="text-lg">+</span>
      <span class="ml-2">Add User</span>
    </button>
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";

// Получение токена из localStorage
const getAuthToken = () => {
  return localStorage.getItem("jwt");
};

const match_table_name = {
  "task_journal" : "TaskJournal",
  "users" : "users",
  "completed_tasks" : "CompletedTasks",
  "missions" : "Mission",
  "resources" : "Resources",
  "employees" : "Employee",
  "reports_journal" : "ReportsJournal",
  "experiment_journal" : "ExperimentJournal"
}
// Настройка axios с токеном
axios.interceptors.request.use(
  (config) => {
    const token = getAuthToken();
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);


// Получение заголовков и данных таблицы
const fetchHeadersAndData = async (table_name) => {
  const headersResponse = await axios.get(
    `http://127.0.0.1:5000/utils/${match_table_name[table_name]}`,
  );
  const dataResponse = await axios.get(
    `http://127.0.0.1:5000/${table_name}/`,
    {
      "data": {}
    }
  );

  return {
    headers: headersResponse.data,
    data: dataResponse.data,
  };
};

const props = defineProps({
  table_name: String,
});

const headers_table = ref([]);
const rows = ref([]);

// Функция для добавления пользователя
const addUser = () => {
  const newUser = {};
  headers_table.value.forEach((header) => {
    newUser[header] = "default";
  });
  rows.value.push(newUser);
};

// Функция для удаления пользователя
const deleteUser = (index) => {
  rows.value.splice(index, 1);
};

// Инициализация данных
onMounted(async () => {
  const { headers, data } = await fetchHeadersAndData(props.table_name);
  headers_table.value = headers.map(value => value[0]);
  rows.value = data;
  console.log(rows.value);
});
</script>

