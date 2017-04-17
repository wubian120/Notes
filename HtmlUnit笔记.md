#HtmlUnit






```
    public static void htmlUnitFirstExample(String shipName, String voyage){

        final WebClient wc = new WebClient();
        String shipUrl = null;
        String shipUrl =  null//FOBURL + shipName + "&voyage=" + voyage;
        try{

            shipName = URLEncoder.encode(shipName, "utf-8");
            shipUrl = FOBURL + shipName + "&voyage=" + voyage;
            final HtmlPage page = wc.getPage(shipUrl);

            final String pageAsText = page.asText();
            System.out.println(pageAsText);

        }catch (Exception e){
            e.printStackTrace();
        }
    }
```


###HtmlUnit官网
http://htmlunit.sourceforge.net/
###简单例子
http://htmlunit.sourceforge.net/gettingStarted.html




















