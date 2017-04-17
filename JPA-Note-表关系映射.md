## JPA Note 表关系映射

### 单向多对一
JPA中的@ManyToOne
主要属性 
- name(必需): 设定“many”方所包含的“one”方所对应的持久化类的属性名 
- column（可选）： 设定one方的主键，即持久化类的属性对应的表的外键 
- class（可选）： 设定one方对应的持久化类的名称，即持久化类属性的类型 
- not-null（可选）： 如果为true,，表示需要建立相互关联的两个表之间的外键约束 
- cascade（可选）： 级联操作选项，默认为none

单向多对一(@ManyToOne)关联是最常见的单向关联关系。假设多种商品(Product)可以有一个商品类型(ProductType),只关心商品(Product)实体找到对应的商品类型(ProductType)实体，而无须关心从某个商品类型(ProductType)找到全部商品(Product).

```
CREATE TABLE `t_product_type` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

CREATE TABLE `t_product` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `type_id` bigint(20) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FK_oyt6r2g6hwbyee5adel4yj59e` (`type_id`),
  CONSTRAINT `FK_oyt6r2g6hwbyee5adel4yj59e` FOREIGN KEY (`type_id`) REFERENCES `t_product_type` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
```

[JPA 菜鸟教程 3 单向多对一](http://blog.csdn.net/je_ge/article/details/53493897)
[]()
[]()