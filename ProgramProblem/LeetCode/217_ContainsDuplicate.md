编号217：存在重复
=============================

> LeetCode 217 Contains Duplicate

*** *** ***

## 题目

给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 `true`。

如果数组中每个元素都不相同，则返回 `false`。

示例 1:
```
输入: [1,2,3,1]
输出: true
```

示例 2:
```
输入: [1,2,3,4]
输出: false
```

示例 3:
```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```


*** *** *** ***

## 解法：构建散列表

将数组中的元素依次添加到散列表中，如果预添加元素已经出现在散列表中，则包含重复元素。

```javascript
var solution = function(nums, k) {
    var h = {};
    for (var i = 0; i < nums.length; i++) {
        var n = nums[i];
        if (n in h) {
            return true;
        }
        h[n] = 1;
    }
    return false;
};
```

#### 输入
<textarea id="problemInput" rows="2" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solution', 'problemInput', 'problemOutput')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInput', '[5,3,4,2,2]')">重置输入</button>
</span>

#### 输出
<p id="problemOutput">NULL</p>


*** *** ***
###### [Back](./index)

<script id="jsSolutions" type="text/javascript">
var solution = function(nums) {
    var h = {};
    for (var i = 0; i < nums.length; i++) {
        var n = nums[i];
        if (n in h) {
            return true;
        }
        h[n] = 1;
    }
    return false;
};
</script>

<script id="jsUtils" type="text/javascript">
var utilTrim = function(s) { return s.replace(/(^\s*)|(\s*$)/g, ""); };
var utilIsInt = function(i) { return (typeof i === "number" && i % 1 === 0) }
</script>

<script id="jsInterface" type="text/javascript">
var parseInput = function(lines) {
    if (!(lines instanceof Array && lines.length == 1)) { return false; }
    try {
        nums = eval(lines[0]);
    } catch (err) { return false; }
    if (!(nums instanceof Array)) { return false; }
    for (var i = 0; i < nums.length; i++) { if (!utilIsInt(nums[i])) { return false; } }
    return nums;
};

var resetInput = function(inputName, inputValue) {
    var i = document.getElementById(inputName);
    i.value = inputValue;
}

var formatOutput = function(result) {
    return result;
};

var problemRun = function(solutionName, inputName, outputName) {
    // get input and output element
    var i = document.getElementById(inputName);
    var o = document.getElementById(outputName);
    i.setAttribute("readonly", "readonly");
    
    // parse input
    var args = utilTrim(i.value);
    var lines = args.split("\n");
    var r = parseInput(lines);
    
    // check input
    if (!r) {
        r = "输入错误";
    } else {
        var nums = r;
        
        // execute solution
        var r = eval(solutionName + "(nums);");
        var r = formatOutput(r);
    }
    
    // change output and unlock input
    o.innerText = r;                    // or innerHTML. value is not valid
    i.removeAttribute("readonly");
};

window.onload = function(){
    var targets = document.getElementsByClassName("problemInit");
    for (var i = 0; i < targets.length; i++) {
        targets[i].click();
    }
}

</script>
