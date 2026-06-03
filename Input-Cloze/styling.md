# CSS

```css
/* ==========================================================================
   1. 全局基础与卡片盒（空间余量优化）
   ========================================================================== */
.card {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
  background-color: #f5f7fa;
  margin: 0;
  padding: 20px;
}

.card-container {
  position: relative;
  background: #ffffff;
  max-width: 600px;
  margin: 30px auto;
  /* 🟢 优化：将 padding-top 从 40px 提升至 60px。
     因为顶部有两个绝对定位的徽章，必须留出足够的安全空间，否则正文首行会与徽章重叠。 */
  padding: 60px 30px 30px 30px;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  border: 1px solid #eef2f5;
  box-sizing: border-box; /* 确保 padding 不撑大盒子整体宽度 */
}

/* ==========================================================================
   2. 顶层徽章位置优化（解决左右错位与重叠）
   ========================================================================== */

/* 🟢 优化：修改为真正的右上/左上对称。
   原代码中 .unit-badge 写的 left: 15px; top: 5px; 显得太靠顶，且字号(15px)比 Topics 还要大，喧宾夺主。 */
.unit-badge {
  position: absolute;
  top: 16px;
  left: 20px;
  background-color: #e6f0fa;
  color: #007bff;
  font-size: 12px;       /* 调整至 12px，与 Topics 保持一致的视觉层级 */
  font-weight: bold;
  padding: 4px 12px;     /* 微调内边距，让胶囊形状更圆润 */
  border-radius: 20px;
  letter-spacing: 0.5px;
  line-height: 1;
}

/* 🟢 优化：原代码写 top: 25px; 会导致它往下压，如果首行文本较长，会直接撞上这个徽章。
   现在将其与 Unit 徽章在顶端（top: 16px）完全对齐。 */
.topics-badge {
  position: absolute;
  top: 16px;
  right: 20px;
  background-color: #f1f5f9; /* 换成稍淡的颜色，避免双徽章都是蓝色造成视觉混乱 */
  color: #64748b;
  font-size: 12px;
  font-weight: bold;
  padding: 4px 12px;
  border-radius: 20px;
  letter-spacing: 0.5px;
  line-height: 1;
}

/* ==========================================================================
   3. 主文本与行内组件（核心垂直中线对齐）
   ========================================================================== */

.content-box {
  font-size: 19px;
  color: #2c3e50;
  /* 🟢 优化：行高从 1.8 提升至 2.0。
     行内输入框和结果框是有厚度（padding/border）的，行高太低会导致换行时，上下行的输入框挤在一起。 */
  line-height: 2.0; 
  text-align: left;
  word-break: break-word; /* 防止长单词、特定符号撑破布局 */
}

/* 📥 正面输入框 - 优化版 */
.custom-type-input {
  display: inline-block;
  /* 🟢 优化：必须加 vertical-align: middle; 否则输入框会比前后文字矮半截或高半截。 */
  vertical-align: middle; 
  position: relative;
  top: -1px; /* 🟢 视觉补偿：微调 1px 消除输入框底边框带来的下坠感 */
  border: none;
  border-bottom: 2px solid #cbd5e1;
  background-color: #f8fafc;
  padding: 2px 2px;
  font-size: 18px;
  color: #2c3e50;
  
  /* 🔧 核心优化：移除固定宽度，使用自适应宽度 */
  width: fit-content; /* 或使用 width: fit-content; */
  min-width: 20px; /* 设置最小宽度，避免内容为空时过窄 */
  max-width: 60%; /* 设置最大宽度，防止过长内容撑破布局 */
  
  text-align: center;
  border-radius: 4px 4px 0 0;
  margin: 0 2px; /* 稍微拉开左右间距 */
  box-sizing: border-box;
  transition: border-color 0.2s ease, background-color 0.2s ease;
  
  /* 🔧 增强优化：内容改变时自动调整宽度 */
  resize: none; /* 禁用用户手动调整大小 */
}

.custom-type-input:focus {
  outline: none;
  border-bottom-color: #007bff;
  background-color: #e6f0fa;
}

/* 📥 提示词标签 */
.cloze-hint-badge {
  font-size: 14px !important;
  color: #94a3b8 !important; /* 降低一点亮度，使其成为辅助视觉 */
  font-weight: normal !important;
  display: inline-block !important;
  vertical-align: middle; /* 🟢 与输入框保持同一中线对齐 */
  margin-left: 1px;       /* 🟢 缩短左边距，让它视觉上紧跟属于它的输入框 */
  margin-right: 1px;      /* 拉开右边距，与后面的正常句子隔开 */
  position: relative;
  top: -1px;
}

/* 📤 背面红绿对比包裹盒 */
.multi-type-result {
  display: inline-block;
  vertical-align: middle; /* 🟢 保持中线对齐 */
  background-color: #f1f5f9;
  padding: 2px 2px;
  border-radius: 2px;
  font-family: Consolas, "Liberation Mono", monospace;
  font-size: 18px;
  margin: 1 1px;
}

/* ==========================================================================
   4. 红绿校验文本细节
   ========================================================================== */
.typeGood, .typeBad, .typeMissed {
  display: inline-block;
  padding: 0 2px; /* 🟢 稍微增加左右内边距，让有颜色的字母不显得拥挤 */
  border-radius: 3px;
  line-height: 1.3;
}
.typeGood { color: #1e7e34; background-color: #d4edda; font-weight: bold; }
.typeBad { color: #bd2130; background-color: #f8d7da; text-decoration: line-through; }
.typeMissed { color: #d39e00; background-color: #fff3cd; font-style: italic; font-weight: bold; }

/* ==========================================================================
   5. 底部补充解析区（间距黄金比例微调）
   ========================================================================== */
.extra-divider {
  margin: 35px 0 20px 0; /* 🟢 适当拉大正文与解析区的距离，增强呼吸感 */
  border: none;
  border-top: 1px dashed #e2e8f0;
}

.extra-box {
  background-color: #f8fafc;
  padding: 18px; /* 🟢 增大内边距，使文字包裹看起来更精致高级 */
  border-radius: 10px;
  text-align: left;
  border-left: 4px solid #94a3b8;
}

.extra-title {
  display: block;
  font-size: 13px;
  font-weight: bold;
  color: #64748b;
  margin-bottom: 8px; /* 🟢 增加标题与正文的间距 */
  letter-spacing: 0.5px;
}

.extra-content {
  font-size: 12px;
  color: #475569;
  line-height: 1.4; /* 🟢 补充解析区也需要良好的阅读行高 */
}
```
