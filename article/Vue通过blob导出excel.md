# Vue通过Blob对象实现导出Excel功能

导出 Excel 有很多方案：

1. 插件，比如vue-json-excel或者是Blob.js+Export2Excel.js来实现导出Excel功能;
2. 打开一个链接地址直接下载;
3. 通过blob对象下载；

之前前端导出excel采用的是打开一个链接地址直接下载，目前接口添加了token验证，所以excel导出采用后端返回流，前端接收后通过blob去下载。

首先上代码：

```js
<template>
	<el-button @click="downloadUrl">导出Excel</el-button>        
</template>
<script>
import api from "./api.js";
import axios from "axios";
export default {
  data() {
    return {
		pageSize:30,
	}
  },
  methods: {
    downloadUrl() {
      // console.log(api.serverUrl);
      let params = {
        pageNum: "1",
        pageSize: this.pageSize,
        companyId: JSON.parse(sessionStorage.getItem("companyId"))
      };
      this.downloadAction(params);
    },
    downloadAction(params) {
      axios
        .get(api.serverUrl + "/xxx/excel", {
          params: params,
          // 1.首先设置responseType对象格式为 blob:
          responseType: "blob"
        })
        .then(
          res => {
            let fileName = "xxx.xlsx";
            let blob = new Blob([res.data], {
              // 2.获取请求返回的response对象中的blob 设置文件类型，这里以excel为例
              type: "application/vnd.ms-excel"
            });
            if ('download' in document.createElement('a')) { // 非IE下载
              // 3.创建一个临时的url指向blob对象
              let url = window.URL.createObjectURL(blob);
              // 4.创建url之后可以模拟对此文件对象的一系列操作，例如：预览、下载
              let a = document.createElement("a");
              a.href = url;
              a.download = fileName;
              a.click();
              // 5.释放这个临时的对象url
              window.URL.revokeObjectURL(url);
            } else {  // IE10+下载
              navigator.msSaveBlob(blob, fileName);
            }
          }
        )
        .catch(error => {
          reject(error);
        });
    },
};
</script>
<style>
</style>
```

+ 'responseType'表示的是服务器响应的数据类型，可以是'arrayBuffer'、'blob'、'document'、'json'、'txt'、'stream'，默认为json ↓
+ 接收后台传给前端的二进制流之前需要先设置responseType为blob，否则默认会以json获取，下载下来的文件打开会提示文件已损坏 ↓
+ 控制台输出的可以看到是个正确的Blob对象，说明配置是OK的 ↓
+ 后端最好也要配置response头的content-type为对应的类型 ↓
+ 然后，需要给这个Blob对象设置一个type，这个type表明改Blob对象所包含数据的MIME类型。如果类型未知，则该值为空字符串 ↓
+ 在正常的导出请求之后可以看到又发了一个新的blob请求，其本质是到这个地址下载文件。

常用文件对应的MIME类型：

 扩展名----------MIME类型
 
.csv--------------text/csv

.jpeg/.jpg-------image/jpeg

.png-------------image/png

.rar--------------application/x-rar-compressed

.doc-------------application/msword

.docx-----------application/vnd.openxmlformats-officedocument.wordprocessingml.document

.xls--------------application/vnd.ms-excel

.xlsx------------application/vnd.openxmlformats-officedocument.spreadsheetml.sheet

.zip--------------application/zip

