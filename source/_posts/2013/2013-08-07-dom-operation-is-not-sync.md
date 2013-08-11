---
layout: post
comments: true
title: "了解浏览器的渲染特性"
categories: ["浏览器", "ie", "dom", "javascript", "render"]
---

### 一个在ie6不兼容的线上问题

昨天，有同事找我看一个线上问题，说一个在ie8测试通过的功能，在ie6上不能正常使用。
一开始，他用alert定位到是哪部分代码出问题，并认为后面有部分代码没有执行。奇怪的是，在这中间加上alert语句的话，
在ie6上也是能够正常运行的。

这部分代码简化后大概是这样的:

```html
<select onclick="javascript:selectbank(this);">
<option value="">--all--</option>
</select>
```

```javascript
function(obj){
  // get val and desc from a pop up page
  var opt = "<option value='" + val + "'>" + desc + "</option>";
  $(obj).append(opt);
  $(obj).val(val);
  // alert("test");
  // ...
}
```

我发现没有加alert的时候，页面会报js错误。
