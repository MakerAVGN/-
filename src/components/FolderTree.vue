<template>
  <div class="folder-tree">
    <ul>
      <li v-for="folder in folders" :key="folder.id">
        <div
          @click="selectAndToggle(folder.id)"
          :class="{ selected: selectedFolder === folder.id }"
        >
          <span :class="{ expanded: expandedFolders.includes(folder.id) }">
            {{ folder.name }}
          </span>
        </div>
        <div v-if="expandedFolders.includes(folder.id)">
          <FolderTree
            :folders="folder.children"
            @folder-selected="selectFolder"
          />
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

interface Folder {
  id: number;
  name: string;
  children: Folder[];
}

export default defineComponent({
  name: "FolderTree",
  props: {
    folders: {
      type: Array as () => Folder[],
      required: true,
    },
  },
  emits: ["folder-selected"],
  setup(props, { emit }) {
    const expandedFolders = ref<number[]>([]);
    const selectedFolder = ref<number | null>(null);

    const toggle = (id: number) => {
      const index = expandedFolders.value.indexOf(id);
      if (index === -1) {
        expandedFolders.value.push(id);
      } else {
        expandedFolders.value.splice(index, 1);
      }
    };

    const selectAndToggle = (id: number) => {
      selectedFolder.value = id;
      emit("folder-selected", id);
      toggle(id);
    };

    const selectFolder = (id: number) => {
      selectedFolder.value = id;
      emit("folder-selected", id);
    };

    return {
      expandedFolders,
      selectedFolder,
      selectAndToggle,
      selectFolder,
    };
  },
});
</script>

<style scoped>
.expanded {
  font-weight: bold;
}

.selected {
  background-color: #e6f3ff;
}

.folder-tree ul {
  list-style: none;
  padding-left: 20px;
}

.folder-tree li div {
  cursor: pointer;
  padding: 5px;
  margin: 2px 0;
}

.folder-tree li div:hover {
  background-color: #f5f5f5;
}
</style>
