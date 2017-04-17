##Java String



str==null   ||   str.equals(""))    (注意顺序)
  再澄清一个概念：  
  如果str==null说明str还未定义内容。此时，谈不上是否为空。  
  str=""，说明str是个空字符串。只不过长度为0。



  ==是用来判断对象句柄地址的。说明s还未定义内容。此时，谈不上是否为空。     
  equal是用来判断句柄内容的。  
  想要实现equal的效果可以使用这样  
  s.intern=="".intern

```
String s;  
  if(s==null) {  
    //为null;  
  }  
  if(s.equals("")) {  
  //为空字符串;  
  }  
  if(s.length()==0) {  
  //为空字符串;  
  }


```


[java String isEmpty() 判断字符串是否为空？](https://www.zhihu.com/question/20570393)
