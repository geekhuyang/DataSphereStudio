<template>
  <div class="we-directory">
    <Input
      v-model="unPrefixPath"
      disabled
      class="we-directory-input"></Input>
    <div
      :class="{'is-hide':isHide}"
      class="we-directory-tree"
      :style="{'height': height + 'px'}">
      <Spin
        v-if="!(tree && tree.length)"
        size="large"
        fix/>
      <we-tree
        ref="tree"
        :data="tree"
        :node-props="nodeProps"
        :sort-fn="sortFn"
        :filter-node="filterNode"
        :load-data-fn="loadDataFn"
        :is-root-default-open="isRootDefaultOpen"
        @node-click="benchClick"/>
    </div>
  </div>
</template>
<script>
import { orderBy } from 'lodash';
export default {
  name: 'DirectoryDialog',
  props: {
    tree: {
      type: Array,
      require: true,
      default: () => [],
    },
    isHide: {
      type: Boolean,
      default: false,
      require: false,
    },
    path: {
      type: String,
      require: true,
      default: '',
    },
    fsType: {
      type: String,
      require: true,
      default: '',
    },
    filterNode: Function,
    loadDataFn: Function,
    isRootDefaultOpen: {
      type: Boolean,
      defalut: false,
    },
    height: {
      type: Number,
      default: 120,
    },
    filterText: {
      type: String,
      default: '',
    },
  },
  data() {
    return {
      nodeProps: {
        children: 'children',
        label: 'name',
        icon: 'icon',
        isLeaf: 'isLeaf',
      },
    };
  },
  computed: {
    unPrefixPath() {
      return this.path && this.path.slice(7, this.path.length);
    },
  },
  watch: {
    filterText() {
      this.$refs.tree.store.filter();
    },
  },
  methods: {
    benchClick(...args) {
      this.$emit('set-node', args[1].node.data, this.fsType);
    },
    sortFn(data) {
      return orderBy(data, ['isLeaf', 'label'], ['asc', 'asc']);
    },
  },
};
</script>
<style lang="scss" src="./index.scss">
</style>
