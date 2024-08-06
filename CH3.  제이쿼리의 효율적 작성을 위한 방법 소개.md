# CH3.  제이쿼리의 효율적 작성을 위한 방법 소개

<br>

제이쿼리를 사용함에 있어 성능 최적화(performance)에 간단히 알아봅니다.

<br>
<br>  

## 1) DOM 을 캐싱(Caching)하라

한번 탐색한 DOM을 다시 탐색하는 것은 비효율적이다.

  
#### BAD CODE

```
$('.btn').on('click', function() {
  // DOM 한번 탐색
  $('.modal').modal();

  // DOM 다시 한번 탐색
  $('.modal').addClass('active');

  // DOM 또 다시 한번 탐색
  $('modal').css(...);
});
```

#### GOOD CODE

```
$('.btn').on('click', function() {
  // DOM 한번만 탐색 (체이닝)
  $('.modal')
    .modal()
    .addClass('active')
    .css(...);
});

$('.btn').on('click', function() {
  // DOM 한번만 탐색 (캐시)
  var modal = $('.modal');

  modal.modal();
  modal.addClass('active');
  modal.css(...);
});
```
<br>
<br>

## 2) 가급적 전역(Global)을 오염시키지 말 것.

전역변수를 무분별하게 사용할 경우, 코드 충돌이 발생할 위험이 커진다. 가급적 명시적 전역변수로 선언하여 사용하는 것을 권장합니다.

#### BAD CODE

```
$element = $('.element');
h = $element.height();         // <= 전역변수
$element.css('height', 200);
```

#### GOOD CODE

```
let $element = $('.element');
let h = $element.height();      // <= 지역변수
$element.css('height', 200);
```
<br>
<br>

## 3) 명시적인 '$' 를 식별자를 사용하자.

jQuery 객체를 참조한 변수 이름 앞에 $ 를 붙여 jQuery 를 사용 가능한 객체임을 식별하도록 하여 혼란을 감소시키자.
  

#### BAD CODE

```
let first = $('.first');
let second = $('.second');
let value = $first.val();
```

#### GOOD CODE

```
let $first = $('.first');
let $second = $('.second');
let value = $first.val();
```

<br>
<br>  

## 4) let 체이닝을 사용하라.

싱글 var 를 활용하면 var 중복사용을 줄일 수 있다.

#### BAD CODE

```
let first  = $('.first');
let second = $('.second');
let value  = $first.val();
```

#### GOOD CODE

```
let $first = $('.first'),
    $second = $('.second'),
    value = $first.val();
```

<br>
<br>

## 5) 'on' 메서드 사용을 선호하라.

단, jQuery 1.7 이상 버전부터 on 을 지원한다는 사실을 잊지 말자.

#### BAD CODE

```
$first.click(function(){
  $first.css('border','1px solid red');
  $first.css('color','blue');
});

$first.hover(function(){
  $first.css('border','1px solid red');
});
```

#### GOOD CODE

```
$first.on('click', function(){
  $first.css('border','1px solid red');
  $first.css('color','blue');
});

$first.on('hover', function(){
  $first.css('border','1px solid red');
});
```
<br>
<br>

## 6) 묶을 수 있다면 최대한 묶어서 사용하라. (간결화)

하나의 요소에 각각의 프로세스를 진행하는 것 보다 하나의 요소에 연결된 프로세스를 진행하는 것이 훨씬 빠르다.
 
#### BAD CODE

```
$first.click(function(){
  $first.css('border','1px solid red');
  $first.css('color','blue');
});
```

#### GOOD CODE

```
$first.on('click',function(){
  $first.css({
    'border':'1px solid red',
    'color':'blue'
  });
});
```

<br>
<br>  

## 7) 문장을 연결(Chaining)되도록 묶어서 가독성이 뛰어난 코드를 만들어라.

하나의 객체에 메소드를 묶으면 진행될 프로세스를 이해하기 쉬워진다. 또한 읽기 쉬운(가독성) 코드는 수정이 용이하다.
 
#### BAD CODE

```
$second.html(value);
$second.on('click',function(){
  alert('hello everybody');
}).fadeIn('slow').animate({height:'120px'},500);
```

