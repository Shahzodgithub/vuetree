<template>
    <div class="max-w-sm rounded overflow-hidden shadow-lg dark:bg-gray-800 view">
        <h4 class="text-center text-white text-xl font-semibold mb-4">VueTreeView</h4>
        <ul class="max-w-md space-y-1 text-gray-500 list-none dark:text-gray-400">
            <tree-node class="item" :item="treeData" @selection-change="handleSelectionChange"
                @add-item="addItem"></tree-node>
        </ul>
    </div>
</template>
  
<script>
import TreeNode from './TreeNode.vue';

export default {
    components: {
        TreeNode,
    },
    setup() {
        const treeData = {
            name: "ROOT",
            children: [
                { name: "child 1" },
                { name: "child 2" },
                {
                    name: "child folder",
                    children: [
                        {
                            name: "child folder 1",
                            children: [{ name: "child 1" }, { name: "child 2" }]
                        },
                        { name: "child 1" },
                        { name: "child 2" },
                        {
                            name: "child folder 2",
                            children: [{ name: "child 1" }, { name: "child 2" }]
                        }
                    ]
                }
            ],
            isSelected: false,
        }
        function addItem(item) {
            item.children.push({
                name: "new child",
                isSelected: false,
            });
        }
        function handleSelectionChange(item) {
            console.log("Selection changed:", item);
        }
        return {
            addItem,
            handleSelectionChange,
            treeData
        }
    }
};
</script>
  