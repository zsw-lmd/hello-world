<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>瀑布流拓展版</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            list-style: none;
        }
        ul {
            margin: 0 auto;
            position: relative;
        }
        li {
            width: 200px;
            background-color: tomato;
            color: white;
            font-size: 3em;
            text-align: center;
            position: absolute;
            -webkit-transition: 0.3s;
            -o-transition: 0.3s;
            transition: 0.3s;
        }
    </style>
</head>
<body>
    <ul></ul>
</body>
<script>
    var ul = document.querySelector("ul");
    
    //用来记录每一列的高度 
    var arrHeight = [];
    //保存已经存在的li
    var arrLi = [];
    //用来保存每一个li的高度
    var liHeight = [];
    
    //用来检测是否是第一次布局
    var isFirst = true;
    
    //用来记录当前的列数
    var col = 0;
    
    //随机函数
    function randFn(min,max) {
        return Math.round(Math.random() * (max-min)) + min;
    }
    //求取列数
    function getCols() {
        var w = document.body.offsetWidth;
        var cols = parseInt(w/210);
        return cols;
    }
    //布局
    function setView() {
        //确定下来ul的宽度
        col = getCols();
        arrHeight = [];
        for(var i = 0; i < col; i++) {
            arrHeight.push(0);
        }
        ul.style.width = 210*col + 'px';
        
        //当前最小列的下标
        function getMin() {
            var min = 0;
            for (var j = 1; j < col; j ++) {
                if (arrHeight[min] > arrHeight[j]) {
                    min = j;
                }
            }
            return min;
        }
        
        //填充li
        for (var i = 0; i < 40; i++) {
//            var li = arrLi[i] || document.createElement("li");
            
            var li = null;
            if (arrLi[i]) {
                li = arrLi[i];
            } else {
                li = document.createElement("li");
            }
            
            var h = liHeight[i] || randFn(100,300);
            li.style.height = h + 'px';
            li.innerHTML = i+1;
            
            //计算li的位置
            var index = getMin();
            li.style.left = index * 210 + 'px';
            li.style.top = arrHeight[index] + 'px';
            
            //更新对应列的高度
            arrHeight[index] += h + 10;
            
            if (isFirst) {
                ul.appendChild(li);
                arrLi.push(li);
                liHeight.push(h);
            }
        }
    }
    setView();
    
    //检测窗口大小变化
    window.onresize = function() {
        var newCol = getCols();
        if (newCol != col) {
            isFirst = false;
            setView();
        }
    }
</script>
</html>








