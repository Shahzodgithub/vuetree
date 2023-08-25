<template>
    <div>
        <li>
            <div>
                <input type="checkbox" v-model="itemData.isSelected" @click="toggleBranchSelection(itemData)" />
                <span :class="{ bold: isFolder }" @click="toggle" class="ml-1">
                    {{ itemData.name }}
                    <span v-if="isFolder">[{{ isOpen ? '-' : '+' }}]</span>
                </span>
            </div>
            <ul v-if="isOpen && isFolder" class="max-w-md space-y-1 text-gray-500 list-none dark:text-gray-400">
                <tree-node class="item" v-for="(child, index) in itemData.children" :key="index" :item="child"
                    @selection-change="$emit('selection-change', $event)" @make-folder="$emit('make-folder', $event)"
                    @add-item="$emit('add-item', $event)" />
                <li class="add" @click="$emit('add-item', itemData)">+</li>
            </ul>
        </li>
    </div>
</template>
  
<script>
import { ref, computed, watch } from "vue"
export default {
    props: {
        item: Object,
    },
    setup(props, { emit }) {
        let itemData = ref(props.item)
        const isOpen = ref(false)

        let isFolder = computed(() => {
            return itemData.value.children && itemData.value.children.length;
        })
        let isIndeterminate = computed(() => {
            if (isFolder.value) {
                const selectedChildren = itemData.value.children.filter((child) => child.isSelected);
                return selectedChildren.length > 0 && selectedChildren.length < itemData.value.children.length;
            }
            return false;
        })

        function toggle() {
            if (isFolder.value) {
                isOpen.value = !isOpen.value;
            }
        }
        function toggleBranchSelection(item) {
            item.isSelected = !item.isSelected;
            if (item.children) {
                for (const child of item.children) {
                    toggleBranchSelection(child);
                }
            }
        }
        watch(() => itemData.value.isSelected, () => {
            emit('selection-change', itemData.value);
        })
        return {
            isOpen,
            isFolder,
            isIndeterminate,
            toggle,
            toggleBranchSelection,
            itemData
        }
    }
};
</script>

  