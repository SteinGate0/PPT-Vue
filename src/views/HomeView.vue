<template>
  <div class="ppt">
      <button @click="demo('text')">Demo Text</button>
      <button @click="demo('chart')">Demo Chart</button>
      <button @click="demo('master')">Demo Master</button>
      <button @click="demo('image')">Demo Image</button>  
      <button @click="demo('table')">Demo Table</button>
  </div>
</template>
<script>
import pptxgen from 'pptxgenjs';
import { genSlides_Chart } from "@/assets/modules/demo_chart.mjs";
import { genSlides_Master } from "@/assets/modules/demo_master.mjs";
import { genSlides_Image } from "@/assets/modules/demo_image.mjs";
import { genSlides_Table } from "@/assets/modules/demo_table.mjs"
import { genSlides_Text } from "@/assets/modules/demo_text.mjs"
export default {
  methods: {
    demo(type) {
        // 创建一个新的PPT文档
        let pptx = new pptxgen();
        pptx.defineLayout({ name: 'A4', width: 13.5, height: 7.7 })
        pptx.layout = 'A4'
        // // 添加幻灯片
        // let slide = pptx.addSlide();
        // // 向幻灯片中添加文本
        // slide.addText('Hello World!', { x: 1, y: 1, fontSize: 18, color: '363636' });
        // // 保存PPT文档
        // pptx.writeFile('example.pptx');
        // 添加包含图表的幻灯片
        switch (type) {
            case 'text':
                genSlides_Text(pptx);
                break;
            case 'chart':
                genSlides_Chart(pptx);
                break;
            case 'master':
                genSlides_Master(pptx);
                break;
            case 'image':
                genSlides_Image(pptx);
                break;
            case 'table':
                genSlides_Table(pptx);
                break;
            default:
                break;
        }
          
        // 保存 PowerPoint 文件
        pptx.writeFile("demo.pptx", (error) => {
            if (error) {
                console.log("保存文件时出现错误：", error);
            } else {
                console.log("文件保存成功！");
            }
        });
    },
  }
}
</script>
<style scoped>
.ppt {
  display: flex;
  justify-content: center;
  align-items: center;
}

button {
  display: inline-block;
  margin: 0 20px;
  padding: 10px 20px;
  font-size: 16px;
  color: white;
  text-align: center;
  text-decoration: none;
  border: none;
  border-radius: 4px;
  background: linear-gradient(90deg, #005BFF, #00ADEE);
  background-size: 200% 200%;
  animation: Gradient 3s ease infinite;
  cursor: pointer;
}

@keyframes Gradient {
  0% {
      background-position: 0% 50%;
  }

  50% {
      background-position: 100% 50%;
  }

  100% {
      background-position: 0% 50%;
  }
}
</style>