---
layout: post
title: JavaScript的一些代码片段
date: 2016-03-03
category: js
tag: js
---

**摘要:**

以前以为后端工程师不用写js,但是在一些创业公司人手不足的情况还是需要写一写的;
多学习些东西总归是好的,不要去排斥.js是一门很牛逼的语言,可以干很多事情哦!
在平时开发过程中使用到的一些js代码小片段,摘抄在这里,有可能会用到的.自己对js代码
很熟悉,在开发后台代码的过程中用到一些,有些没有记下来,这里当做备忘了.


#### JS case: replace exclude zh abc & . string !

```
var newstr = str.replace(/[^\u4E00-\u9FA5\w\.]/g, '');
```

---

#### Select Table Element Cloumn

- sum the cloumn value
    
```
var x = 0;
for (var j = 0; j < $("tbody tr").length; j++) {
	x += parseInt($("tbody tr:eq(" + j + ")").children("td:eq(5)").html());
}
```

---

#### Get The Params From Url:

```
    function getUrlParam(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
        var r = window.location.search.substr(1).match(reg);
        if (r != null) return unescape(r[2]);
            return null;
    }
```

---

#### Float Regex
 
```
var checkNum = /^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$/;
```

---

#### ZH charset Replace

```
 var reg = /[\u4e00-\u9fa5]+/g;
 file.name = file.name.replace(reg, escape(Math.random().toString(36).replace(/[^a-z]+/g, '').substr(0, 5)));
```


---

#### Set A Select Element Option Value To A Input Elemtnt 

```
function select_to_input($(select_dom), $(input_dom), target_attr) {
    $(select_dom).change(function () {
        var content = $(select_dom).find('option:selected').attr(target_attr);
        $(input_dom).val(content);
    });
}
```

---

#### Jquery Packaging A Form Data(Array)

```
<div id="data">
    <form>
        <input type="file" name="userfile" id="userfile" size="20" />
        <input type="button" id="upload" value="upload" />
    </form>
</div>
<script>
        $(document).ready(function(){
                $('#upload').click(function(){
                    var fd = new FormData();    
                    fd.append( 'userfile', $('#userfile')[0].files[0]);
                    $.ajax({
                      url: 'upload/do_upload',
                      data: fd,
                      processData: false,
                      contentType: false,
                      type: 'POST',
                      success: function(data){
                        $('#data').empty();
                        $('#data').append(data);
                      }
                    });
                });
        });
</script>
```

---

#### Get A Dom From iframe Element

```
    $(window.frames["ifram-id"].document).find("target-dom");
```

---

#### Form List Submit

`<input type='submit' onclick='function abc()'>`
If in function abc has ajax to submit the form as the same time the element type='submit' , then will be submit twice!!!

---

#### Date Reverse Str

```
    function dateToStr(dateArray) {
        var dsArray = new Array();
        for (var i = 0; i < dateArray.length; i++) {
            console.log(dateArray[i]);
            var date_str = String(dateArray[i].getFullYear()) + '-' + String(dateArray[i].getMonth() + 1) + '-' + String(dateArray[i].getDate());
            dsArray.push(date_str);
        }
        return dsArray
    }
   
```

---

#### Array In Turn

```
 function reversArray(arr) {
        var x = [];
        $.each(arr, function (i, re) {
            x[i] = arr.pop();
        });
        return x
    }
```

---

#### String Format

```
String.format = function () {
        if (arguments.length == 0)
            return null;
        var str = arguments[0];
        for (var i = 1; i < arguments.length; i++) {
            var re = new RegExp('\\{' + (i - 1) + '\\}', 'gm');
            str = str.replace(re, arguments[i]);
        }
        return str;
    };
```


#### Form Serialize Data

```
    $("form").on("submit", function (event) {
        var formdata = $(this).serializeArray();
        var data = {};
        $(formdata).each(function (index, obj) {
            data[obj.name] = obj.value;
        });
        var url = '/m/ninepic';
        serverData(data, callbackCreateNinePic, url, 'post');
        return false
    });
```

#### Gen Date 
- Calculate current date and format date string.

```
Date.prototype.reduceFormatDate = function (days) {// yyyy-m-d
    Date.prototype.reduceDays = function (days) {
        this.setDate(this.getDate() - parseInt(days));
        return this;
    };
    var currentDate = new Date();
    var targetDate = currentDate.reduceDays(days);
    var dd = targetDate.getDate();
    var mm = targetDate.getMonth() + 1;
    var y = targetDate.getFullYear();
    var someFormattedDate = y + '-' + mm + '-' + dd;
    return someFormattedDate
};
```

```
Date.prototype.reduceFormatDate = function (days) {// yyyy-mm-dd
    Date.prototype.reduceDays = function (days) {
        this.setDate(this.getDate() - parseInt(days));
        return this;
    };
    var currentDate = new Date();
    var targetDate = currentDate.reduceDays(days);
    var dd = ('0' + (currentDate.getDate())).slice(-2);
    var mm = ('0' + (currentDate.getMonth() + 1)).slice(-2);
    var y = targetDate.getFullYear();
    var someFormattedDate = y + '-' + mm + '-' + dd;
    return someFormattedDate
};
```

