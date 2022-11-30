!<!--  -->
<template>
  <div>
   <div @click="saveWorld">存储为world</div>
   <div class="echarst" ref="echarst"></div>
  </div>
</template>

<script>

import docxtemplater from 'docxtemplater'
import PizZip from 'pizzip'
import JSZipUtils from 'jszip-utils'
import ImageModule from 'docxtemplater-image-module-free'
import {saveAs} from 'file-saver'
import * as echarts from 'echarts'

function base64DataURLToArrayBuffer(dataURL) {
  const base64Regex = /^data:image\/(png|jpg|svg|svg\+xml);base64,/;
  if (
        typeof dataURL !== "string" ||
        !base64Regex.test(dataURL)
    ) {
        return false;
    }
    const stringBase64 = dataURL.replace(base64Regex, "");

    // For nodejs, return a Buffer
    if (typeof Buffer !== "undefined" && Buffer.from) {
        return Buffer.from(stringBase64, "base64");
    }

    // For browsers, return a string (of binary content) :
    const binaryString = window.atob(stringBase64);
    const len = binaryString.length;
    const bytes = new Uint8Array(len);
    for (let i = 0; i < len; i++) {
        const ascii = binaryString.charCodeAt(i);
        bytes[i] = ascii;
    }
    return bytes.buffer;
}

export default {
  components: {},
  data() {
    return {
      myEcharts: {}
    };
  },
  computed: {},
  watch: {},
  methods: {
    initEcharst () {
      var myEcharts = echarts.init(this.$refs.echarst)
      myEcharts.setOption({
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [
          {
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
          }
        ]
      });
      this.myEcharts = myEcharts
    },
    saveWorld() {
      var that = this;
      JSZipUtils.getBinaryContent('/1.docx', function ( error ,content) {
        if(error) {
          throw error;
        }
        let zip = new PizZip(content)
        let doc = new docxtemplater().loadZip(zip);
        let opts = {}
        opts.centered = true; // 图片居中，在word模板中定义方式为{%src}
        opts.fileType = "docx";
        opts.getImage = function (chartId) {
          // 解析图片，传输到world中需要转换成arraryBuffer形式才能正确传输
          return base64DataURLToArrayBuffer(chartId)
        }
        // 设置图片输出的大小
        opts.getSize = function () {
            return [500, 500]
        }
        let imageModule = new ImageModule(opts);
        // 增加对图片的处理
        console.log(arguments,'arguments')
        doc.attachModule(imageModule);
        console.log(that.myEcharts)
        //将echarts图表转换成base64格式的图片
        var img = that.myEcharts.getDataURL()

        // var img = 'data:image/png;base64,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'
        doc.setData({
          image: img,
          name: 'jjjj',
        });
        try {
          // 用模板变量的值替换所有模板变量
          doc.render();
        } catch (error) {
          // 抛出异常
        }
        let out = doc.getZip().generate({
          type: "blob",
          mimeType: "application/vnd.openxmlformats-officedocument.wordprocessingml.document"
        });
        saveAs(out, "echarst图片以及文字.docx");

      })
    }


  },
  created() {},
  mounted() {
    this.initEcharst()
  },
};
</script>
<style lang="scss" scoped>
.echarst {
  width: 800px;
  height: 800px;
}

</style>