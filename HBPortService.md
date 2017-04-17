# HB Port Service



http://trans.hb56.com:8011/api/General/PostDataDW?customer=YUNLSP&KeyId=01dde226-ca25-4135-ab34-a8a1ab80ad12
```
{
  "发送方": "YUNLSP",
  "是否重发": "N",
  "FILETYPE": "BILLNO",
  "提单号": "PCLU1707CK5000",
  "船名": "PANCON SUNSHINE",
  "航次": "1707E",
  "进出标识": "E"
}
```
```
200, OK
Pragma: no-cache
Date: Tue, 28 Mar 2017 01:57:19 GMT
Via: 1.0 lwin22 ()
X-CorrelationID: Id-00c3d958cce3c97ce1b85477 0
Server: Microsoft-IIS/7.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
Allow: OPTIONS, POST
Content-Type: application/json; charset=utf-8
Access-Control-Allow-Origin: chrome-extension://ecjfcmddigpdlehfhdnnnhfgihkmejin
Access-Control-Expose-Headers: x-correlationid
Cache-Control: no-cache
Connection: close
Max-Forwards: 20
Expires: -1
```


http://trans.hb56.com:8011/api/General/GetDataDW?customer=YUNLSP&KeyId=01dde226-ca25-4135-ab34-a8a1ab80ad12


"提单号": "HLCUSHA1703JGMH1"
{
   "dt0": [
      {
         "KEY1": "510030419621991",
         "KEY2": "5100304547209",
         "箱标识": "510030419621991",
         "提单号": "HLCUSHA1703JGMH1",
         "码头": "振东",
         "箱号": "FCIU2104112",
         "尺寸箱型": "20GP",
         "箱高度": "PQ",
         "持箱人": "HLC",
         "箱状态": "出口重箱",
         "船名": "HYUNDAI MERCURY",
         "航次": "057E",
         "码头船名": "HYUNDAI MERCURY",
         "码头航次": "057E",
         "进出口标志": "E",
         "铅封号": "HLD3408311",
         "卸货港": "CAVA5",
         "目的港": "CAVA5",
         "冷藏箱温度": " ",
         "超限": null,
         "超限长度": "///",
         "危险品等级": null,
         "联合国编号": null,
         "箱皮重": 2220,
         "箱总重": 7183,
         "进港时间": "2017-03-27T21:12:53",
         "运抵报文发送时间": "2017-03-27T21:10:27",
         "出港时间": null,
         "分件数": 373,
         "分重量": 4963.4,
         "分体积": 27.4,
         "场箱位": "1C4531",
         "坏污标志": null,
         "装货港": "CNSHA",
         "CUD": "I",
         "操作者": "DCST",
         "FILENAME": "WS_DW_CONTAINERS",
         "EWL_ID": "1697516"
      }
   ],
   "dt1": [
      {
         "KEY1": "41282510030435646303",
         "箱标识": "510030419621991",
         "箱号": "FCIU2104112",
         "作业方式代码": "LL",
         "作业方式名称": "收到装箱单",
         "作业时间": "2017-03-27T17:05:22",
         "作业码头": "振东",
         "备注": null,
         "CUD": "I",
         "操作者": "DCST",
         "FILENAME": "WS_DW_CONACT",
         "EWL_ID": "1697517"
      }
   ],
   "dt2": [
      {
         "KEY1": "41282510030435646649",
         "箱标识": "510030419621991",
         "箱号": "FCIU2104112",
         "作业方式代码": "LP",
         "作业方式名称": "装箱单处理",
         "作业时间": "2017-03-27T17:05:23",
         "作业码头": "振东",
         "备注": null,
         "CUD": "I",
         "操作者": "DCST",
         "FILENAME": "WS_DW_CONACT",
         "EWL_ID": "1697518"
      }
   ],
   "dt3": [
      {
         "KEY1": "41282510030435659315",
         "箱标识": "510030419621991",
         "箱号": "FCIU2104112",
         "作业方式代码": "I",
         "作业方式名称": "进场",
         "作业时间": "2017-03-27T21:10:27",
         "作业码头": "振东",
         "备注": null,
         "CUD": "I",
         "操作者": "DCST",
         "FILENAME": "WS_DW_CONACT",
         "EWL_ID": "1697519"
      }
   ],
   "dt4": [
      {
         "KEY1": "41282510030435659424",
         "箱标识": "510030419621991",
         "箱号": "FCIU2104112",
         "作业方式代码": "S",
         "作业方式名称": "运抵",
         "作业时间": "2017-03-27T21:18:09",
         "作业码头": "振东",
         "备注": null,
         "CUD": "I",
         "操作者": "DCST",
         "FILENAME": "WS_DW_CONACT",
         "EWL_ID": "1697520"
      }
   ]
}


回执数据

http://trans.hb56.com:8011/api/General/PostGetFlagDW?customer=YUNLSP&KeyId=01dde226-ca25-4135-ab34-a8a1ab80ad12

报文格式：
[
  {
    "EWL_ID": "1032563",
    "EWL_ERRORMSG": "OK",
    "EWL_FEEDBACKTM": "2017-03-19 00:01:29"
  },
  {
    "EWL_ID": "1032564",
    "EWL_ERRORMSG": "OK",
    "EWL_FEEDBACKTM": "2017-03-19 00:01:29"
  },
  {
    "EWL_ID": "1032565",
    "EWL_ERRORMSG": "OK",
    "EWL_FEEDBACKTM": "2017-03-19 00:01:29"
  }
]


EWL_ID为步骤2）中获取的报文数据的ID；
EWL_ERRORMSG为报错信息，如果没有报错值为“OK”，有报错可返回其他值；
EWL_FEEDBACKTM为回执时间



