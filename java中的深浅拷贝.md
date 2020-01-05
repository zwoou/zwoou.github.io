---
title: java中的深浅拷贝
date: 2017-08-23 10:08:30 
tags: 
    - java
---
**浅拷贝：** 被复制对象的所有变量都含有与原来对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，浅复制仅仅考虑所复制的对象，而不复制他引用的对象。
**深拷贝：** 深复制把要复制的对象所引用的对象都复制一遍。
<!-- more -->
- 如何进行浅拷贝
实现Cloneable接口，然后实现clone()方法，这个方法是空的，我们可以进行重写，然后返回对象即可。
- 如何进行深拷贝
我们需要将对象进行序列化，同时使用相关流的操作进行反序列化，最后得到一个对象。
``` java
public class Prototype implements Cloneable,Serializable {
    private String oName;
    ReferencePro referencePro;
    public Prototype(String oName,ReferencePro referencePro){
        this.oName=oName;
        this.referencePro=referencePro;
    }
    // 浅拷贝代码
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return (Prototype)super.clone();
    }

    // 深拷贝
    public Object deepClone() throws IOException, ClassNotFoundException {
        // 定义一个输出流，将当前对象输出到流中
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(byteArrayOutputStream);
        objectOutputStream.writeObject(this);
        // 定义一个输入流，将当前对象输入到流中，通过反序列化，从而获得对象。
        ByteArrayInputStream byteArrayInputStream = new ByteArrayInputStream(byteArrayOutputStream.toByteArray());
        ObjectInputStream objectInputStream = new ObjectInputStream(byteArrayInputStream);
        return objectInputStream.readObject();
    }
}
```

