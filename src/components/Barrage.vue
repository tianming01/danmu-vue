<template>
  <div id="danmu">
    <div class="stage">
      <vue-baberrage
        :isShow = "barrageIsShow"
        :barrageList = "barrageList"
        :loop = "barrageLoop"
        :maxWordCount = "60"
      >
      </vue-baberrage>
    </div>
    <div class="demo-control">
      <div class="inside">
        <input type="text" style="float:left"  v-model="msg"/>
        <button type="button" style="float:left" @click="sendMsg">发送</button>
      </div>
    </div>
  </div>
</template>

<script>
  import { MESSAGE_TYPE } from 'vue-baberrage'

  export default {
    name: 'Barrage',
    data () {
      return {
        msg: '弹幕测试',
        barrageIsShow: true,
        currentId: 0,
        barrageLoop: false,
        barrageList: []
      }
    },
    methods: {
      removeList () {
        this.barrageList = []
      },
      addToList (message) {
        this.barrageList.push({
          id: ++this.currentId,
          avatar: 'https://s2.ax1x.com/2020/02/17/3PmEyd.md.png',
          msg: message,
          time: 15,
          type: MESSAGE_TYPE.NORMAL
        })
      },
      sendMsg () {
        // 发送消息到 WebSocket 服务器
        this.websocket.send(this.msg)
      }
    },
    created () {
      // 初始化 websocket 并定义回调函数
      // this.websocket = new WebSocket('ws://192.168.152.131:9599')
      this.websocket = new WebSocket('ws://danmu.qiyewei.cn/ws')
      this.websocket.onopen = function (event) {
        console.log('已建立 WebSocket 连接')
      }
      const that = this
      this.websocket.onmessage = function (event) {
        // 接收到 WebSocket 服务器返回消息时触发
        // const data = JSON.parse(event.data)
        const message = event.data
        console.log(message)
        that.addToList(message)
      }
      this.websocket.onerror = function (event) {
        console.log('与 WebSocket 通信出错')
      }
      this.websocket.onclose = function (event) {
        console.log('断开 WebSocket 连接')
      }
    },
    destroyed () {
      this.websocket.close()
    }
  }
</script>

<style scoped>
  body,html {
    margin: 0
  }

  #app {
    font-family: Avenir,Helvetica,Arial,sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50
  }

  h1,h2 {
    font-weight: 400
  }

  .stage {
    height: 800px;
    width: 100%;
    background: #025d63;
    position: relative;
    overflow: hidden
  }
  .baberrage-stage {
    top: 0;
    z-index: 5
  }

  .baberrage-stage .baberrage-item.normal {
    color: #fff
  }

  .top {
    border: 1px solid #6ab
  }

  .demo-control {
    width: 100%;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    height: 58px;
    margin: 0 auto;
    position: absolute;
    bottom: 30px;
    z-index: 99
  }

  .demo-control,.demo-control .inside {
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center
  }

  .demo-control .inside {
    background: rgba(0,0,0,.6);
    padding: 15px 5px;
    border-radius: 5px;
    border: 2px solid #8ad9ff;
    width: 80%;
    max-width: 400px;
  }

  .demo-control button,.demo-control input,.demo-controlselect {
    height: 35px;
    padding: 0;
    float: left;
    background: #00f3d8;
    border: 1px solid #71daff;
    color: #008e97;
    border-radius: 0;
    width: 15%;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    outline: none
  }

  .demo-control button {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
    border-left: 0
  }

  .demo-control button:hover {
    color: #00f3d8;
    background: #008e97;
    cursor: pointer
  }

  .demo-control input {
    width: 75%;
    height: 35px;
    background: rgba(0,0,0,.7);
    border: 1px solid #8ad9ff;
    padding-left: 15px;
    color: #fff
  }
</style>
