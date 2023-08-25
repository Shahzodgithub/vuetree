<template>
    <div class="flex items-center justify-center tree-box">
        <tree-view />
    </div>
</template>
  
<script>
import TreeView from './components/TreeView.vue';

export default {
    components: {
        TreeView,
    },
};
</script>
  
<style>
ul li span {
    font-size: 1.3rem;
}
</style>
  