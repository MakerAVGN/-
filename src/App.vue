<template>
  <div>
    <button @click="openModal">Открыть</button>
    <ModalComponent
      :title="'Выберите папку'"
      :folders="mockFolders"
      :is-open="isModalOpen"
      @close="closeModal"
      @select="handleFolderSelect"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import ModalComponent from "./components/ModalComponent.vue";

const mockFolders = [
  {
    id: 1,
    name: "Папка 1",
    children: [
      { id: 2, name: "Папка 1.1", children: [] },
      {
        id: 3,
        name: "Папка 1.2",
        children: [{ id: 4, name: "Папка 1.2.1", children: [] }],
      },
    ],
  },
  { id: 5, name: "Папка 2", children: [] },
];

export default defineComponent({
  name: "App",
  components: { ModalComponent },
  setup() {
    const isModalOpen = ref(false);

    const openModal = () => {
      isModalOpen.value = true;
    };

    const closeModal = () => {
      isModalOpen.value = false;
    };

    const handleFolderSelect = (folderId: number) => {
      console.log("Выбрана папка:", folderId);
      // Здесь ваша логика обработки выбранной папки
    };

    return {
      isModalOpen,
      mockFolders,
      openModal,
      closeModal,
      handleFolderSelect,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
