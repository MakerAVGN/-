<template>
  <div>
    <button @click="openModal">Открыть</button>
    <ModalComponent
      :folders="mockFolders"
      :title="'Выберите папку'"
      :isOpen="isModalOpen"
      @close="closeModal"
      @confirm="handleFolderSelect"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import ModalComponent from "@/components/ModalComponent.vue";

const mockFolders = [
  {
    id: 1,
    name: "Папка 1",
    children: [
      {
        id: 2,
        name: "Папка 1.1",
        children: [],
      },
      {
        id: 3,
        name: "Папка 1.2",
        children: [
          {
            id: 4,
            name: "Папка 1.2.1",
            children: [],
          },
        ],
      },
    ],
  },
  {
    id: 5,
    name: "Папка 2",
    children: [],
  },
];

export default defineComponent({
  name: "HomePage",
  components: { ModalComponent },
  setup() {
    const isModalOpen = ref(false);
    const selectedFolder = ref<number | null>(null);

    const openModal = () => {
      isModalOpen.value = true;
    };

    const closeModal = () => {
      isModalOpen.value = false;
    };

    const handleFolderSelect = (id: number) => {
      selectedFolder.value = id;
      console.log(`Выбрана папка с ID: ${id}`);
    };

    return {
      openModal,
      closeModal,
      handleFolderSelect,
      mockFolders,
      isModalOpen,
    };
  },
});
</script>

<style scoped>
button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>
