<template>
  <div id="topoGraph">
    <img src="./topo.png" id="graph">
    <br>
    <a id="topoFont">拓扑图</a>
  </div>
</template>

<script>
export default {
  name: 'TopoGraph'
}
</script>

<style scoped>
#graph{
  width: 80%;
  height: 500px;
  margin: 0 auto;
}
#topoFont{
  font-size: 40px
}
</style>
