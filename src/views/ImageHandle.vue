<script setup>
import { ref } from "vue";
import axios from "axios";

// 与后端对接将这段解注释
// import XLSX from 'xlsx';
// 上面这一段

const BASE_URL = 'xxx.xxx.xx.x:xxxx'


// static data
let list = []
for(let i=0;i<20;++i){
  list.push({
    x:1,
    y:1
  })
}
const staticImgURL = '/public/img/piclook.png'
const staticExcelURL = '/public/excel/output.xlsx'
const outputFileName = 'output.xlsx'
// dynamic data
const excelURL = ref(undefined)
const tableData = ref(undefined)
const status = ref('未上传图片')
// file upload
const imgUpload = ref()
let img = ref()

const upload = (e) => {
  status.value = '图片上传中'
  let file = e.target.files[0]
  if(file){
    // replace img with the file
    img.value.src = window.URL.createObjectURL(file)
    // create form data
    let formData = new FormData()
    formData.append('file', file)
    status.value = '图片上传成功'
  }else{
    alert('获取图片失败')
    imgClear()
  }
}
const imgHandle = async () => {
  // submit to backend

  // 与后端对接将这一段解注释
  // const res = await axios.post(BASE_URL+'/api/img', {
  //   data: formData
  // })
  // json_res = JSON.parse(res.data)
  // tableData.value = json_res['tableData']
  // excelURL.value = json_res['excelURL']
  // 就上面这一段

  // 这一段提供静态数据，将其注释
  tableData.value = list
  excelURL.value = staticExcelURL
  status.value = '成功提取数据'
  // 就上面这一段
}
const imgClear = () => {
  tableData.value = undefined
  status.value = '未上传图片'
  img.value.src = staticImgURL

}

const excelDownload = ref()

const excelOutput = () => {
  // 与后端对接将这一段解注释
  // let workbook = XLSX.utils.book_new()
  // const sheet = XLSX.utils.json_to_sheet(list)
  // XLSX.utils.book_append_sheet(workbook, sheet, 'Sheet')
  // XLSX.writeFile(workbook, 'output.xlsx')
  // 就上面这一段

  excelDownload.value.click()

}
</script>

<template>
  <div class="img">
    <div class="img-add">
      <input type="file" formmethod="post" name="img"
             accept="image/*" hidden
             ref="imgUpload" @change="upload($event)" >
      <img class="img-add-png" :src="staticImgURL"
            ref="img" @click="imgUpload.click">
      <button v-if="status==='未上传图片'"
          class="img-add-button" @click="imgUpload.click">
        点击上传需要识别的图片
      </button>
      <button v-if="status==='图片上传成功'"
          class="img-add-button" @click="imgHandle">
        提取图片数据
      </button>
      <button v-if="status==='成功提取数据'"
              class="img-add-button" @click="imgClear">
        清空
      </button>
    </div>
    <div class="img-op">
      <div :style="{color: status==='未上传图片'?'red':
      status==='图片上传成功'||status==='成功提取数据'?'green':''}"
          class="img-op-status">
        {{ status }}
      </div>
      <div class="img-op-output">
        <el-table
            v-if="status==='成功提取数据'"
            :data="tableData" height="240" class="table"
            :cell-style="{ textAlign: 'center' }"
            :header-cell-style="{ 'text-align': 'center' }">
          <el-table-column prop="x" label="x坐标" />
          <el-table-column prop="y" label="y坐标" />
        </el-table>
        <button
            v-if="tableData"
            class="img-op-output-button" @click="excelOutput">
          点击导出excel文件
        </button>
        <a ref="excelDownload" :href="excelURL" :download="outputFileName"
           target="_blank"  hidden></a>
      </div>
    </div>
  </div>
</template>

<style lang="less" scoped>
@import "@/assets/css/img-handle";
</style>
