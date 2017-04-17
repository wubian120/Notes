# Java 抽象类


// abstract类中可以没有abstract方法，但有abstract方法的类必须定义为abstract类
abstract class C3 {
    // abstract类中可以有成员变量，但成员变量不能用abstract修饰
    private int mPara;
    // abstract方法不能有方法体，abstract方法不能是private
    public abstract void setPara(int para);
    // 抽象类中可以有非抽象方法
    public int getPara(){
        return mPara;
    }
}








http://geek.csdn.net/news/detail/125966?utm_source=tuicool&utm_medium=referral






