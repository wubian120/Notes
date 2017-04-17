# DataStructure Hash Table




### 除法散列法

h(k) = k mod m
k为槽的数量  m 数组的大小 

mod 取余



> %与mod的区别：%出来的数有正有负，符号取决于左操作数。。。而mod只能是正（因为a = b * q + r (q > 0 and 0 <= r < q), then we have a mod q = r 中r要大于等于0小于q）
>所以要用%来计算mod的话就要用这样的公式：a mod b = (a % b + b) % b
>括号里的目的是把左操作数转成正数
>绝大多数取模运算都会满足   r = a - (a/b) * b。


### 乘法散列法

### 参考资料


$$ cahhh