<template>
  <div class="home">
    <div class="content flex_wrapper">
      <div class="input">
        <h4>更新简历信息：</h4>
        <textarea v-model="resume_data" rows="40"></textarea>
        <div class="info">
          <li>请按照样例中的格式更新简历信息，不要加key，不需要的key可以不写</li>
          <li>简历信息会保存在本地缓存中，清空缓存前建议保存信息</li>
        </div>
        <button v-on:click="saveData()">保存(至本地缓存)</button>
        <button v-on:click="reInit()">还原（恢复至样例数据）</button>
      </div>
      <div class="result">
        <h4>预览简历内容：</h4>
        <pre class="scroll">{{ resume_data }}</pre>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import {
  getLocalstorage,
  setLocalstorage,
  dateFormat
} from "@/services/utils.js";
import * as INITDATA from "../../resume/data.json";
export default Vue.component("custom", {
  name: "app",
  components: {
  },
  data: function() {
    return {
      layout: "",
      INITDATA: INITDATA,
      resume_data: null
    };
  },
  created: function() {
    // storage
    let local_data = getLocalstorage("resume_data");
    if (local_data) {
      this.resume_data = local_data;
    } else {
      this.resume_data = JSON.stringify(this.INITDATA, null, 4);
    }
  },
  mounted: function() {},
  methods: {
    saveData: function() {
      console.log("save", this.resume_data);
      // window.localStorage.setItem("resume_data", this.resume_data)
      setLocalstorage({
        resume_data: this.resume_data
      });
    },
    reInit: function(event) {
      this.resume_data = JSON.stringify(this.INITDATA, null, 4);
    },
    timeFormat: function(time) {
      const date = new Date(time);
      const format = "yyyy-MM-dd hh:mm";
      return dateFormat(date, format);
    }
  }
});
</script>

<style scoped>
.flex_wrapper {
  display: flex;
}
.content {
  margin: 20px;
}
.input,
.result {
  flex-basis: 50%;
  padding: 0 20px;
}
textarea {
  width: 100%;
}
pre {
  white-space: pre-wrap;
  display: block;
  padding: 8.5px;
  margin: 0 0 9px;
  font-size: 12px;
  line-height: 1.42857143;
  word-break: break-all;
  word-wrap: break-word;
  color: #f5f5f5;
  background-color: #333;
  border: 1px solid #ccc;
  border-radius: 2px;
}
.scroll {
  height: 50em;
  overflow: auto;
}
.info {
  background-color: rgb(147, 197, 228);
  box-shadow: 1px 1px 1px #ccc, -1px -1px 1px #ccc;
  padding: 1em;
  font-size: 13px;
}
</style>
