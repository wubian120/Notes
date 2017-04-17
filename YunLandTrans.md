## YunLand Finance

HaoMall

hm_Fe_Funds : 资金：包括实收实付； 43个字段

hm_Fe_Invoice ： 发票                 共：65个字段
hm_Fe_InvoiceDetail： 发票明细    共13个字段

hm_Fe_FundsDetail ： 资金明细   共2个字段

hm_Fee_ActionTmp  ： 只有两项，费用项和费用金额

hm_Fr_FeeBillListTmpBak ： 无字段说明， 共34个字段

hm_Order_Fee ：    订单费用  69个字段
hm_Fr_Inquiry_Fee：   详细报价  共33个字段

hm_Order_Bill ： 订单  154个字段


**dbo.sp_HelpTbdesc 'hm_Fe_InvoiceDetail'**

Need:

order_fee:  已有  单票 费用 明细  也就是 应收

order_fee_received 实收

invoice_request   开票申请

fe_invoice      发票， 参考 hm_Fe_Invoice

收到付款 之后 做什么？ 哪些动作 ？
核对费用。
需要将 付款 与那几笔 订单  明细 关联起来。

结算单， 将 发票号与订单 号  一对多 关联起来。 

凭证审核
fe_receival
fe_payment   付款
  ///

fe_voucher  凭证号， 凭证日期，凭证金额。

凭证与发票的 关系  一对一  一对多 

凭证与订单的关系    一对多，  多对多
结算单 与  订单的关系 。 

百信库
BestLOG8_HHW 中  表 Gl_Voucher  Gl_VoucherClass   Gl_VoucherDetail  Gl_VoucherFeeLink


需要解决问题：
点击生成凭证  从哪张表 读数据  写到  凭证表 ？ 都要写哪些数据 ？



fe_invoice  表结构


invoice_type     发票类型： ？？ 都有哪些类型？


invoice_code             发票代码
invoice_no               发票号码

结算单 fe_statement   百信 haomall 有没有相关表
对账单  

对应 陆运系统 里的 lo_fe_accounting


船东 提单号  mbl

海管家 提单号   hbl

fi_order_info     订单信息表  只读

order_id        订单GUID
order_no        订单号
customer_name   客户名称
customer_code   往来户代码
voyage          航次
vessel          船名
discharge_port  目的港
cargo_ready_date  货好时间（做箱时间）
container_volume   箱型箱量（合计）1*20GP+2*40HC
package           件数
weight           毛重
measure          体积
account_period   帐期  月结，票结
created_time      订单创建时间


fi_order_fee      订单相关费用   读写

order_fee_id       费用GUID
order_id           订单GUID
received_or_paid   应收 or 应付
fee_name           费用名称
currency           币别
fee_code           费用代码
price              价格 （含税）
rate               比率 （税率？）
fee_count          费用数量
fee_amount         实际金额






fi_order_info  : fi_order_fee  一对多




fi_receival       应收

fi_payment        应付

fi_invoice        发票

fi_voucher        凭证

fi_request        请求

fi_accounting     账单

- 陆运系统有结算单

### 后端业务逻辑

1. 接收来自陆运平台的 订单费用 信息。 



2. 接收陆运平台 发送的结算请求
3. 接收 前端 发送的  计费管理的访问 请求 返回 所有的 订单计费 信息。 订单计费信息 是综合信息 由 ‘订单基本信息’与‘订单费用信息’ 以及 ‘发票信息’组成
4. 税率 在 陆运系统 生成结算单时 指定
5. 凭证与费用的关系    
6. 发票与费用的关系   一对多
7. 应收付 包含 实收付
8. 核销 与 费用       一对多 

订单计费信息    //页面 共25个字段
[order_fee]费用类型fee_code，[order_fee status]操作状态，[order_fee fee_name]费用名称，[order_info  order_no]订单号，[c]往来户名称，[c]船名，[order_info]航次，[order_info]目的港[]，
[order_info cargo_ready_date]做箱日，[order_info ]箱型箱量， []件数，[]毛重，[]体积，
[order_info.accountPeriod]客户帐期，发票号码，发票日期， 币别， 应收/应付不含税金额， 税率， 税额， 应收应付价税合计。预计收/付款日， 实收/付日， 凭证号码，凭证日期，撤销人员， 撤销时间。

### 业务流程
                  应收   ->  开发票  
       核算 ->                          ->核销(实收核销)(实付核销)     ->凭证 -> 结束
                  应付   ->  收开票           

陆运平台 -> 核算请求（订单费用）   [订单费用存入db]； 开票请求（应收信息）;   付款请求（应付信息）；

状态：核算通知（接受/未接受），应收（开票审批，待开票，待打印，核销），应付（接收发票，付款审核，付款，核销），凭证 （未建凭证，已建凭证）      


客户结算单 ： 订单 1对多     
客户结算通知单  是否 包含 订单费用明细  ？

付款结算通知单  ：订单    1对 多   

费用核算请求
应收开票请求
应付付款请求


陆运平台 ->  发送   订单费用
        ->  发送    


### TO DO 

[d] 表结构确定
[d] JPA 连接测试， 框架构建
[]  陆运财务计算API
[]  









[RESTful API 设计指南](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)

[hibernate/jpa 注解 注释到数据库（MySql）](http://blog.csdn.net/xiaobangsky/article/details/17675015?locationNum=2)



[JPA criteria 查询:类型安全与面向对象](https://my.oschina.net/zhaoqian/blog/133500)

[JPA 2.0 Criteria Query with Hibernate](https://www.javacodegeeks.com/2013/04/jpa-2-0-criteria-query-with-hibernate.html)