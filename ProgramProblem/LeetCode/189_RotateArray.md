编号189：旋转数组
=============================

> LeetCode 189 Rotate Array

*** *** ***

## 题目

给定一个数组，将数组中的元素向右移动 `k` 个位置，其中 `k` 是非负数。

示例 1:
```
输入: [1,2,3,4,5,6,7] 和 k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

示例 2:
```
输入: [-1,-100,3,99] 和 k = 2
输出: [3,99,-1,-100]
解释: 
向右旋转 1 步: [99,-1,-100,3]
向右旋转 2 步: [3,99,-1,-100]
```

#### 说明:

尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。

要求使用空间复杂度为 `O(1)` 的原地算法。


*** *** *** ***

## 解法1：多次右移

模拟向右移动的过程，每次移动一个位置，共移动 `k` 次。

在移动过程中，考虑两种特殊情况：

1. 向右移动 `nums.length` 次会将使数组同原始顺序一致，因此只需要移动 `k % nums.length` 次。若 `k % nums.length == 0` 则无需移动。
2. 当 `nums.length <= 1` 时，无论移动多少次，数组都不会改变，可以直接返回。

在以下解法中，都将首先考虑这两种特殊情况。

注意，解法一会因*超出时间限制（TLE，Time Limit Exceeded）*无法通过 leetcode 测试。

```javascript
var solution = function(nums, k) {
    var step = function(nums) {
        if (nums.length <= 1) { return; }
        var t = nums[nums.length - 1];
        for (var i = nums.length - 1; i >= 1; i--) {
            nums[i] = nums[i - 1];
        }
        nums[0] = t;
    };
    
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    k = k % nums.length;
    for (var i = 0; i < k; i++) {
        step(nums);
    }
};
```

#### 输入
<textarea id="problemInputStep" rows="3" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionStep', 'problemInputStep', 'problemOutputStep')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputStep', '[1,2,3,4,5,6,7]\n3')">重置输入</button>
</span>

#### 输出
<p id="problemOutputStep">NULL</p>


*** *** *** ***

## 解法2：一步到位

对于第 `i` 个元素，其旋转 `k` 个位置后的新位置为 `(i + k) % nums.length`。

同时借助 `O(n)` 额外空间，将原有数组中的数据复制到新数组中。

```javascript
var solution = function(nums, k) {
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    origin = nums.slice(0);
    for (var i = 0; i < nums.length; i++ ) {
        n = (i + k) % nums.length;
        nums[n] = origin[i];
    }
};
```

#### 输入
<textarea id="problemInputCopy" rows="3" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionCopy', 'problemInputCopy', 'problemOutputCopy')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputCopy', '[1,2,3,4,5,6,7]\n3')">重置输入</button>
</span>

#### 输出
<p id="problemOutputCopy">NULL</p>


*** *** *** ***

## 解法3：递推调换

在解法2的基础上，尝试不使用额外空间就衍生出了解法3。

其思想为，由当前元素调换后的位置决定下一个调换的元素。假设当前正在调换第 `i` 号元素（原数组中编号，下同）并被调换到第 `n` 号元素的位置，则下一次调换第 `n` 号元素。以此类推，便可只记录调换后元素的初始值（即第 `n` 号元素的初始值），便可完成全部调换。最终数组中每个元素都被调换。因此可使用计数器记录调换的次数，当调换次数等同于数组长度 `nums.length`，则结束递推。

需注意，调换中可能出现环，如果原数组 `[1,2,3,4,5,6]` 移动 `k = 3` 次，则会出现 `1-4-1`、`2-5-2`、`3-6-3` 的情况。因而需记录调换的起始点 `s`，当新调换位置 `n == s` 则表示回到起始点，需选择新的起始点继续调换。由上例可以猜测，可选择 `s + 1` 作为新的起始点，且在递推结束前新起始点不会出现在已调换的环中。简要证明如下：

> 令当前起始点编号为 `s` 且编号 `0` 到 `s` 元素为起始点的环中未出现重复元素。假设 `s + 1` 号元素曾出现在已有环中，但此时尚未遍历所有元素。不妨令 `s + 1` 出现在起始点为 `t (0 <= t <= s)` 的环中，则 `s + 2` 也一定出现在起始点为 `(t + 1) % (s + 1)` 的环中（每个环的编号都是上一个环编号递推加一的结果）。以此类推， `s + 1` 后所有元素都出现在已有环中，已经遍历所有元素，与假设矛盾。

```javascript
var solution = function(nums, k) {
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    var s = 0;        // start index
    var n = s;        // next index
    var count = 0;    // total count
    
    var t = nums[s];  // temp data
    while (count < nums.length) {
        n = (n + k) % nums.length;
        [nums[n], t] = [t, nums[n]];
        count += 1;
        
        if (s == n) {
            s += 1;
            n = s;
            t = nums[n];
        }
    }
};
```

#### 输入
<textarea id="problemInputJuggling" rows="3" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionJuggling', 'problemInputJuggling', 'problemOutputJuggling')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputJuggling', '[1,2,3,4,5,6,7]\n3')">重置输入</button>
</span>

#### 输出
<p id="problemOutputJuggling">NULL</p>

### 解法3：扩展说明

事实上，解法3也被称作循环置换（cyclic replacement）或杂耍算法（juggling）。可以通过数学计算与证明，得到环的数量、以及每个环的长度。先给出结果，环的数量为 `GCD(k, nums.length)` （最大公约数），每个环的长度为 `LCM(k, nums.length) / k` （最小公倍数）。简单证明如下（为简化书写，用 `l` 代替 `nums.length`）：

首先，证明每个环的长度。在解法3的调换序列中，必然存在环（即便环的数量为1，最终也会回到起始元素）。将第1号元素作为起点，其环中元素序列可以表示为 `[0 % l, k % l,  2k % l, .. , Nk % l]`，其中 `N` 代表环的长度，最终 `Nk % l == 0 % l`。为了求环的长度，需要令 `Nk` 为 `l` 的最小倍数，那么 'N' 的最小值即为 `LCM(k, l) / k`。

其次，证明环的数量。假如 `k` 和 `l` 互质，则只存在一个环（数组为完全剩余系）。若 `k` 和 `l` 不互质，则构造 `k' = k / GCD(k, l)` 和 `l' = l / GCD(k, l)`，使得 `k'` 和 `l'` 互质。这意味着可以把整个数组划分成多个长度为 `GCD(k, l)` 的块。如果按块移动数组，则可保证只存在一个环。由此可推断，一次移动一个元素，环的长度为 `GCD(k, l)`。

相关内容可以参考 [leetcode][1]、[geeks][2]，[StackOverflow][3]。

[1]: (https://leetcode.com/problems/rotate-array/solution/
[2]: https://www.geeksforgeeks.org/array-rotation/
[3]: https://stackoverflow.com/questions/23321216/rotating-an-array-using-juggling-algorithm


*** *** *** ***

## 解法4：块翻转

假定 `0 < k < nums.length`，将数组右移 `k` 位事实上等同于将后 `k` 个元素放到数组最前方，等同于将以 `nums.length - k` 为分界点的两个块调换位置。

+ 1 2 3 - 4 5 6 7 *(raw)*
+ 4 5 6 7 - 1 2 3 *(k = 4)*
+ 7 6 5 4 - 3 2 1 *(reverse)*

如上所示，当 `k = 4` 时 `[7 - 4, 7)` 被放置到数组最前端（第2行），也等同于将 `[0, 3)` 同 `[3, 7)` 的块调换位置。

因而，可巧妙的将数组整体翻转，令后方元素被放置在前方（第3行）。注意此时每个块中的数字顺序是反的，需要对每个块内分别翻转，即可得到最终结果。

```javascript
var solution = function(nums, k) {
    var reverse = function(ns, si, ei) {
        if (ns.length <= 1 || ei - si <= 1) { return; }
        for (var i = 0; i < (ei - si) / 2; i++) {
            ti = si + i;
            tj = ei - i - 1;
            [ns[ti], ns[tj]] = [ns[tj], ns[ti]];
        }
    };
    
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    k = k % nums.length;
    reverse(nums, 0, nums.length);
    reverse(nums, 0, k);
    reverse(nums, k, nums.length);
};
```

#### 输入
<textarea id="problemInputReverse" rows="3" cols="80%"></textarea>

<p></p>
<span>
<button onclick="problemRun('solutionReverse', 'problemInputReverse', 'problemOutputReverse')">计算结果</button>
<button class="problemInit" onclick="resetInput('problemInputReverse', '[1,2,3,4,5,6,7]\n3')">重置输入</button>
</span>

#### 输出
<p id="problemOutputReverse">NULL</p>


*** *** ***
###### [Back](./index)

<script id="jsSolutions" type="text/javascript">
var solutionStep = function(nums, k) {
    var step = function(nums) {
        if (nums.length <= 1) { return; }
        var t = nums[nums.length - 1];
        for (var i = nums.length - 1; i >= 1; i--) {
            nums[i] = nums[i - 1];
        }
        nums[0] = t;
    };
    
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    k = k % nums.length;
    for (var i = 0; i < k; i++) {
        step(nums);
    }
};

var solutionCopy = function(nums, k) {
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    origin = nums.slice(0);
    for (var i = 0; i < nums.length; i++ ) {
        n = (i + k) % nums.length;
        nums[n] = origin[i];
    }
};

var solutionJuggling = function(nums, k) {
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    var s = 0;
    var n = s;
    var count = 0;
    
    var t = nums[s];
    while (count < nums.length) {
        n = (n + k) % nums.length;
        [nums[n], t] = [t, nums[n]];
        count += 1;
        
        if (s == n) {
            s += 1;
            n = s;
            t = nums[n];
        }
    }
};

var solutionReverse = function(nums, k) {
    var reverse = function(ns, si, ei) {
        if (ns.length <= 1 || ei - si <= 1) { return; }
        for (var i = 0; i < (ei - si) / 2; i++) {
            ti = si + i;
            tj = ei - i - 1;
            t = ns[ti];
            ns[ti] = ns[tj];
            ns[tj] = t;
        }
    };
    
    if (nums.length <= 1 || k % nums.length == 0) { return; }
    
    k = k % nums.length;
    reverse(nums, 0, nums.length);
    reverse(nums, 0, k);
    reverse(nums, k, nums.length);
};
</script>

<script id="jsUtils" type="text/javascript">
var utilTrim = function(s) { return s.replace(/(^\s*)|(\s*$)/g, ""); };
var utilIsInt = function(i) { return (typeof i === "number" && i % 1 === 0) }
</script>

<script id="jsInterface" type="text/javascript">
var parseInput = function(lines) {
    if (!(lines instanceof Array && lines.length == 2)) { return false; }
    try {
        nums = eval(lines[0]);
        k = eval(lines[1]);
    } catch (err) { return false; }
    if (!(nums instanceof Array)) { return false; }
    for (var i = 0; i < nums.length; i++) { if (!utilIsInt(nums[i])) { return false; } }
    if (!utilIsInt(k)) { return false; }
    return [nums, k];
};

var resetInput = function(inputName, inputValue) {
    var i = document.getElementById(inputName);
    i.value = inputValue;
}

var formatOutput = function(result) {
    return "[" + result.join(",") + "]";
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
        var nums = r[0];
        var k = r[1];
        
        // execute solution
        eval(solutionName + "(nums, k);");
        var r = formatOutput(nums);
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
