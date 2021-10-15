<script setup>
import { defineProps } from 'vue'

const props = defineProps({
  list: {
    type: Array,
    default: []
  }
})

const emit = defineEmits(['showDetail'])

function showDetail (item) {
  emit('showDetail', item)
}
</script>

<template>
  <div>
    <ul>
      <li v-for="(item, index) in props.list" :key="index" @click="showDetail(item)">
        <p>{{item['部门名称']}}-{{item['招考人数']}}-{{item['面试人员比例']}}</p>
      </li>
    </ul>
  </div>
</template>

<style lang="less" scoped>
li {
  list-style: none;
  cursor: pointer;
}
li:nth-child(odd) {
  background: #eee;
}
</style>