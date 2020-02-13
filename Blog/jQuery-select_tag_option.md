# [select](https://www.w3schools.com/tags/tag_select.asp) tag의 모든 값 가져오기

> Get all options values of select tag

## Code

```
$('#select').find('option').map(function() {return $(this).val();}).get()
// return []
```

> $('#select option')과 같은 표현식도 가능하지만, 각 DOM 객체를 확실히 표현하기 위해 나눠서 사용했다.

## .get()

[.get()](https://api.jquery.com/get)

마지막에 .get()을 하지 않으면 select tag의 모든 jQuery가 순회할 수 있는 properties를 가져온다.  
따라서, **값만 가져오는 `.get()`을 꼭 넣어준다.**

## Codepen

<p class="codepen" data-height="265" data-theme-id="default" data-default-tab="html,result" data-user="SangHakLee_1470355178" data-slug-hash="oNgRmgr" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="get select-box value">
  <span>See the Pen <a href="https://codepen.io/SangHakLee_1470355178/pen/oNgRmgr">
  get select-box value</a> by Sanghak,Lee (<a href="https://codepen.io/SangHakLee_1470355178">@SangHakLee_1470355178</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
