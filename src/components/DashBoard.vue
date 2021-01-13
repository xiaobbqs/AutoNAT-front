<template>
  <div id="board">
    <el-row>
      <el-col :span="10">
        <topo></topo>
      </el-col>
      <el-col :span="10">
        <tab></tab>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import topo from './TopoGraph'
import tab from './FeedbackTab'

export default {
  name: 'DashBoard',
  components: {topo, tab}
}
</script>

<style scoped>
  #board {
    margin-top: 150px;
    margin-right: 0px;
  }
</style>
