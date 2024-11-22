<template>
  <div v-if="isOpen" class="modal">
    <div class="modal-content">
      <h2>{{ title }}</h2>
      <FolderTree :folders="folders" @folder-selected="onFolderSelect" />
      <button @click="onConfirm">Ок</button>
      <button @click="onClose">Закрыть</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import FolderTree from "./FolderTree.vue";

interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

export default defineComponent({
  name: "ModalComponent",
  components: { FolderTree },
  props: {
    title: {
      type: String,
      required: true,
    },
    folders: {
      type: Array as () => Folder[],
      required: true,
    },
    isOpen: {
      type: Boolean,
      default: false,
    },
  },
  emits: ["close", "select"],
  setup(props, { emit }) {
    const selectedFolderId = ref<number | null>(null);

    const onClose = () => emit("close");

    const onConfirm = () => {
      if (selectedFolderId.value !== null) {
        emit("select", selectedFolderId.value);
        emit("close");
      } else {
        alert("Пожалуйста, выберите папку.");
      }
    };

    const onFolderSelect = (id: number) => {
      selectedFolderId.value = id;
      console.log(`Выбрана папка с ID: ${id}`);
    };

    return {
      onClose,
      onConfirm,
      onFolderSelect,
    };
  },
});
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: white;
  padding: 20px;
  margin: 10% auto;
  width: 80%;
  max-width: 600px;
}
</style>
