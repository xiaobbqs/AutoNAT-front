<template>
  <div id="feedbackTab">
    <el-button size="large" @click="this.start">test</el-button>
    <el-button size="large" @click="this.natConfig">config</el-button>
    <el-tabs v-model="activeName" type="card">
      <el-tab-pane
        v-for="(item, index) in editableTabs"
        :key="item.id"
        :label="item.name"
        :name="item.id">
        <!--{{item.content}}-->
        <textarea :id="'text'+item.id" readonly="readonly" style="width:95%; height:450px;"
                  onpropertychange="this.scrollTop = this.scrollHeight">
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


  data() {
    return {
      activeName: '1',
      editableTabs: [],
      tabIndex: 0,
      isTip: true,
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

    start() {
      this.load = true;
      let data = {};
      this.socket.send("test_NAT")
    },

    addTab() {
      let newTabId = ++this.tabIndex + '';
      this.editableTabs.push({
        id: newTabId,
        name: 'Router' + this.tabIndex,
        content: 'New Tab content'
      });
      this.isTip = false;
      this.activeName = newTabId;
      console.log(newTabId);
    },

    natConfig() {
      this.load = true;
      this.socket.send("config_NAT")
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
      }
      ;
      return numout;
    },

    addNotExistTab(routerName) {
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

      var content = msg.data;
      console.log(content);
      //假设router格式为 RouterX_dddddddddddd
      let data = JSON.parse(content)
      if (data.result) {
        alert(data.result);
        return;
      }

      var routerName = data.device
      console.log(routerName);
      var routerContent = data.output
      console.log(data["command"])
      console.log(data.output)

      //判断是否存在 不存在就创建新的tab
      var judge = 0;
      for (var i = 0; i < this.editableTabs.length; i++) {
        if (routerName == this.editableTabs[i].name) {
          judge = 1;
          if (document.getElementById("text" + this.editableTabs[i].id).value) {
            document.getElementById("text" + this.editableTabs[i].id).value
              = document.getElementById("text" + this.editableTabs[i].id).value + " " + routerContent;
            console.log(document.getElementById("text" + this.editableTabs[i].id).value)
          } else {
            document.getElementById("text" + this.editableTabs[i].id).value = routerContent;
          }
        }
      }
      if (judge == 0) {
        this.addNotExistTab(routerName);
        document.getElementById("text" + this.editableTabs[this.editableTabs.length - 1].id).value = routerName + "> " + routerContent;
      }
      //存在就直接增加内容
      console.log(content)
    },
  }
}
</script>

<style scoped>
  #feedbackTab {
    width: 80%;
    height: 500px;
    margin: 0 auto;
  }
</style>
