#Front Template
```html
<div style="display: none;">{{cloze:Text}}</div>
<div class="card-container">
  {{#Unit Number}}
    <div class="unit-badge">Unit {{Unit Number}}:{{Unit Subject}}</div>
  {{/Unit Number}}

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
    
    let inputHtml = `<input type="text" class="custom-type-input" data-index="${index}" value="${savedVal}" placeholder="填空 ${index}" autocomplete="off">`;
    
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
		  sessionStorage.setItem('anki_type_' + idx, e.target.value);
    }
  });

  // 自动聚焦第一个输入框
  const firstInput = displayBox.querySelector('.custom-type-input');
  if (firstInput) firstInput.focus();
	sessionStorage.setItem('currentCardNum' , currentCardNum);
})();
</script>
```
