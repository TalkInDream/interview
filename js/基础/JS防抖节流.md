# 防抖与节流
1、防抖：
```
<button id="button">防抖</button>

function debounce(handler, time) {
    var timer;
    var num = 0;
    return function () {
        num++;
        var _this = this;
        console.log('33333',num);
        var args = arguments;
        timer !== 'undefined' && clearTimeout(timer);
        timer = setTimeout(function () {
            handler.apply(_this, args)
        }, time)
    }
} 

let fn = debounce(function(){ console.log('1111111');} , 2000);
button.onclick = function () {
    fn();
}
```