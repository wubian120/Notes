#Java 序列化


**序列化：把对象转为字节序列的过程**
**反序列化：字节序列恢复为Java对象**

##序列化方式：

扩展Serializable 接口
java.io.Serializable 空接口


Serializable：标记接口，实现该接口无须实现任何方法，只是表明该类的实例是可序列化的
Externalizable

所有在网络上传输的对象都应该是可序列化的，否则将会出现异常；所有需要保存到磁盘里的对象的类都必须可序列化；程序创建的每个JavaBean类都实现Serializable；


实现Serializable实现序列化的类，程序可以通过如下两个步骤来序列化该对象：
java.io.ObjectOutputStream  对象输出流
java.io.ObjectInputStream   对象输入流

1.创建一个ObjectOutputStream，这个输出流是一个处理流，所以必须建立在其他节点流的基础之上

// 创建个ObjectOutputStream输出流
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("object.txt"));
2.调用ObjectOutputStream对象的writeObject方法输出可序列化对象

// 将一个Person对象输出到输出流中
oos.writeObject(per);

```
public class User implements Serializable {

    private int age;
    private String name;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() { return name;}

    public void setName(String name) {
        this.name = name;
    }

    public static void main(String[] args) throws Exception, IOException{

          //序列化
//        User u = new User();
//        u.setAge(25);
//        u.setName("vven");
//        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("E:/person.txt")));
//        oos.writeObject(u);

        //反序列化
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(new File("E:/person.txt")));
        User u1 = (User) ois.readObject();

        System.out.println(u1.getAge());
        System.out.println(u1.getName());
    }

}

//output:
//25
//vven
//

```
http://www.cnblogs.com/xdp-gacl/p/3777987.html



### 序列对象引用字段
**如果序列化java对象的某个实体 是另一个类的引用。 则该类必须是可序列化的，否则 抛出异常。

Exception in thread "main" java.io.NotSerializableException: basics.A



[]()
[]()
[]()
[]()
