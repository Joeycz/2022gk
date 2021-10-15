<script setup lang="ts">
import config from './config.json'
import data1 from './assets/2022/1.json'
import data2 from './assets/2022/2.json'
import data3 from './assets/2022/3.json'
import data4 from './assets/2022/4.json'
import { onMounted, ref, watch } from 'vue'
import List from './List.vue'

const drawer = ref(false)
const drawerData = ref([])

const tabList = ref([
  {
    label: config['1'],
    name: config['1'],
    data: data1
  },
  {
    label: config['2'],
    name: config['2'],
    data: data2
  },
  {
    label: config['3'],
    name: config['3'],
    data: data3
  },
  {
    label: config['4'],
    name: config['4'],
    data: data4
  }
])

const tabListClone = ref([
  {
    label: config['1'],
    name: config['1'],
    data: data1
  },
  {
    label: config['2'],
    name: config['2'],
    data: data2
  },
  {
    label: config['3'],
    name: config['3'],
    data: data3
  },
  {
    label: config['4'],
    name: config['4'],
    data: data4
  }
])
const activeName = ref(config['1'])
const filterOptions = {
  place: {
    list: [
      {
        value: '',
        name: '无',
        label: '无'
      },
      {
        value: '北京',
        name: '北京',
        label: '北京'
      },
      // {
      //   value: '天津',
      //   name: '天津',
      //   label: '天津'
      // }
    ]
  },
  xueli: {
    list: [
      {
        value: '',
        name: '无',
        label: '无'
      },
      {
        value: '本科',
        name: '本科',
        label: '本科'
      },
      {
        value: '大专',
        name: '大专',
        label: '大专'
      }
    ]
  }
}
const place = ref('北京')
const xueli = ref('本科')

watch(place, (val) => {
  filterData()
})

watch(xueli, (val) => {
  console.log(val)
  filterData()
})

function filterData () {
  console.log('-->', place.value)
  tabListClone.value.forEach((item, index) => {
    tabList.value[index].data = item.data.filter(ite => {
      return (!place.value || ite['工作地点'].match(new RegExp(place.value)))
        && ite['学历'].match(new RegExp(xueli.value))
    })
  })
}

function handleShowDetail (data: any) {
  drawer.value = true
  drawerData.value = Object.entries(data)
}

onMounted(() => {
  filterData()
})
</script>

<template>
  <div class="filter-box">
    <div class="filter-item">
      <p>工作地点</p>
      <el-select v-model="place" placeholder="工作地点">
        <el-option
          v-for="item in filterOptions.place.list"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
    </div>
    <div class="filter-item">
      <p>学历</p>
      <el-select v-model="xueli" placeholder="学历">
        <el-option
          v-for="item in filterOptions.xueli.list"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
    </div>
  </div>
  <el-tabs class="gktab" v-model="activeName">
    <el-tab-pane
      v-for="(item, index) in tabList"
      :key="index"
      :label="item.label + `(${item.data.length})`"
      :name="item.name">
      <List :list="item.data" @showDetail="handleShowDetail"></List>
    </el-tab-pane>
  </el-tabs>

  <el-drawer
    v-model="drawer"
    direction="rtl"
    size="50%"
    custom-class="gkdrawer"
  >
    <div class="detail-box">
      <div class="detail-item" v-for="(item, index) in drawerData" :key="index">
        <div class="item-tit">{{item[0]}}：</div>
        <div class="item-con" v-if="item[0] === '部门网站'">
          <a :href="`http://${item[1]}`" target="_blank" rel="noopener noreferrer">{{item[1]}}</a>
        </div>
        <div class="item-con" v-else>{{item[1]}}</div>
      </div>
    </div>
  </el-drawer>
</template>

<style lang="less">
.gktab .el-tabs__header {
  position: sticky;
  top: 0;
  left: 0;
  background: white;
  z-index: 999;
}
.gkdrawer {
  .el-drawer__body {
    height: calc(100vh - 77px);
    box-sizing: border-box;
  }
  .detail-box {
    height: 100%;
    overflow: scroll;
  }
  .detail-item {
    display: flex;
    align-items: center;
    padding: 5px 0;
    .item-tit {
      flex-shrink: 0;
      width: 100px;
      margin-right: 16px;
      color: #666;
    }
    .item-con {
      font-weight: bold;
    }
  }
  .detail-item + .detail-item {
    border-top: 1px solid var(--el-border-color-base);
  }
}
.filter-box {
  display: flex;
  .filter-item + .filter-item {
    margin-left: 16px;
  }
}
</style>