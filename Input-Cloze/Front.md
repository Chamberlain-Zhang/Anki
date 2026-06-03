#Front Template
```html
<div style="display: none;">{{cloze:Text}}</div>
<div class="card-container">
  {{#Unit Number}}
    <div class="unit-badge">Unit {{Unit Number}}:{{Unit Subject}}</div>
  {{/Unit Number}}

  {{#Topics}}
    <div class="topics-badge">{{Topics}}</div>
  {{/Topics}}

  <div id="raw-text" style="display:none;">{{Text}}</div>

  <div id="display-content" class="content-box"></div>
</div>

<script>
(function() {
  const rawTextHtml = document.getElementById('raw-text').innerHTML;
  const displayBox = document.getElementById('display-content');
  
  // 获取当前卡片的填空序号
  let currentCardNum = 1;
  const cardClass = document.body.className;
  const matchNum = cardClass.match(/card(\d+)/);
  if (matchNum) {
    currentCardNum = parseInt(matchNum[1]);
  }

  // 针对象牙塔双大括号的防挂机制
  const openBraces = '{' + '{';
  const closeBraces = '}' + '}';

  // 【终极重构正则】
  // ([^\\}]+?) -> 捕获组 1：答案（匹配到遇到 } 或 : 为止）
  // (?:区域) -> 匹配 ::提示词，并用 ([^\\}]+) 捕获组 2 抓取纯提示文本
  const currentClozeRegex = new RegExp(openBraces + `c${currentCardNum}::([^\\}]+?)(?:::(.*?[^\\}]+?))?` + closeBraces, 'g');
  const otherClozeRegex = new RegExp(openBraces + `c\\d+::([^\\}]+?)(?:::(.*?[^\\}]+?))?` + closeBraces, 'g');

  let index = 0;
  // 处理当前需要考试的空
  let processedHtml = rawTextHtml.replace(currentClozeRegex, function(match, answer, hint) {
    index++;
    const savedVal = sessionStorage.getItem('anki_type_' + index) || '';
    
    // 2. 🟢【核心宽度计算器】
    // 取“标准答案长度”和“用户已填写的答案长度”的最大值，确保不管用户在打字还是空置，框都能包住文本
    const textLength = Math.max(answer.length, savedVal.length);
    
    // 根据字数动态计算像素宽度。18px 的字体大小，每个英文字母大约占 10-11px。
    // 额外加上 20px 的安全边距（Padding 补偿），防止光标把最后一个字母挤歪。
    const dynamicWidth = (textLength * 10.5) + 10; 
    
    // 3. 将计算好的 width 动态塞进 HTML 标签中
    let inputHtml = `<input type="text" 
                            class="custom-type-input" 
                            data-index="${index}" 
                            value="${savedVal}" 
                            style="width: ${dynamicWidth}px;" 
                            placeholder="" 
                            autocomplete="off">`;
    
    // 如果存在提示词，且提示词不为空白
    if (hint && hint.trim()) {
      inputHtml += `<span class="cloze-hint-badge">(${hint.trim()})</span>`;
    }
    
    return inputHtml;
  });

  // 还原不需要校验的其他空
  processedHtml = processedHtml.replace(otherClozeRegex, function(match, answer, hint) {
    return answer;
  });
  
  displayBox.innerHTML = processedHtml;

  // 监听输入事件
  displayBox.addEventListener('input', function(e) {
    if (e.target.classList.contains('custom-type-input')) {
      const idx = e.target.getAttribute('data-index');
		 const currentVal = e.target.value;
		  sessionStorage.setItem('anki_type_' + idx, e.target.value);
			// 🟢【打字实时伸缩器】
      // 动态获取输入框上的 placeholder 或者绑定的初始宽度依据（这里我们通过输入框当前的字数来实时纠偏宽度）
      // 假设当前输入的字数大于 4 个字符，就开始成比例扩容
      if (((currentVal.length * 10.5) + 10) > parseFloat(e.target.style.width)) {
         e.target.style.width = (currentVal.length * 10.5) + 10 + 'px';
      }
    }
  });

// 监听回车键事件（切换到答案页面）
displayBox.addEventListener('keydown', function(e) {
  if (e.target.classList.contains('custom-type-input')) {
    // 检测是否按下回车键
    if (e.key === 'Enter' || e.keyCode === 13) {
      // 阻止默认的表单提交行为
      e.preventDefault();
      
      // 保存当前输入（可选，确保最后一次输入也被保存）
      const idx = e.target.getAttribute('data-index');
      sessionStorage.setItem('anki_type_' + idx, e.target.value);
      
      // 切换到Anki的answer页面
      // 方法1：使用Anki的内置函数（推荐）
      if (typeof pycmd !== 'undefined') {
        pycmd('ans');
      }
      
      // 方法2：如果在网页版Anki中，可能需要触发按钮点击
      // const showAnswerBtn = document.querySelector('.show-answer-button');
      // if (showAnswerBtn) {
      //   showAnswerBtn.click();
      // }
      
      // 方法3：如果使用自定义的Anki接口
      // window.ankiBridge?.showAnswer();
    }
  }
});

  // 自动聚焦第一个输入框
  const firstInput = displayBox.querySelector('.custom-type-input');
  if (firstInput) firstInput.focus();
	sessionStorage.setItem('currentCardNum' , currentCardNum);
})();
</script>
```
