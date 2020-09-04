<template>
  <div>
    <h1>LibreSpeed Example</h1>
    <!-- <div id="startStopBtn" @click="startStop" /> -->
    <button id="startStopBtn" type="button" @click="startStop">
      test
    </button>
    <div id="serverId" />
    <div id="test">
      <div class="testGroup">
        <div class="testArea">
          <div class="testName">
            Download
          </div>
          <div id="dlText" class="meterText" />
          <div class="unit">
            {{ dSpeed }} Mbps
          </div>
        </div>
        <div class="testArea">
          <div class="testName">
            Upload
          </div>
          <div id="ulText" class="meterText" />
          <div class="unit">
            {{ uSpeed }} Mbps
          </div>
        </div>
      </div>
      <div class="testGroup">
        <div class="testArea">
          <div class="testName">
            Ping
          </div>
          <div id="pingText" class="meterText" />
          <div class="unit">
            ms
          </div>
        </div>
        <div class="testArea">
          <div class="testName">
            Jitter
          </div>
          <div id="jitText" class="meterText" />
          <div class="unit">
            ms
          </div>
        </div>
      </div>
      <div id="ipArea">
        IP Address:
        <span id="ip" />
      </div>
    </div>
    <a href="https://github.com/librespeed/speedtest">Source code</a>
  </div>
</template>

<script>
export default {
  data () {
    return {
      s: null,
      SPEEDTEST_SERVERS: [
        {
        // this is my demo server, remove it
          name: 'Speedtest Demo Server (Helsinki)', // user friendly name for the server
          server: '//fi.openspeed.org/', // URL to the server. // at the beginning will be replaced with http:// or https:// automatically
          dlURL: 'garbage.php', // path to download test on this server (garbage.php or replacement)
          ulURL: 'empty.php', // path to upload test on this server (empty.php or replacement)
          pingURL: 'empty.php', // path to ping/jitter test on this server (empty.php or replacement)
          getIpURL: 'getIP.php' // path to getIP on this server (getIP.php or replacement)
        }
      ],
      uSpeed: 0,
      dSpeed: 0
    }
  },
  mounted () {
    /* eslint-disable */
    window.addEventListener("load", () => {
      this.s = new Speedtest();
      this.loadServers();
      this.s.onupdate=function(data){ //callback to update data in UI
      this.uSpeed = (data.testState==3&&data.ulStatus==0)?"...":data.ulStatus;
      this.dSpeed = (data.testState==1&&data.dlStatus==0)?"...":data.dlStatus;
    // this.I("ip").textContent=data.clientIp;
    // this.I("dlText").textContent=(data.testState==1&&data.dlStatus==0)?"...":data.dlStatus;
    // this.I("ulText").textContent=(data.testState==3&&data.ulStatus==0)?"...":data.ulStatus;
    // this.I("pingText").textContent=data.pingStatus;
    // this.I("jitText").textContent=data.jitterStatus;
    }
    this.s.onend=function(aborted){ //callback for test ended/aborted
        this.I("startStopBtn").className=""; //show start button again
        if(aborted){ //if the test was aborted, clear the UI and prepare for new test
        this.initUI();
        }
    }
    });
    alert('aaaa');
  },
  methods: {
    selectServer() {
      //called after loading server list
      this.s.selectServer(function (server) {
        // run server selection. When the server has been selected, display it in the UI
        // this.I("startStopBtn").style.display = ""; //show start/stop button again
        // this.I("serverId").textContent = server.name; //show name of test server
      });
    },
    loadServers() {
      alert(this.s);
      //called when the page is fully loaded
      // this.I("startStopBtn").style.display = "none"; //hide start/stop button during server selection
      if (typeof this.SPEEDTEST_SERVERS === "string") {
        //load servers from url
        this.s.loadServerList(this.SPEEDTEST_SERVERS, function (servers) {
          //list loaded
          this.SPEEDTEST_SERVERS = servers;
          this.selectServer();
        });
      } else {
        //hardcoded list of servers, already loaded
        this.s.addTestPoints(this.SPEEDTEST_SERVERS);
        this.selectServer();
      }
    },
    startStop() {
      //start/stop button pressed
      // if (this.s.getState() == 3) {
        //speedtest is running, abort
        // this.s.abort();
      // } else {
        //test is not running, begin
        // this.s.start();
        // alert(this.s);
        // this.I("startStopBtn").className = "running";
      // }
    },
    //function to (re)initialize UI
    initUI() {
      this.I("dlText").textContent = "";
      this.I("ulText").textContent = "";
      this.I("pingText").textContent = "";
      this.I("jitText").textContent = "";
      this.I("ip").textContent = "";
    },
    I (id) {
      return document.getElementById(id);
      }
  },
  head: {
    script: [
      { src: "speedtest.js" },
      { src: "speedtest_worker.js" },
    ],
  },
};
</script>

<style scorped>
h1 {
  color: #404040;
}
#startStopBtn {
  display: inline-block;
  margin: 0 auto;
  color: #6060aa;
  background-color: rgba(0, 0, 0, 0);
  border: 0.15em solid #6060ff;
  border-radius: 0.3em;
  transition: all 0.3s;
  box-sizing: border-box;
  width: 8em;
  height: 3em;
  line-height: 2.7em;
  cursor: pointer;
  box-shadow: 0 0 0 rgba(0, 0, 0, 0.1), inset 0 0 0 rgba(0, 0, 0, 0.1);
}
#startStopBtn:hover {
  box-shadow: 0 0 2em rgba(0, 0, 0, 0.1), inset 0 0 1em rgba(0, 0, 0, 0.1);
}
#startStopBtn.running {
  background-color: #ff3030;
  border-color: #ff6060;
  color: #ffffff;
}
#startStopBtn:before {
  content: "Start";
}
#startStopBtn.running:before {
  content: "Abort";
}
#test {
  margin-top: 2em;
  margin-bottom: 12em;
}
div.testArea {
  display: inline-block;
  width: 14em;
  height: 9em;
  position: relative;
  box-sizing: border-box;
}
div.testName {
  position: absolute;
  top: 0.1em;
  left: 0;
  width: 100%;
  font-size: 1.4em;
  z-index: 9;
}
div.meterText {
  position: absolute;
  bottom: 1.5em;
  left: 0;
  width: 100%;
  font-size: 2.5em;
  z-index: 9;
}
#dlText {
  color: #6060aa;
}
#ulText {
  color: #309030;
}
#pingText,
#jitText {
  color: #aa6060;
}
div.meterText:empty:before {
  color: #505050 !important;
  content: "0.00";
}
div.unit {
  position: absolute;
  bottom: 2em;
  left: 0;
  width: 100%;
  z-index: 9;
}
div.testGroup {
  display: inline-block;
}
@media all and (max-width: 65em) {
  body {
    font-size: 1.5vw;
  }
}
@media all and (max-width: 40em) {
  body {
    font-size: 0.8em;
  }
  div.testGroup {
    display: block;
    margin: 0 auto;
  }
}
</style>
