<template>
<div id="feedbackTab">
  <el-button size="large" @click="this.start">start</el-button>
  <el-button size="large" @click="this.addTab">addTab</el-button>
  <el-tabs v-model="activeName" type="card">
<!--    <el-tab-pane label="tab1" name="first" :key="'first'">
&lt;!&ndash;      <div style="background: yellow; display: inline">
        tab1内容
        tab1内容
        tab1内容
      </div>&ndash;&gt;
      <textarea id="firstText" readonly="readonly">
      </textarea>
    </el-tab-pane>

    <el-tab-pane label="tab2" name="second" :key="'second'">
      <textarea id="secondText" readonly="readonly">
      </textarea>
    </el-tab-pane>-->

    <el-tab-pane
      v-for="(item, index) in editableTabs"
      :key="item.id"
      :label="item.name"
      :name="item.id">
      <!--{{item.content}}-->
      <textarea :id="'text'+item.id" readonly="readonly" style="width:95%; height:450px;">
      </textarea>
    </el-tab-pane>
  </el-tabs>
</div>
</template>

<script>
export default {
  name: 'FeedbackTab',

  data(){
    return {
      activeName: '1',
      editableTabs: [/*{
        id: '1',
        content: 'Tab 1 content'
      }, {
        id: '2',
        content: 'Tab 2 content'
      }*/],
      tabIndex : 0,
      isTip : true,
      //默认第一个选项卡
    }
  },
  methods: {
    start(){
      for(let i=0;i<this.editableTabs.length;i++){
        document.getElementById("text"+this.editableTabs[i].id).value=this.editableTabs[i].name+" start!!!"
      }
    },

    addTab(){
      let newTabId = ++this.tabIndex + '';
      this.editableTabs.push({
        id: newTabId,
        name: 'RT'+this.tabIndex,
        content: 'New Tab content'
      });
      this.isTip = false;
      this.activeName = newTabId;
    },

    /*beforeLeaveTab() {
      if (!this.isTip) {
        this.isTip = true;
        return true;
      }

      return this.$confirm('此操作将切换tab页, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '切换成功！可以做一些其他的事情'
        });
      }).catch(() => {
        this.$message({
          type: 'success',
          message: '取消成功！可以做一些其他的事情'
        });
        throw new Error("取消成功！");
      });
    }*/
  }
}
</script>

<style scoped>
#feedbackTab{
  width: 80%;
  height: 500px;
  margin: 0 auto;
}
</style>
