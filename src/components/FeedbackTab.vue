<template>
<div id="feedbackTab">
  <el-button size="large" @click="this.start">test</el-button>
  <el-button size="large" @click="this.natConfig">config</el-button>
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
  props: {
    createWspath: {
      type: String,
      default: "ws://localhost:8887",
    },
    serviceHost: {
      type: String,
      default: "http://localhost:8887",
    },
  },

  mounted() {
    this.createWs();
  },


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

    createWs() {
      if (typeof WebSocket === "undefined") {
        return;
      }
      this.socket = new WebSocket(this.createWspath);
      this.socket.onopen = function () {
        this.record = new Map();
        console.log("socket 创建成功");
      };
      this.socket.onerror = function (err) {
        console.log(err);
      };

      this.socket.onmessage = this.onmessage;
      this.socket.onclose = function () {
        console.log("socket 退出");
      };
    },

    start(){
      for(let i=0;i<this.editableTabs.length;i++){
         document.getElementById("text"+this.editableTabs[i].id).value=this.editableTabs[i].name+" start!!!"
      }
      this.load = true;
      let data = {
        //具体接口未定
       //topic: this.topic,
      //  groupId: this.groupId + "",
      //  start: this.queryDates[0] ? this.queryDates[0] : '',
      //  end: this.queryDates[1] ? this.queryDates[1] : '',
       // hero: "",
      };
      this.$http
        .get(this.serviceHost + "/test", data)
        .then((res) => {
          console.log(res);
        });

    },

    addTab(){
      let newTabId = ++this.tabIndex + '';
      this.editableTabs.push({
        id: newTabId,
        name: 'Router'+this.tabIndex,
        content: 'New Tab content'
      });
      this.isTip = false;
      this.activeName = newTabId;
      console.log(newTabId);
    },

    natConfig(){
      this.load = true;
      let data = {
        //具体接口未定
        //topic: this.topic,
        // hero: "",
      };
      this.$http
        .get(this.serviceHost + "/config", data)
        .then((res) => {
          console.log(res);
        });
    },

    stringTonum(a) {
      var str = a.toLowerCase().split("");
      var al = str.length;
      var getCharNumber = function (charx) {
        return charx.charCodeAt() - 96;
      };
      var numout = 0;
      var charnum = 0;
      for (var i = 0; i < al; i++) {
        charnum = getCharNumber(str[i]);
        numout += charnum * Math.pow(26, al - i - 1);
      };
      return numout;
    },

    addNotExistTab(routerName){
      let newTabId = ++this.tabIndex + '';
      this.editableTabs.push({
        id: newTabId,
        name: routerName,
        content: 'New Tab content'
      });
      this.isTip = false;
      this.activeName = newTabId;
      console.log(newTabId);
    },

    onmessage(msg) {
      var content=msg.data;
      //假设router格式为 RouterX_dddddddddddd
      var routerName=content.split("_")[0];
      var routerContent=content.split("_")[1];
      //判断是否存在 不存在就创建新的tab
      var judge =0;
      for(var i=0;i<this.editableTabs.length;i++){
        if(routerName==this.editableTabs[i].name)
        {
          judge=1;
          document.getElementById("text"+this.editableTabs[i].id).value
            =document.getElementById("text"+this.editableTabs[i].id).value+" \\n " +routerContent;
        }
      }
      if(judge==0){
         addNotExistTab(routerName);
        document.getElementById("text"+this.editableTabs[this.editableTabs.length-1].id).value
          =document.getElementById("text"+this.editableTabs[this.editableTabs.length-1].id).value+" \\n " +routerContent;
      }
      //存在就直接增加内容


    //  for(let i=0;i<this.editableTabs.length;i++){
    //    document.getElementById("text"+this.editableTabs[i].id).value=this.editableTabs[i].name+" start!!!"
    //  }
      console.log(content)
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
