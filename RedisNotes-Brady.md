# Redis Notes

5种数据结构
- String
- List
- Set
- Hash
- ZSET 有序集合

====================

价格表 就是一个 table  hash table来存储 


JedisPoolConfig config = new JedisPoolConfig();
  
//连接耗尽时是否阻塞, false报异常,ture阻塞直到超时, 默认true
config.setBlockWhenExhausted(true);
  
//设置的逐出策略类名, 默认DefaultEvictionPolicy(当连接超过最大空闲时间,或连接数超过最大空闲连接数)
config.setEvictionPolicyClassName("org.apache.commons.pool2.impl.DefaultEvictionPolicy");
  
//是否启用pool的jmx管理功能, 默认true
config.setJmxEnabled(true);
  
//MBean ObjectName = new ObjectName("org.apache.commons.pool2:type=GenericObjectPool,name=" + "pool" + i); 默认为"pool", JMX不熟,具体不知道是干啥的...默认就好.
config.setJmxNamePrefix("pool");
  
//是否启用后进先出, 默认true
config.setLifo(true);
  
//最大空闲连接数, 默认8个
config.setMaxIdle(8);
  
//最大连接数, 默认8个
config.setMaxTotal(8);
  
//获取连接时的最大等待毫秒数(如果设置为阻塞时BlockWhenExhausted),如果超时就抛异常, 小于零:阻塞不确定的时间,  默认-1
config.setMaxWaitMillis(-1);
  
//逐出连接的最小空闲时间 默认1800000毫秒(30分钟)
config.setMinEvictableIdleTimeMillis(1800000);
  
//最小空闲连接数, 默认0
config.setMinIdle(0);
  
//每次逐出检查时 逐出的最大数目 如果为负数就是 : 1/abs(n), 默认3
config.setNumTestsPerEvictionRun(3);
  
//对象空闲多久后逐出, 当空闲时间>该值 且 空闲连接>最大空闲数 时直接逐出,不再根据MinEvictableIdleTimeMillis判断  (默认逐出策略)   
config.setSoftMinEvictableIdleTimeMillis(1800000);
  
//在获取连接的时候检查有效性, 默认false
config.setTestOnBorrow(false);
  
//在空闲时检查有效性, 默认false
config.setTestWhileIdle(false);
  
//逐出扫描的时间间隔(毫秒) 如果为负数,则不运行逐出线程, 默认-1
config.setTimeBetweenEvictionRunsMillis(-1);
  
JedisPool pool = new JedisPool(config, "localhost");


###参考资料###

Jedis对redis的操作详解
http://blog.csdn.net/u013256816/article/details/51125842
Redis入门很简单之六【Jedis常见操作】
http://hello-nick-xu.iteye.com/blog/2077243?utm_source=tuicool&utm_medium=referral

使用Spring + Jedis集成Redis

https://my.oschina.net/u/866380/blog/521658

xetorthio/jedis
https://github.com/xetorthio/jedis/

Jedis使用示例
http://javacrazyer.iteye.com/blog/1840161


深入Jedis
https://blog.huachao.me/2016/2/%E6%B7%B1%E5%85%A5Jedis/?utm_source=tuicool&utm_medium=referral

http://snowolf.iteye.com/blog/1667104


JedisPool异常Jedis链接处理
http://www.cnblogs.com/wcd144140/p/4883139.html?utm_source=tuicool&utm_medium=referral


Spring+Jedis+Redis自定义模板实现缓存Object对象
http://wujiu.iteye.com/blog/2217580
Redis入门 – Jedis存储Java对象 - (Java序列化为byte数组方式)
http://alanland.iteye.com/blog/1600685

http://xetorthio.github.io/jedis/


Jedis常用命令和系统命令
http://sgq0085.iteye.com/blog/2181348?utm_source=tuicool&utm_medium=referral

jedis使用线程池封装redis基本操作
http://blog.csdn.net/a67474506/article/details/40660031?utm_source=tuicool&utm_medium=referral

jedis连接池详解(Redis)
http://tianxingzhe.blog.51cto.com/3390077/1684306
Redis学习记录(二)--使用Jedis连接
http://www.jianshu.com/p/c3b8180af944

