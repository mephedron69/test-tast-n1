<script lang="ts" setup>
import Tree from "@/components/Tree.vue";
import { ref, watch, computed } from "vue";

const props = defineProps<{
    title: string;
    children?: any | null;
    level?: number;
    id?: number;
    selectedID?: number;
}>();

const emit = defineEmits(
    ['select']
)

const currentLevel = props.level || 0;
const isOpen = ref(false);

const openFunc = () => {
    if(props.children && props.children.length) {
        isOpen.value = !isOpen.value;
        emit('select', null);
    }
    else {
        emit('select', props.id);
    }
}

//////////////////////////////////////////////

const checkFullRoad = () => {
    if (props.id === props.selectedID) {
        isOpen.value = true;
    }

    const checkParents = (parentFolders: any[]) => {
        for (const item of parentFolders) {
            if (item.id === props.selectedID) {
                isOpen.value = true;
                break;
            }
            if (item.children) {
                checkParents(item.children);
            }
        }
    };

    if (props.children) {
        checkParents(props.children);
    }
}

watch(() => props.selectedID, () => {
    checkFullRoad();
}, { immediate: true });
</script>

<template>
    <div class="tree__folder" :style="{ paddingLeft: `${currentLevel * 20}px` }">
        <div class="tree__folder-main" @click="openFunc()" :class="{'active': props.id == selectedID}">
            <p>{{ title }}</p>
            <div v-if="children && children.length">
                <img src="@/assets/icons/arrow.svg" :style="isOpen ? 'rotate: 180deg;' : 'rotate: 0deg;'"/>
            </div>
        </div>
        
        <div class="tree__folder-children" v-show="isOpen && children?.length">
            <Tree 
            v-for="item in children" 
            :key="item.id" 
            :title="item.name" 
            :children="item.children"
            :id="item.id"
            :level="currentLevel + 1"
            :selectedID="selectedID"
            @select="emit('select', $event)" />
        </div>
    </div>
</template>

<style lang="scss">
.tree__folder {
    display: flex;
    flex-direction: column;
    gap: 5px;
    &-main {
        display: flex;
        align-items: center;
        justify-content: space-between;
        cursor: pointer;
        background-color: #F9F9F9;
        border: 1px solid rgba(0, 0, 0, 0.1);
        color: black;
        padding: 8px;
        border-radius: 5px;
        transition: all 0.3s ease;
        img {
            transition: all 0.3s ease;
        }
        &:hover {
            background: #1877F2;
            color: white;
            img {
                filter: brightness(0) saturate(100%) invert(93%) sepia(100%) saturate(22%) hue-rotate(148deg) brightness(106%) contrast(100%);
            }
        }
    }
    .active {
        background: #1877F2;
        color: white;
        img {
            filter: brightness(0) saturate(100%) invert(93%) sepia(100%) saturate(22%) hue-rotate(148deg) brightness(106%) contrast(100%);
        }
    }
    &-children {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
}
</style>