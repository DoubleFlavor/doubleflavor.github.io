## How to support multi-language

You can see that [About Page](https://doubleflavor.github.io/about/) and this page supports Korean and English.  
In this post, let's talk about how to support multi-language in the page.  

```{ %```, ```% }```, ```{ {```, ```} }``` should not have space between characters, but I added space because they are considered as a real code.

## footer.html
First, add the below code in ```footer.html``` for multi-lingual translation.  

If ```multilingual:true``` in the page's meta data, the below code handles each language setting.  
You can add, modify, delete other languages by changing the code.  

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

### Create a html file
Create a html file named ```multilingual-sel.html``` to set **Language Selector**.  
```html
<!-- Language Selector -->
<select class="sel-lang" onchange= "onLanChange(this.options[this.options.selectedIndex].value)">
    <option value="0" selected> 한글 | Korean </option>
    <option value="1"> 영어 | English </option>
</select>
```


### Define *sel-lang* class
Define ```sel-lang``` class in ```XXX.less``` file.  
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

## Support Multi-language in Page/Post
You can create a page by ```.html``` file or write a post using ```.md``` file.  

### Post Meta Data
To support multi-language translation, add ```multilingual: true``` in **post meta data**.  

### Add multi-language option in a .html file.
If you are using a ```.html``` file to create a page, add the below code in ```.html``` file.  
```html
{ % if page.multilingual % }
    { % include multilingual-sel.html % }
{ % endif % }
```
<br>
If "post metat data" and "multi-language option" code is added,  
You can see the **Option Box** that shows language option in page's header.  
![옵션박스](/img/in-post/2021-01-22-ys-journal2.png)

### Connect .md file 
Add the below code in ```.html``` file or ```.md``` file.  

```kr``` and ```en ``` **class** is defined in ```footer.html```.  
Write a post in a **markdown** format in ```path/file_name.md``` that is next to **include**.  

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

## Write .md file
Create a ```path``` folder and a ```file_name.md``` file in the ```_include``` directory which is stated in "Connect .md file".  
Write a file using corresponding language.  

<br>
By following above process, a page/post will suport multi-language option.<br><br>
