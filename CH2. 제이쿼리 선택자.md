# CH2. 제이쿼리 선택자

<br>
<br>

제이쿼리는 **'DOM'**의 조작을 위해 엘리먼트를 선택하는 강력한 기능 제공하고 있으며, 선택된 엘리먼트들을 제어할 수 있는 다양한 함수들도 함께 제공하고 있습니다. 이 때 제이쿼리를 이용해 어떤 엘리먼트를 대상으로 할 지 지정해 주는 역할을 하는 것을 **'선택자'** 하며, 제이쿼리의 가장 큰 장점 중 하나는 **'CSS'에서 사용하는 선택자를 그대로 사용**할 수 있다는 것입니다.

<br>

### 기본 예제(Default example) \-   [샘플보기1](http://wdschools.co.kr/gate/classroom/chapter4-jquery/page/sample/test2-selector.html)    [샘플보기2](http://wdschools.co.kr/gate/classroom/chapter4-jquery/page/sample/traversing/selector.html)

아래의 소스를 복사하여 적용을 시작하세요!

<br>

#### [ 샘플 코드 ]


- HTML

```
/* ////////////////////////////////////////// HTML 소스  ////////////////////////////////////////// */

<h1>[ 제이쿼리 셀렉터(선택자) ]</h1>

<p>제이쿼리는 제어를 위한 다양한 함수도 제공하며, 크로스브라우징도 함께 지원합니다!</p>

<ol>
	<li class="list1 first">당근</li>
	<li class="list1">양파</li>
	<li class="list1">양배추</li>
	<li class="list1">피망</li>
	<li class="list1">여우</li>
	<li class="list2" id="cat">고양이</li>
	<li class="list2">새</li>
	<li class="list2" id="dog">강아지</li>
	<li class="list2">물고기</li>
</ol>

<div class="box">
	<p>우리는 웹퍼블리셔 입니다!</p>
	<div>
		<p>제이쿼리의 다양한 선택자를 활용해 봅시다!</p>
	</div>
</div>

<p>
	<label for="t1">Message :</label>
	<input type="text" name="msg" id="t1">
</p>



/* ////////////////////////////////////////// jQuery 소스  ////////////////////////////////////////// */

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>

<script>
	
$(function(){

  // Tag 선택자 
  $('h1').css({
	  'background-color': '#c60',
	  'padding': '10px',
	  'color': '#fff'
  });
  $('li').css('margin-bottom', '5px');

  // Class 선택자
  $('.list1').css('background-color', '#f2f2f2');
  $('.list2').css('background-color', '#e2e2e2');

  // Id 선택자
  $('#cat').css('background-color', '#fc9');
  $('#dog').css('background-color', '#9f9');

  // 선택자 그룹지정
  $('#cat, #dog').css('border', '1px solid #000');

  // 인접형제 선택자
  $('ol+div').css({
	  'border': '5px solid #ccc',
	  'padding': '10px'
  });
  $('ol+div p').css({
	  'margin': 0, 
	  'padding': 0
  })

  // 하위선택자 - 자손선택자
  $('.box p').css('color', 'green');

  // 하위선택자 - 직계자식선택자
  $('.box>p').css('color', 'red');

  // 속성 선택자
  $('label[for]').css({
    'color': '#333',
    'font-weight': 'bold'
  });

  // 가상클래스 선택자
  $('ol li:nth-child(3)').css({
	  'background-color': '#9f9',
	  'border': '1px solid #000'
  });

  // 인덱스 번호로 찾기
  $('ol li:eq(0)').css({
	  'background-color': '#fc9',
	  'border': '1px solid #000'
  });

});
</script>
```
