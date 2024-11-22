<script lang="ts" setup>
import Tree from "@/components/Tree.vue";
import { mockType } from "@/types"
import { ref } from "vue";

const props = defineProps<{
    title: string;
    opener: boolean;
    tree: mockType[];
    selectedFolderID?: number | null;
}>();

const emit = defineEmits(
    ['close', 'sendID']
)

const selectedID = ref<number>(props.selectedFolderID ? props.selectedFolderID : 0);

const sendID = () => {
    if(selectedID.value) {
        emit('sendID', selectedID.value)
    }
}
</script>

<template>
    <div class="backgroundModal animated06s opacityAnimation" v-if="opener">
        <div class="modal animated06s slowApear">
            <img src="@/assets/icons/close.svg" class="modal-close" @click="emit('close')"/>
            <p class="modal-title">{{ title }}</p>

            <div class="modal-tree">
                <Tree v-for="item in tree" :key="item.id" 
                :title="item.name" 
                :children="item.children" 
                :id="item.id" 
                :selectedID="selectedID"
                @select="(data) => ((selectedID) = (data))"/>
            </div>

            <button :class="{'active-button': selectedID}" @click="sendID">Ok</button>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.modal {
    margin: 50px 0;
    width: 550px;
    background: #FFFFFF;
    box-shadow: 0px 0px 14px 0px #1877F233;
    padding: 25px 40px;
    border-radius: 5px;
    display: flex;
    flex-direction: column;
    position: relative;
    &-close {
        cursor: pointer;
        position: absolute;
        top: 20px;
        right: 20px;
    }
    &-title {
        font-size: 18px;
        font-weight: 500;
    }
    &-tree {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    button {
        margin-top: 15px;
        cursor: pointer;
        background: grey;
        color: white;
        border: none;
        font-size: 14px;
        font-weight: 400;
        padding: 8px 12px;
        border-radius: 5px;
        transition: opacity 0.3s ease;
    }
    .active-button {
        background: #6eaaf8;
        transition: all 0.3s ease;
        &:hover {
            background: #5499f4;
        }
    }
}

.backgroundModal {
    overflow: auto;
    z-index: 10;
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    display: flex;
    align-items: center;
    justify-content: center;
}

.animated06s {
    animation-duration: 0.6s !important; 
    animation-fill-mode: both !important; 
}

.opacityAnimation {
    animation-name: opacityAnimation !important; 
}

.slowApear { 
    animation-name: slowApear; 
} 

@keyframes opacityAnimation {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@keyframes slowApear { 
    from {
        opacity: 0;
        transform: scale(0.5);
} 
    to { 
        opacity: 1; 
        transform: scale(1);
    } 
} 
</style>