#### GOOD CODE

```
$second
  .html(value)
  .on('click',function(){alert('hello everybody');})
  .fadeIn('slow')
  .animate({height:'120px'}, 500);
```
<br>
<br>  

## 8) DOM 조작이 많을 경우, 분리(Detach)하여 적용하자.

문서에 붙여진 상태에서 다수의 DOM 조작은 비효율적이기 때문에 조작(manupuration)하고 다시 붙이는게 좋다.
  
#### BAD CODE

```
let $container   = $(".container"),
$containerLi = $(".container li"),
$element     = null;

$element = $containerLi.first();
// $element에 다수의 조작(Manipulation)이 수행
```

#### GOOD CODE

```
let $container = $(".container"),
$containerLi = $container.find("li"),
$element = null;

$element = $containerLi.first().detach();
// $element에 다수의 조작(Manipulation)이 수행

$container.append($element);

* .detach() 함수는 .remove()입니다. 대신 .detach() 함수에 의해 제거된 요소들은 모두 jQuery 데이터 형태로 유지됩니다. 
즉, 이 함수는 DOM에서 제거했다가 추후에 다시 삽입하는 형태로 사용하기 아주 유용합니다.

- 샘플 => http://api.jquery.com/detach/
```
<br>
<br>  

## 9) jQuery 객체를 참조한 변수를 통해 요소를 탐색하라

DOM 캐시와 같은 이유로 한번 참조한 대상을 통해 내부를 탐색하는 것이 보다 빠르다.  

#### BAD CODE

```
let $container   = $('.container'),
$containerLi     = $('.container li'),
$containerLiSpan = $('.container li span');
```  

#### GOOD CODE

```
let $container = $('.container '),
$containerLi = $container.find('li'),
$containerLiSpan = $containerLi.find('span');
```
<br>
<br>

## 10) 전체 선택자 ( \* ) 사용을 가급적 피하라.

* 의 사용은 속도를 느리게 만드는 주범이다.  

#### BAD CODE

```
$('.container > *');
```  

#### GOOD CODE

```
$('.container').children();
```
<br>
<br>  

## 11) 암묵적 선택자보다는 명시적인 선택자를 활용하라.

암묵적 선택자를 사용하는 것은 결국 \*(전체 선택자)를 사용하는 것과 다름없다.  

#### BAD CODE

```
$('.someclass :radio');
```
 
#### GOOD CODE

```
$('.someclass input:radio');
```
<br>
<br>

## 12) ID 선택자 사용 시에는 앞에 요소 이름을 제거하라.

문서에는 단 하나의 ID 를 사용해야 하기 때문에 중복될리 만무하므로 불필요한 요소를 사용하여 탐색을 하게 할 필요가 없다. 

#### BAD CODE

```
$('div#myid');
$('div#footer a.myLink');
```

#### GOOD CODE

```
$('#myid');
$('#footer a.myLink');
```
<br>
<br>

## 13) ID 내부의 ID 를 찾는 것은 어리석은 일이다.

고유한 ID 영역 내부에서 탐색을 하는데 있어서 굳이 상위 영역의 식별자까지 사용하여 탐색을 하게할 필요가 없다.  

#### BAD CODE

```
$('#outer #inner');
``` 

#### GOOD CODE

```
$('#inner');
```
<br>
<br> 

## 14) 가급적 최신 버전의 jQuery 를 사용하라.

업그레이드된 최신 jQuery 버전이 속도, 기능면에서 좋다. 단, 2.x 버전부터는 IE 6,7,8 이 지원되지 않습니다.

<br> 

## 15) 프로젝트의 최종단계에서는 jQuery CDN 을 사용하라.

CDN(Content Delivery Network) 사용시 사용자의 가장 가까운 곳에서 데이터를 가져오기 때문에 속도 향상에 도움이 된다.

<br> 

## 16) jQuery 만 사용하기 보다는 JS(Javascript) 와 함께 사용하라.

jQuery 는 결국 JavaScript 일 뿐더러 jQuery 보다 JS(JavaScript) 가 보다 빠르다.