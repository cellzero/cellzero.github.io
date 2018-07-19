编号268：缺失数字
=============================

> LeetCode 268 Missing Number

*** *** ***

## 题目

给定一个包含 `0, 1, 2, ..., n` 中 `n` 个数的序列，找出 `0 .. n` 中没有出现在序列中的那个数。

示例 1:
```
输入: [3,0,1]
输出: 2
```

示例 2:
```
输入: [9,6,4,2,3,5,7,0,1]
输出: 8
```

#### 说明：

你的算法应该具有线性时间复杂度。你能否仅使用额外常数空间来实现?


**** **** ****

## 解法1：加和求差

使用求和公式 `n * (n + 1) / 2` 算出从 `0 .. n` 的和。随后依次减去数组中的数字，最终剩余的数即为未出现的数字。

```javascript
var solution = function(nums) {
    var n = nums.length;
    var sum = n * (n + 1) / 2;
    for (var i = 0; i < nums.length; i++) {
        sum -= nums[i];
    }
    return sum;
};
```

#### 输入
<textarea id="problemInputSum" rows="2" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionSum', 'problemInputSum', 'problemOutputSum')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputSum', '[5,4,3,1,0]')">重置输入</button>
</span>

#### 输出
<p id="problemOutputSum">NULL</p>

### 可能存在的问题

+ 当 `n` 非常大时，求和公式可能溢出。

**** **** ****

## 解法2：异或操作

将问题稍微转化，即可变为 [136. 只出现一次的数字](./136_SingleNumber) 。方法如下：原数组加上 `0 .. n` 即变成了一个长度为 `2n + 1` 的数组，且该数组中，除了目标元素（即缺失元素）每个元素出现两次。

转换后即可采用异或操作求出缺失数字。

```javascript
var solution = function(nums) {
    var r = 0;
    for (var i = 0; i < nums.length; i++) {
        r ^= nums[i];
        r ^= i;
    }
    r ^= nums.length;
    
    return r;
};
```

#### 输入
<textarea id="problemInputXor" rows="2" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionXor', 'problemInputXor', 'problemOutputXor')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputXor', '[5,4,3,1,0]')">重置输入</button>
</span>

#### 输出
<p id="problemOutputXor">NULL</p>


**** **** ****

### 相关问题

+ [136. 只出现一次的数字](./136_SingleNumber)


*** *** ***
###### [Back](./index)

<script id="jsSolutions" type="text/javascript">
var solutionSum = function(nums) {
    var n = nums.length;
    var sum = n * (n + 1) / 2;
    for (var i = 0; i < nums.length; i++) {
        sum -= nums[i];
    }
    return sum;
};
var solutionXor = function(nums) {
    var r = 0;
    for (var i = 0; i < nums.length; i++) {
        r ^= nums[i];
        r ^= i;
    }
    r ^= nums.length;
    
    return r;
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
            if (nums[i] in h) { return false; }
            h[nums[i]] = 1;
        }
        return true;
    };
    
    if (!(lines instanceof Array && lines.length == 1)) { return false; }
    try {
        nums = eval(lines[0]);
    } catch (err) { return false; }
    if (!(nums instanceof Array)) { return false; }
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

