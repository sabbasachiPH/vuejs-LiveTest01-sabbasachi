
<script setup>
import { ref } from 'vue';

const tasks = ref([
  { name: 'Task 1', time: 30 },
  { name: 'Task 2', time: 40 },
  { name: 'Task 3', time: 60 },
  { name: 'Task 4', time: 45 },
  { name: 'Task 5', time: 50 },
]);

const isEditModalOpen = ref(false);
const editedTask = ref({ name: '', time: 0 });
let editedTaskIndex = null;

function openEditModal(index) {
  editedTask.value = { ...tasks.value[index] };
  editedTaskIndex = index;
  isEditModalOpen.value = true;
};

function closeEditModal() {
  isEditModalOpen.value = false;
};

function saveEditedTask() {
  if (editedTaskIndex !== null) {
    tasks.value.splice(editedTaskIndex, 1, { ...editedTask.value });
    closeEditModal();
  }
};

function deleteTask(id) {
  const itemPosition = tasks.value.findIndex(task => id === task.id);
  tasks.value.splice(itemPosition, 1);
}
</script>

<template>
  <div>
    <table class="w-6/12  border-collapse border border-gray-300 mt-8 ">
      <thead class="bg-gray-200">
        <tr>
          <th class="p-2">Name</th>
          <th class="p-2">Time</th>
          <th class="p-2">Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td class="p-2">{{ task.name }}</td>
          <td class="p-2">{{ task.time }}</td>
          <td class="p-2">
            <button @click="openEditModal(index)" class="bg-green-500 text-white px-4 py-2 rounded mr-2">Edit</button>
            <button @click="deleteTask(index)" class="bg-red-500 text-white px-4 py-2 rounded">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Edit Modal -->
    <div v-if="isEditModalOpen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50">
      <div class="bg-white p-6 rounded">
        <label class="block mb-2">Name:</label>
        <input v-model="editedTask.name" class="border border-gray-300 p-2 mb-4 w-full" />

        <label class="block mb-2">Time:</label>
        <input v-model="editedTask.time" type="number" class="border border-gray-300 p-2 mb-4 w-full" />

        <button @click="saveEditedTask" class="bg-green-500 text-white px-4 py-2 rounded mr-2">Save</button>
        <button @click="closeEditModal" class="bg-gray-400 text-white px-4 py-2 rounded">Cancel</button>
      </div>
    </div>
  </div>
</template>
