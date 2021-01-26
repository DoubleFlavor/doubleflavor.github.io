# 페이지에서 다중언어 지원하기

[About 페이지](https://doubleflavor.github.io/about/)와 현재 페이지가 한글과 영어를 지원하는 것을 볼 수 있다.  
오늘은 페이지에서 다중언어를 지원하는 방법에 대해서 알아보고자 한다.  

```{ %```, ```% }```, ```{ {```, ```} }```가 코드 예시 작성에 실제 코드로 인식되어 부득이하게 띄어 썼는데, 실제 코드에서는 붙여야 한다.

## footer.html에 다중 언어 지원 코드 작성
```footer.html``` 파일에 해당 코드를 작성한다.  
페이지의 메타 데이터에 ```multilingual:true```로 되어있다면, 각각의 언어를 설정해주는 코드이다.  
이 코드를 수정하여 다른 언어를 추가, 변경, 삭제할 수 있다.

```html
<!-- Multi-Lingual -->
{ % if page.multilingual % }
<!-- Handle Language Change -->
<script type="text/javascript">
    // get nodes
    var $kr = document.querySelector(".kr");
    var $en = document.querySelector(".en");
    var $select = document.querySelector("select");

    // Changes at v1.8.1: include lang flag as a url query. This interop well with catalog hash anchors.
    function getLang() { return new URLSearchParams(document.location.search).get("lang") }

    function setLang(newLang) {
        var params = new URLSearchParams(document.location.search)
        params.set("lang", newLang)
        document.location.search = params.toString()  // refresh.
    }

    // handle render
    function _render() {
        var lang = getLang()
        // en
        if (lang == "en") {
            $select.selectedIndex = 1;
            $en.style.display = "block";
            $en.classList.add("active");
            $kr.style.display = "none";
            $kr.classList.remove("active");
            // default to kr
        } else {
            $select.selectedIndex = 0;
            $kr.style.display = "block";
            $kr.classList.add("active");
            $en.style.display = "none";
            $en.classList.remove("active");
        }
        // interop with catalog 
        generateCatalog(".catalog-body");
    }

    // handle select change
    function onLanChange(index) {
        if (index == 0) {
            lang = "kr"
        } else {
            lang = "en"
        }
        setLang(lang)
    }

    // init
    _render();
</script>
{ % endif % }

```

## Language Selector

### Language Selector를 보여주는 html 파일 생성
```multilingual-sel.html```라는 파일 명으로 selector를 하나의 html 파일로 만든다.
```html
<!-- Language Selector -->
<select class="sel-lang" onchange= "onLanChange(this.options[this.options.selectedIndex].value)">
    <option value="0" selected> 한글 | Korean </option>
    <option value="1"> 영어 | English </option>
</select>
```


### *sel-lang* 클래스 선언
```.less```파일에 ```sel-lang``` 클래스를 선언한다.
```
// Select
select {
  -webkit-appearance: none;
  margin-top: 15px;
  color: #337ab7;
  border-color: #337ab7;
  padding: 0em 0.4em;
  background: white;
  &.sel-lang {
    min-height: 28px;
    font-size: 14px;
  }
}
```

## 페이지/포스트에 다중 언어 지원하기
```.html```파일로 페이지를 만들거나, ```.md```파일로 포스트를 작성할 수 있다.

### 포스트 메타 데이터
다중언어를 지원하고 싶다면, **포스트 메타 데이터**에 ```multilingual: true```를 추가한다.

### html 파일에 다중언어 옵션 추가하기
```.html``` 파일을 통해 다중언어를 지원하는 페이지를 만든다면, 다음 코드를 추가한다.  
```html
{ % if page.multilingual % }
    { % include multilingual-sel.html % }
{ % endif % }
```
<br>
파일에 포스트 메타 데이터와 다중언어 옵션 코드를 모두 작성했다면,  
포스트 헤더 부분에 다음과 같이 언어를 선택하는 **옵션 박스**를 볼 수 있다.  
![옵션박스](/img/in-post/2021-01-22-ys-journal2.png)

### .md 파일 연결하기 
```.html```파일 또는 ```.md```파일에 다음 코드를 추가한다.  
**class**에 있는 ```kr```과 ```en```는 ```footer.html```에 선언되어 있다.  
**include** 다음에 있는 ```path/file_name.md```에 포스트를 마크다운(markdown) 형식으로 작성한다.

```html
<!-- Korean Version -->
<div class="kr post-container">
    { % capture about_kr % }{ % include about/kr.md % }{ % endcapture % }
    { { about_kr | markdownify } }
</div>

<!-- English Version -->
<div class="en post-container">
    { % capture about_en % }{ % include about/en.md % }{ % endcapture % }
    { { about_en | markdownify } }
</div>
```

## .md 작성하기
위에 .md 파일 연결하기에 작성한 대로 해당 위치에 ```path```폴더와 ```file_name.md```를 생성한다.  
각 파일을 해당되는 언어로 작성한다.

<br>
위의 방법을 따라하여 .html또는 .md 파일에서 다중언어를 지원할 수 있다.<br><br>
