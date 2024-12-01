<template>
  <div class="p-6">
    <!-- Заголовок -->
    <h2 class="text-2xl font-bold mb-4">{{ match_table_name[table_name] }}</h2>

    <!-- Таблица -->
    <div class="overflow-x-auto">
      <table class="min-w-full border border-gray-300 text-left">
        <thead class="bg-gray-100">
          <tr>
            <!-- Генерируем заголовки таблицы -->
            <th v-for="header in headers_table" :key="header" class="px-4 py-2 border-b">
              {{ header }}
            </th>
            <th class="px-4 py-2 border-b text-center">ACTIONS</th>
          </tr>
        </thead>
        <tbody>
          <!-- Генерируем строки таблицы -->
          <tr v-for="(row, rowIndex) in rows" :key="row[0]" class="hover:bg-gray-50">
            <td
              v-for="(header, colIndex) in headers_table"
              :key="colIndex"
              class="px-4 py-2 border-b"
            >
              {{ row[colIndex] }}
            </td>
            <td class="px-4 py-2 border-b text-center">
              <button class="text-blue-500 hover:text-blue-700 mr-2" @click="openEditModal(row, headers_table)">
                ✏
              </button>
              <button
                class="text-red-500 hover:text-red-700"
                @click="deleteUser(headers_table[0], row[0])"
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
      @click="openAddModal"
    >
      <span class="text-lg">+</span>
      <span class="ml-2">Add User</span>
    </button>

    <!-- Модальное окно Edit -->
    <div
      v-if="isEditModalOpen"
      class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center"
    >
      <div class="bg-white p-6 rounded shadow-md w-96">
        <h3 class="text-lg font-bold mb-4">Edit User</h3>
        <div v-for="(value, key) in editableRow" :key="key" class="mb-3">
          <label class="block text-sm font-medium mb-1">{{ key }}</label>
          <input
            type="text"
            v-model="editableRow[key]"
            class="w-full border border-gray-300 rounded px-2 py-1"
          />
        </div>
        <div class="flex justify-end space-x-2 mt-4">
          <button
            class="bg-gray-300 hover:bg-gray-400 text-gray-800 py-1 px-3 rounded"
            @click="closeEditModal"
          >
            Cancel
          </button>
          <button
            class="bg-blue-500 hover:bg-blue-600 text-white py-1 px-3 rounded"
            @click="saveEdit"
          >
            Save
          </button>
        </div>
      </div>
    </div>

    <!-- Модальное окно для добавления -->
    <div
      v-if="isAddModalOpen"
      class="fixed inset-0 bg-gray-900 bg-opacity-50 flex items-center justify-center"
    >
      <div class="bg-white p-6 rounded shadow-md w-96">
        <h3 class="text-lg font-bold mb-4">Add New User</h3>
        <div v-for="header in headers_table" :key="header" class="mb-3">
          <label class="block text-sm font-medium mb-1">{{ header }}</label>
          <input
            type="text"
            v-model="newRow[header]"
            class="w-full border border-gray-300 rounded px-2 py-1"
          />
        </div>
        <div class="flex justify-end space-x-2 mt-4">
          <button
            class="bg-gray-300 hover:bg-gray-400 text-gray-800 py-1 px-3 rounded"
            @click="closeAddModal"
          >
            Cancel
          </button>
          <button
            class="bg-blue-500 hover:bg-blue-600 text-white py-1 px-3 rounded"
            @click="saveAdd"
          >
            Add
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import router from '@/router/index.js'

const props = defineProps({
  table_name: String,
})

const isEditModalOpen = ref(false)
const editableRow = ref({})

// Открытие модального окна для редактирования
const openEditModal = (row_data, headers) => {
  const newObject = {};

  let i = 0;
  headers.forEach((header) => {
    if (header.includes("id")){
      header = "id";
    }
    newObject[header] = row_data[i++]
})

  editableRow.value = newObject // Копируем данные строки
  console.log("headers", headers)
  console.log("row_data", row_data)
  console.log("newObject", newObject)


  isEditModalOpen.value = true
}

// Закрытие модального окна
const closeEditModal = () => {
  isEditModalOpen.value = false
}

// Сохранение изменений
const saveEdit = async () => {
  console.log("editableRow", editableRow.value)
  const updateObject = editableRow.value
  const response = await axios.put(`http://127.0.0.1:5000/${props.table_name}/`, updateObject);
  console.log(response)
  closeEditModal()
  reloadData()
}


const isAddModalOpen = ref(false)
const newRow = ref({})

// Открытие модального окна для добавления
const openAddModal = () => {
  newRow.value = {}
  headers_table.value.forEach((header) => {
    newRow.value[header] = '' // Инициализация пустыми значениями
  })
  isAddModalOpen.value = true
}

// Закрытие модального окна
const closeAddModal = () => {
  isAddModalOpen.value = false
}

// Сохранение новой записи
const saveAdd = () => {
  newRow.value = ({ ...newRow.value });
  add_api(newRow.value);
  closeAddModal();
  reloadData();
}


// Получение токена из localStorage
const getAuthToken = () => {
  return localStorage.getItem('jwt')
}

const match_table_name = {
  task_journal: 'TaskJournal',
  users: 'users',
  completed_tasks: 'CompletedTasks',
  missions: 'Mission',
  resources: 'Resources',
  employees: 'Employee',
  reports_journal: 'ReportsJournal',
  experiment_journal: 'ExperimentJournal',
}
// Настройка axios с токеном
axios.interceptors.request.use(
  (config) => {
    const token = getAuthToken()
    if (token) {
      config.headers.Authorization = `Bearer ${token}`
    }
    return config
  },
  (error) => {
    return Promise.reject(error)
  },
)

const add_api = async (object) => {
  const response = await axios.post(`http://127.0.0.1:5000/${props.table_name}/`, object);
  console.log(response)
}
// Получение заголовков и данных таблицы
const fetchHeadersAndData = async (table_name) => {
  try {
    const headersResponse = await axios.get(`http://127.0.0.1:5000/utils/${match_table_name[table_name]}`);
    const dataResponse = await axios.get(`http://127.0.0.1:5000/${table_name}/`);

    return {
      headers: headersResponse,
      data: dataResponse,
    }
  } catch (error) {
    if (error.response.status === 401) {
        await router.push('/login'); // Перенаправление на страницу логина, если токен отсутствует
      }
  }
  return {
    headers: null,
    data: null,
  }


}



const headers_table = ref([])
const rows = ref([])



// Функция для удаления пользователя
const deleteUser = async (primary_key, index) => {
  const object = {
    [primary_key] : index
  };
  console.log(object);
  const response = await axios.delete(`http://127.0.0.1:5000/${props.table_name}/`, {
    data: object
  });
  console.log(response);
  reloadData();
}


const reloadData = () => {
  loadData()
}

const loadData = async () => {
  try {
    const { headers, data } = await fetchHeadersAndData(props.table_name)
    headers_table.value = headers.data.map((value) => value[0])
    rows.value = data.data
  } catch (error) {
    console.error('Error loading data:', error)
  }
}

// Инициализация данных
onMounted(async () => {
  const { headers, data } = await fetchHeadersAndData(props.table_name);
  headers_table.value = (headers.data).map((value) => value[0]);
  rows.value = data.data;
})
</script>
