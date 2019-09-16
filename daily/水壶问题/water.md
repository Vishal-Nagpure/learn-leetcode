# 毎日一题 -  水壶问题

## 信息卡片

* 时间：2019-09-15
* 题目链接：https://leetcode-cn.com/problems/water-and-jug-problem/
* tag：Math  
## 题目描述
```
给你一个装满水的 8 升满壶和两个分别是 5 升、3 升的空壶，请想个优雅的办法，使得其中一个水壶恰好装 4 升水，每一步的操作只能是倒空或倒满。
```

## 参考答案
上面的问题是一个特例，我们可以抽象为[leetcode-365-水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)。

有两个容量分别为 x升 和 y升 的水壶（壶1，壶2）以及无限多的水。请判断能否通过使用这两个水壶，从而可以得到恰好 z升 的水？
解题核心思路（x < y，即壶1容量小于壶2，x == y的情况后面讨论）：
1. 将壶2倒满，往壶1倒入至满。
2. 若壶1满，记录当前壶2中新水量。壶1倒出，将壶2中剩余的继续往壶1中倒入；（当壶1满，继续此操作，并记录当前壶2中新水量nw， 若此新水量已被记录，则）。
3. 若出现壶1不满时（即此时壶2必空），重复操作1。

开辟一个新数组nws记录所有新水量，对任意nws[i]，可构造的水量为nws[i]，nws[i]+x，nws[i]+x。
（其实不需要新数组，因为数学上可以证明新水量的值循环周期呈现，故可以使用一个临时变量cur，当cur==x为终止条件）

数学证明新水量nw值是循环周期的：
![](111.jpg)




1.暴力求解，时间复杂度O(n^2)
>
>我们可以一次遍历gas，对于每一个gas我们依次遍历后面的gas，计算remian，如果remain一旦小于0，就说明不行，我们继续遍历下一个
```js
// bad 时间复杂度0（n^2）
let remain = 0;
const n = gas.length;
for (let i = 0; i < gas.length;i++){
    remain += gas[i];
    remain -= cost[i];
    let count = 0;
    while (remain >= 0){
        count++;
        if (coun === n ) return i;
        remain += gas[getIndex(i + count,n)];
        remain -= cost[getIndex(i + count,n)];
    }
    remain = 0
}
retirn -1;
```
2.比较巧妙的方法，时间复杂度是O(n)
>
>这个方法基于两点：
>
>2-1:如果站点i到达站点j走不通,那么从i到j之间的站点（比如k）出发一定都走不通。前提i（以及i到k之间）不会拖累总体（即remain >= 0）。
>2-2:如果cost总和大于gas总和，无论如何也无法走到终点，这个比较好理解。因此假如存在一个站点出发能够到达终点，其实就说明cost总和一定小于等于gas总和
>
```js
const n = gas.length;
let total = 0;
let remain = 0;
let start = 0;
for(let i = 0; i < n; i++){
    total += gas[i];
    total += cost[i]
    remain += gas[i];
    remain -= cost[i];
    // 如果remain < 0,说明从start到i走不通
    // 并且从start到i走不通，那么所有的solution中包含start到i的肯定都走不通
    // 因此我们重新从i + 1开始作为start
    if (remain < 0){
        remain = 0;
        start = i + 1;
    }
}
// 事实上，我们遍历一遍，也就确定了每一个元素作为start是否可以走完一圈
// 如果costu总和大于gas总和，无论如何也无法走到终点
return total >= 0? start : -1;
```
## 优秀解答

>暂缺