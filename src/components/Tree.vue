<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
     <Tree :value="nodes" class="w-full md:w-30rem"></Tree>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import Tree from 'primevue/tree';
import { services }  from '../data/tree.json';

// eslint-disable-next-line no-unused-vars
const rawData = ref(services);

console.log(services.value);
const nodes = computed(() => {
  // eslint-disable-next-line no-unused-vars
  const nodeMap = new Map();
  const roots = [];

  rawData.value.forEach(service => {
    const node = {
      key: service.id,
      label: `${service.name} ${+service.price != 0 ?  `(${service.price})` : ""}`,
      children: [],
      leaf: service.node ===  0
    };
    nodeMap.set(service.id, node);
    if (!service.head) {
      roots.push(node);
    } else {
      const parent = nodeMap.get(service.head);
      if (parent) {
        parent.children.push(node);
      }
    }
  });

  return roots;
});
</script>

<style lang="scss" scoped>
</style>