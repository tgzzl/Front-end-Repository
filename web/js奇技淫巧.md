## [JS奇技淫巧](https://github.com/jawil/blog/issues/24)

- 取整 \
var a = ~~2.33 // 2 \
var b= 2.33 | 0 // 2 \
var c= 2.33 >> 0 // 2
- 金钱格式化 \
var a = '1234567890' \
a.replace(/\B(?=(\d{3})+(?!\d))/g, ',') \
(+a).toLocaleString()
- 两个整数交换数值 \
a ^= b; \
b ^= a; \
a ^= b;
-取出一个数组中的最大值和最小值 \
var numbers = [5, 458 , 120 , -215 , 228 , 400 , 122205, -85411]; \
Math.max.apply(Math, numbers); \
Math.max(...numbers);
-
