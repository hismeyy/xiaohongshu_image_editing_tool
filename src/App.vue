<script setup>
import {ref} from 'vue';
import { read, utils } from 'xlsx';

const codeContent = ref('');

const generateImage = () => {
  let canvas = document.querySelector('.img');

  // 如果 canvas 不存在，则创建一个新的 canvas 元素
  if (!canvas) {
    const canvasContainer = document.querySelector('.imgs');
    canvas = document.createElement('canvas');
    canvas.className = 'img'; // 设置 class
    canvas.width = 1080;
    canvas.height = 1440;
    canvas.style.marginLeft = '40px'; // 添加样式
    canvas.style.marginRight = '40px'; // 添加样式
    canvas.style.width = '450px'; // 添加样式
    canvas.style.height = '600px'; // 添加样式
    canvas.style.border = '1px solid #000'; // 添加样式
    canvasContainer.appendChild(canvas);
  }

  const ctx = canvas.getContext('2d');

  let canvas_width = 1080
  let canvas_height = 1440

  // 已知
  // canvas
  // ctx
  // canvas_width
  // canvas_height


  // 清空 canvas
  ctx.clearRect(0, 0, canvas_width, canvas_height);

  // 执行文本域中的 JavaScript 代码
  eval(codeContent.value);
};

const handleImportExcel = () => {
  const input = document.createElement('input');
  input.type = 'file';
  input.accept = '.xlsx, .xls';
  input.onchange = (e) => {
    const file = e.target.files[0];
    const reader = new FileReader();
    reader.onload = (event) => {
      const data = new Uint8Array(event.target.result);
      const workbook = read(data, { type: 'array' });
      const firstSheetName = workbook.SheetNames[0];
      const sheet = workbook.Sheets[firstSheetName];
      const sheetData = utils.sheet_to_json(sheet, { header: 1 });
      console.log('Excel Sheet Data:', sheetData);
      // 如果需要逐行处理数据，可以在这里添加逻辑
    };
    reader.readAsArrayBuffer(file);
  };
  input.click();
};
</script>


<template>
  <div class="grid-demo-background">
    <a-space direction="vertical" :size="16" style="display: block;">
      <a-row class="grid-demo">
        <a-col :span="4">
          <div class="box">
            <div id="logo-title">
              <a-space size="large">
                <a-avatar :size="40" shape="square">
                  <img
                      alt="logo"
                      src="https://upload.wikimedia.org/wikipedia/commons/c/c1/XiaohongshuLOGO.png"
                  />
                </a-avatar>
                <h2>XHSImgTool</h2>
              </a-space>
            </div>
            <div style="margin-top: 30px">
              <a-space>
                <a-button type="primary" @click="handleImportExcel">导入Excel</a-button>
                <a-button type="primary">导出图片</a-button>
              </a-space>
            </div>
            <div id="code" style="margin-top: 15px">
              <textarea id="code-box" v-model="codeContent"/>
            </div>
            <div id="code" style="margin-top: 5px">
              <a-space>
                <a-button type="primary" @click="generateImage">生成图片</a-button>
              </a-space>
            </div>
          </div>
        </a-col>
        <a-col :span="20">
          <div class="box">
            <div class="imgs">
            </div>
          </div>
        </a-col>
      </a-row>
    </a-space>
  </div>
</template>

<style scoped>
.box {
  padding: 20px;
}

#code-box {
  width: 100%;
  height: 675px;
  resize: none; /* 禁用调整大小 */
  padding: 10px;
  background-color: #F2F3F5;
  border: 1px solid #F2F3F5; /* 默认边框颜色 */
  outline: none; /* 移除默认的聚焦边框 */
}

#code-box:focus {
  border-color: #165DFF; /* 选中时的边框颜色 */
}

/* 滚动条整体 */
#code-box::-webkit-scrollbar {
  width: 3px; /* 滚动条宽度 */
  height: 3px; /* 滚动条高度（对于水平滚动条） */
}

/* 滚动条轨道 */
#code-box::-webkit-scrollbar-track {
  background: #F2F3F5; /* 轨道背景颜色 */
}

/* 滚动条滑块 */
#code-box::-webkit-scrollbar-thumb {
  background: #165DFF; /* 滑块颜色 */
}

/* 滚动条滑块在悬停状态时 */
#code-box::-webkit-scrollbar-thumb:hover {
  background: #165DFF; /* 悬停状态滑块颜色 */
}

/* 滚动条整体 */
.imgs::-webkit-scrollbar {
  width: 3px; /* 滚动条宽度 */
  height: 5px; /* 滚动条高度（对于水平滚动条） */
}

/* 滚动条轨道 */
.imgs::-webkit-scrollbar-track {
  background: #F2F3F5; /* 轨道背景颜色 */
}

/* 滚动条滑块 */
.imgs::-webkit-scrollbar-thumb {
  background: #165DFF; /* 滑块颜色 */
}

/* 滚动条滑块在悬停状态时 */
.imgs::-webkit-scrollbar-thumb:hover {
  background: #165DFF; /* 悬停状态滑块颜色 */
}


.imgs {
  margin: 90px 20px 20px;
  border: 1px solid black;
  height: 82vh;
  white-space: nowrap; /* 防止换行 */
  overflow-x: auto; /* 水平滚动条 */
  overflow-y: hidden; /* 禁止垂直滚动条 */
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
}

.img {
  margin-left: 40px;
  margin-right: 40px;
  width: 450px;
  height: 600px;
  border: 1px solid #000; /* 默认边框颜色 */
}

</style>
