<script lang="ts" setup>
import Modal from "@/components/Modal.vue";
import { mockType } from "@/types"
import { ref } from "vue";

let opener = ref<boolean>(false);

const mockFolders: mockType[] = [
  {
    id: 1, name: 'Папка 1', children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3, name: 'Папка 1.2', children: [
          { id: 4, name: 'Папка 1.2.1', children: [] }
        ]
      }
    ]
  },
  { id: 5, name: 'Папка 2', children: [] },
];

let selectedFolder = ref<number>();

const getSelectedID = (value: any) => {
  selectedFolder.value = value;
  opener.value = false;
}
</script>

<template>
  <div class="home">
    <button @click="opener = true" v-if="selectedFolder">Выбранный ID: {{ selectedFolder }} </button>
    <button @click="opener = true" v-else>Открыть </button>

    <Modal 
    :opener="opener" 
    title="Выбор папок" 
    :tree="mockFolders"
    @close="(data) => (opener) = (false)"
    :selectedFolderID="selectedFolder"
    @sendID="getSelectedID"/>
  </div>
</template>

<style lang="scss" scoped>
.home {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background: #121212;

  button {
    cursor: pointer;
    background: #4338ca;
    color: white;
    border: none;
    font-size: 18px;
    font-weight: 700;
    padding: 12px 48px;
    border-radius: 10px;
    transition: opacity 0.3s ease;

    &:hover {
      opacity: 0.8;
    }
  }
}
</style>