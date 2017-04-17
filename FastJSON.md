#FastJSON Notes



##FastJson 多层嵌套
```
//Response.java
public class Response {

    private String type;
    private CommonResponse common;
    private Object object = new Object();

    public Object getObject() {return object;}
    public void setObject(Object object) {this.object = object;}

    public CommonResponse getCommon() {return common;}
    public void setCommon(CommonResponse common) {this.common = common;}

    public String getType() {return type;}
    public void setType(String type) {this.type = type;}

    public String toJSONString(){
        return JSON.toJSONString(this, SerializerFeature.WriteNullListAsEmpty);    }

    public static void main(String[] args){

        JsonData d1 = new JsonData();
        d1.setI(1);
        d1.setD(new Date());
        d1.setS("d1");

        JsonData d2 = new JsonData();
        d2.setI(2);
        d2.setD(new Date());
        d2.setS("d2");

        JsonData d3 = new JsonData();
        d3.setI(3);
        d3.setD(new Date());
        d3.setS("d3");

        List<Object> objects = new JSONArray();
        CommonResponse common = new CommonResponse();
        common.setCode("1001");
        common.setMessage("he he he");

        objects.add(JSON.toJSON(d1));
        objects.add(JSON.toJSON(d2));
        objects.add(JSON.toJSON(d3));

        Response response = new Response();
        response.setCommon(common);
        response.setObject(objects);
//        response.setData(objects);
//        String out = JSON.toJSONString(response, SerializerFeature.WriteNullListAsEmpty);
        System.out.println(response.toJSONString());

    }
}
//CommonResponse.java
public class CommonResponse {
    private String code;
    private String message;

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}


//output 
{"common":{"code":"1001","message":"he he he"},"object":[{"s":"d1","d":1490081384276,"i":1},{"s":"d2","d":1490081384276,"i":2},{"s":"d3","d":1490081384276,"i":3}],"type":null}
```
##fastjson Spring集成




```
"{
 ""type"": """",
 ""commonResponse"": {
  ""code"": """",
  ""message"": ""订单信息查询成功""
 },
 ""data"": [
  {
   ""orderID"": ""cee000ceeb3045acb8176d417069bc9a"",
   ""orderno"": ""FE16120500002"",
   ""businesstype"": ""SE"",
   ""lType"": """",
   ""comnamecn"": ""捷尔特国际货运代理（苏州）有限公司"",
   ""mBLNO"": null,
   ""loadingPort"": ""SHANGHAI"",
   ""portarae"": """",
   ""cargoreaddate"": 1481072400000,
   ""orderstatus"": ""0"",
   ""portopentime"": null,
   ""isdispatched"": false,
   ""containerVolume"": ""1X20GP""
  }, {
   ""orderID"": ""55691f7a9255456aabd50ab207c9389e"",
   ""orderno"": ""LD16120500001"",
   ""businesstype"": ""SE"",
   ""lType"": ""CT"",
   ""comnamecn"": ""无锡久骏国际货运代理有限公司"",
   ""mBLNO"": ""TLTCHTSN6491501"",
   ""loadingPort"": ""TAICANG"",
   ""portarae"": ""太仓港区"",
   ""cargoreaddate"": 1481072400000,
   ""orderstatus"": ""0"",
   ""portopentime"": null,
   ""isdispatched"": false,
   ""containerVolume"": ""1X20GP""
  }
 ]
}"          
```

- JSONArray：相当于List<Object>
public class JSONObject extends JSON implements Map<String, Object>, Cloneable, Serializable, InvocationHandler 

[]()
[Spring MVC整合fastjson](http://blog.csdn.net/xiongchen2012/article/details/51065465) worked
[SpringMVC杂记(1) 使用阿里巴巴的fastjson](http://www.cnblogs.com/AloneSword/p/4097941.html)
[FastJSON 简介及其Map/JSON/String 互转](https://my.oschina.net/leejun2005/blog/268634?utm_source=tuicool&utm_medium=referral)
[fastjson的JSONArray和JSONObject](https://my.oschina.net/sulliy/blog/499834)
[]()
[]()
[]()
[]()
[]()




































