<template>
  <div>
    <pro-search :items="searchItems" :data-source="dataSource" />
  </div>
</template>
<script>
import ProSearch from '@vue-pro/search';
export default {
  name: 'App',
  components: {
    ProSearch,
  },
  data() {
    return {
      dataSource: {},
    };
  },
  computed: {
    searchItems() {
      return [
        {
          type: 'input',
          label: '公司',
          prop: 'clueName',
        },
        {
          type: 'input',
          label: '姓名',
          prop: 'conntrName',
        },
        {
          type: 'select',
          label: '跟进状态',
          prop: 'followStatus',
          options: [{ label: '确定', value: 1 }],
        },
      ];
    },
  },
};
</script>

<style></style>
