# PostgreSQL Notes



**fastjson 1.2.24之前的版本 有安全漏洞**


##Spring + fastjson 

1. pom.xml加入 fastjson
2. springmvc.xml 配置fastjson
3. 



- Postgresql 值类型对应java 类型



1 data_type_id Data Type Id java.lang.Integer int4 11
2 smallint_type Smallint Type java.lang.Integer int2 6
3 int_type Int Type java.lang.Integer int4 11

-  bigint_type Bigint Type   ：：：  java.lang.Long int8 20

5 decimal_type Decimal Type java.math.BigDecimal numeric 18
6 numeric_type Numeric Type java.math.BigDecimal numeric 12
7 real_type Real Type java.lang.Float float4 14
8 doubleprecision_type Doubleprecision Type java.lang.Double float8 24
9 serial_type Serial Type java.lang.Integer int4 11
10 bigserial_type Bigserial Type java.lang.Long int8 20
11 varchar_type Varchar Type java.lang.String varchar 30
12 char_type Char Type java.lang.String bpchar 30
13 text_type Text Type java.lang.String text 2147483647
14 bytea_type Bytea Type [B bytea 2147483647
15 date_type Date Type java.sql.Date date 13
16 time_type Time Type java.sql.Time time 15
17 timetz_type Timetz Type java.sql.Time timetz 21
18 timestamp_type Timestamp Type java.sql.Timestamp timestamp 29
19 timestamptz_type Timestamptz Type java.sql.Timestamp timestamptz 35
20 interval_type Interval Type org.postgresql.util.PGInterval interval 49
21 boolean_type Boolean Type java.lang.Boolean bool 1

22 point_type Point Type org.postgresql.geometric.PGpoint point 2147483647 23
linesegment_type Linesegment Type org.postgresql.geometric.PGlseg lseg
2147483647

24 box_type Box Type org.postgresql.geometric.PGbox box 2147483647
25 path_type Path Type org.postgresql.geometric.PGpath path 2147483647

26 polygon_type Polygon Type org.postgresql.geometric.PGpolygon polygon
2147483647 27 circle_type Circle Type org.postgresql.geometric.PGcircle
circle 2147483647

28 cidr_type Cidr Type java.lang.Object cidr 2147483647
29 inet_type Inet Type java.lang.Object inet 2147483647
30 macaddr_type Macaddr Type java.lang.Object macaddr 2147483647
31 bit2_type Bit2 Type java.lang.Boolean bit 2
32 bitvarying5_type Bitvarying5 Type java.lang.Object varbit 5


Where is the most recent list?

Thank you!
Dave

P.S.
If these are not already in the information_schema (or similar), I think
they would make for a worthwhile addition.


CHARACTER       String
VARCHAR         String
LONGVARCHAR     String
NUMERIC         java.math.BigDecimal
DECIMAL         java.math.BigDecimal
BIT boolean     Boolean
TINYINT byte    Integer
SMALLINT    short   Integer
INTEGER int Integer
BIGINT  long    Long
REAL    float   Float
FLOAT   double  Double
DOUBLE PRECISION    double  Double
BINARY      byte[]
VARBINARY       byte[]
LONGVARBINARY       byte[]
DATE        java.sql.Date
TIME        java.sql.Time
TIMESTAMP       java.sql.Timestamp



[PostgreSQL 9.3.16 Documentation-Numeric Types](https://www.postgresql.org/docs/9.3/static/datatype-numeric.html)
[PostgreSQL学习手册(常用数据类型) ](http://blog.itpub.net/9521459/viewspace-759337)
[深入浅出理解Postgres中的内存管理](https://www.sdk.cn/news/3659)
[PostgreSQL数据类型](http://www.yiibai.com/postgresql/2013080435.html)
[PostgreSQL模式Schema](http://www.yiibai.com/html/postgresql/2013/080441.html)
[]()
[]()

[PostgreSQL学习手册(索引)](http://www.cnblogs.com/stephen-liu74/archive/2012/05/09/2298182.html)
[PostgreSQL学习手册(目录)](http://www.cnblogs.com/stephen-liu74/archive/2012/06/08/2315679.html)

[]()

