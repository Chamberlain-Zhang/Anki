# Back Template

'''
<div style="display: none;">{{cloze:Text}}</div>
<div class="card-container">
  {{#Unit Number}}
    <div class="unit-badge">Unit {{Unit Number}}:{{Unit Subject}}</div>
  {{/Unit Number}}

  <div id="raw-answer" style="display:none;">{{Text}}</div>

  <div id="display-content" class="content-box"></div>

  {{#Extra}}
    <div class="extra-divider"></div>
    <div class="extra-box">
      <span class="extra-title">💡 补充解析</span>
      <div class="extra-content">{{Extra}}</div>
    </div>
  {{/Extra}}
</div>

<script>
(function() {
  const rawAnswerHtml = document.getElementById('raw-answer').innerHTML;
  const displayBox = document.getElementById('display-content');
  
  let currentCardNum = 1;
  const cardClass = document.body.className;
  const matchNum = cardClass.match(/cloze-(\d+)/);
  if (matchNum) {
    currentCardNum = parseInt(matchNum[1]);
  }

  // 逐字比对算法
  function diffStrings(user, correct) {
    if (user === correct) return `<span class="typeGood">${correct}</span>`;
    if (!user) return `<span class="typeMissed">${correct}</span>`;
    
    let result = '';
    let i = 0, j = 0;
    while (i < user.length || j < correct.length) {
      if (i < user.length && j < correct.length && user[i] === correct[j]) {
        result += `<span class="typeGood">${user[i]}</span>`;
        i++; j++;
      } else {
        if (j < correct.length && (i >= user.length || user[i] !== correct[j])) {
          let hasMatchLater = false;
          for (let k = j + 1; k < Math.min(j + 5, correct.length); k++) {
            if (user[i] === correct[k]) { hasMatchLater = true; break; }
          }
          if (hasMatchLater) {
            result += `<span class="typeMissed">${correct[j]}</span>`;
            j++;
          } else if (i < user.length) {
            result += `<span class="typeBad">${user[i]}</span>`;
            i++;
          } else {
            result += `<span class="typeMissed">${correct[j]}</span>`;
            j++;
          }
        } else if (i < user.length) {
          result += `<span class="typeBad">${user[i]}</span>`;
          i++;
        }
      }
    }
    return result;
  }

  const openBraces = '{' + '{';
  const closeBraces = '}' + '}';

  // 采用终极重构正则
  const currentClozeRegex = new RegExp(openBraces + `c${currentCardNum}::([^\\}]+?)(?:::(.*?[^\\}]+?))?` + closeBraces, 'g');
  const otherClozeRegex = new RegExp(openBraces + `c\\d+::([^\\}]+?)(?:::(.*?[^\\}]+?))?` + closeBraces, 'g');

  let index = 0;
  // 处理当前需要校验的空
  let processedHtml = rawAnswerHtml.replace(currentClozeRegex, function(match, correctAns, hint) {
    index++;
    const userAns = (sessionStorage.getItem('anki_type_' + index) || '').trim();
    const comparedHtml = diffStrings(userAns.trim(), correctAns.trim());
    
    sessionStorage.removeItem('anki_type_' + index);
    
    let resultHtml = `<code class="multi-type-result">${comparedHtml}</code>`;
    if (hint && hint.trim()) {
      resultHtml += `<span class="cloze-hint-badge">(${hint.trim()})</span>`;
    }
    return resultHtml;
  });

  // 还原不需要校验的其他空
  processedHtml = processedHtml.replace(otherClozeRegex, function(match, answer, hint) {
    return `<span class="other-cloze-text">${answer}</span>`;
  });

  displayBox.innerHTML = processedHtml;
})();
</script>
'''
