# 基础知识
## isset()
定义：检查变量是否已设置，**且不为 NULL**
说明：bool isset(mixed $var [, mixed $……])
返回：如果变量 $var 已设置且不为 NULL 则返回 TRUE，否则返回 FALSE。如果有多个变量，则全部变量已设置且不为 NULL时才返回 TRUE，否则为 FALSE。
## empty()
定义：检查一个变量是否为空
说明：bool empty(mixed $var)
返回：当 $var 存在且为非空非零则返回 FALSE，否则返回 TRUE。empty() 等价于 !isset($var) || $var == false
其中，等价于 false 的值有：
- ""
- 0
- 0.0
- "0"
- NULL
- FALSE
- array()：空数组
- $var：一个声明了但是没有值的变量
*注意：*变量不存在时并不会产生警告；变量值为若干个空格时返回 FALSE。
## foreach 和 引用
引用：如果想在遍历数组的过程中修改数组的元素，可以在 foreach 中对 $v 使用引用。此时被引用的元素 $v 指向当前数组元素的内存地址，即共享一段内存地址。因此修改 $v 的值会同时改变 $arr[$k] 的值。
```
$arr = [1, 2, 3, 4];
foreach ($arr as $k => &$v) {
}
var_dump($arr);
unset($v);
$v = 'a';
var_dump($arr);
foreach ($arr as $k => &$v) {
}
var_dump($arr);
$v = "b";
var_dump($arr);
```
返回结果：
```
array(4) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>int(3)
  [3]=>&int(4) // 注意此处的引用，说明对 $v 赋值会改变 $arr 的最后一个变量
}
array(4) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>int(3)
  [3]=>int(4) // 把 $v unset 掉，然后赋值，将不会改变最后一个值
}
array(4) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>int(3)
  [3]=>&int(4) // 同第一个结果
}
array(4) {
  [0]=>int(1)
  [1]=>int(2)
  [2]=>int(3)
  [3]=>&string(1) "b" // 此时 $v 仍是指针状态，被赋值后可更改数组的最后一个键的值
}
```
