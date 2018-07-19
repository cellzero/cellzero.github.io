编号136：只出现一次的数字
=============================

> LeetCode 136 Single Number

*** *** ***

## 题目

给定一个*非空*整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

示例 1:
```
输入: [2,2,1]
输出: 1
```

示例 2:
```
输入: [4,1,2,1,2]
输出: 4
```

#### 说明：

你的算法应该具有线性时间复杂度。你可以不使用额外空间来实现吗？


*** *** *** ***

## 解法：异或操作

将数组中所有元素进行异或操作，结果即为只出现了一次的元素。简单证明如下：

对于数字任意数字，有 `n ^ 0 = n; n ^ n = 0;` 且异或操作满足交换律。即数组中相同的元素通过异或操作后必然为 `0`，而只出现一次的元素同 `0` 异或得到本身，即为所求结果。

```javascript
var solution = function(nums) {
    var t = 0;
    for (var i = 0; i < nums.length; i++) {
        t ^= nums[i];
    }
    return t;
};
```

#### 输入
<textarea id="problemInput" rows="2" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solution', 'problemInput', 'problemOutput')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInput', '[4,1,2,1,2]')">重置输入</button>
</span>

#### 输出
<p id="problemOutput">NULL</p>

*** *** *** ***

### 相关问题

+ [268. 缺失数字](./268_MissingNumber)


*** *** ***
###### [Back](./index)

<script id="jsSolutions" type="text/javascript">
var solution = function(nums) {
    var t = 0;
    for (var i = 0; i < nums.length; i++) {
        t ^= nums[i];
    }
    return t;
};
</script>

<script id="jsUtils" type="text/javascript">
var utilTrim = function(s) { return s.replace(/(^\s*)|(\s*$)/g, ""); };
var utilIsInt = function(i) { return (typeof i === "number" && i % 1 === 0) }
</script>

<script id="jsInterface" type="text/javascript">
var parseInput = function(lines) {
    var checkNums = function(nums) {
        var h = {}
        for (var i = 0; i < nums.length; i++) {
            if (!utilIsInt(nums[i])) { return false; }
            h[nums[i]] = (nums[i] in h) ? (h[nums[i]] + 1) : (1) ;
        }
        
        var c = 0;
        for (var i = 0; i < nums.length; i++) {
            if (h[nums[i]] == 1) { c += 1; }
            else if (h[nums[i]] == 2) { continue; }
            else { return false; }
        }
        
        return (c == 1) ? true : false;
    };
    
    if (!(lines instanceof Array && lines.length == 1)) { return false; }
    try {
        nums = eval(lines[0]);
    } catch (err) { return false; }
    if (!(nums instanceof Array)) { return false; }
    if (!(nums.length > 0)) { return false; }
    if (!checkNums(nums)) { return false; }
    
    return nums;
};

var resetInput = function(inputName, inputValue) {
    var i = document.getElementById(inputName);
    i.value = inputValue;
};

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
};

</script>
