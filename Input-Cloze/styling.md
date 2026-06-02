# CSS

'''
/* 全局基础与卡片盒 */
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
  padding: 40px 30px 30px 30px;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
  border: 1px solid #eef2f5;
}

/* 右上角 Unit 徽章 */
.unit-badge {
  position: absolute;
  top: 15px;
  right: 15px;
  background-color: #e6f0fa;
  color: #007bff;
  font-size: 12px;
  font-weight: bold;
  padding: 4px 10px;
  border-radius: 20px;
  letter-spacing: 0.5px;
}

/* 主文本段落 */
.content-box {
  font-size: 19px;
  color: #2c3e50;
  line-height: 1.8;
  text-align: left;
}

/* 📥 核心：正面嵌入到句子中的多个输入框 */
.custom-type-input {
  display: inline-block;
  border: none;
  border-bottom: 2px solid #cbd5e1;
  background-color: #f8fafc;
  padding: 2px 8px;
  font-size: 18px;
  color: #2c3e50;
  width: 120px; /* 默认宽度 */
  text-align: center;
  border-radius: 4px 4px 0 0;
  margin: 0 4px;
  transition: all 0.2s ease;
}

.custom-type-input:focus {
  outline: none;
  border-bottom-color: #007bff;
  background-color: #e6f0fa;
}

/* 📤 核心：背面嵌入到句子中的红绿对比包裹盒 */
.multi-type-result {
  display: inline-block;
  background-color: #f1f5f9;
  padding: 2px 6px;
  border-radius: 6px;
  font-family: Consolas, "Liberation Mono", monospace;
  font-size: 18px;
  margin: 0 4px;
  vertical-align: middle;
}

/* 🟢 校验正确 */
.typeGood {
  color: #1e7e34;
  background-color: #d4edda;
  padding: 0 2px;
  border-radius: 2px;
  font-weight: bold;
}

/* 🔴 校验错误 */
.typeBad {
  color: #bd2130;
  background-color: #f8d7da;
  padding: 0 2px;
  border-radius: 2px;
  text-decoration: line-through;
}

/* 🟡 漏拼/没写 */
.typeMissed {
  color: #d39e00;
  background-color: #fff3cd;
  padding: 0 2px;
  border-radius: 2px;
  font-style: italic;
  font-weight: bold;
}

/* 补充解析区 */
.extra-divider {
  margin: 25px 0 15px 0;
  border: none;
  border-top: 1px dashed #e2e8f0;
}

.extra-box {
  background-color: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  text-align: left;
  border-left: 4px solid #94a3b8;
}

.extra-title {
  display: block;
  font-size: 13px;
  font-weight: bold;
  color: #64748b;
  margin-bottom: 6px;
}

.extra-content {
  font-size: 16px;
  color: #475569;
}

/* 提示词标签样式 */
.cloze-hint-badge {
  font-size: 14px !important;   /* 确保字号生效 */
  color: #64748b !important;   /* 确保颜色可见（深灰色） */
  font-weight: normal !important;
  margin-left: 6px;
  margin-right: 6px;
  display: inline-block !important;
  vertical-align: middle;
}
'''
