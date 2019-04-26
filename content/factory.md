# 工厂模式

## 介绍

工厂方法模式，是创建型设计模式之一。
在任何需要生产复杂对象的地方，都可以使用工厂模式。复杂对象适合工厂模式，而能够通过new直接完成创建的对象无需使用工厂模式

## 定义

定义一个用于创建对象的接口，让子类决定实例化哪个类

## 代码

### 通用工厂
```
public abstract class Product {
    public abstract void method();
}

public class ConcreteProductA extends Product {
    @Override
    public void method() {
        System.out.println("我是产品A");
    }
}

public class ConcreteProductB extends Product {
    @Override
    public void method() {
        System.out.println("我是产品B");
    }
}
```

```
public abstract class Factory {
    public abstract Product createProduct();
}

public class ConcreteFactory extends Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

```

```
public class Client {
    public static void main(String[] args){
        Factory factory=new ConcreteFactory();
        Product product=factory.createProduct();
        product.method();
    }
}

```

这个工厂模式的特点是，每个工厂只能生产一种固定的产品。主要实现了把复杂生产过程封装起来，不对用户进行暴露。

### 单工厂
 
有时候产品虽然不同但是生产方式差不多，就可以利用反射来简化工厂的数量

我们可以对工厂进行下面的改造，运用泛型
```
public abstract class Factory {
    public abstract <T extends Product> T createProduct(Class<T> clz);
}
public class ConcreteFactory extends Factory {

    @Override
    public <T extends Product> T createProduct(Class<T> clz) {
        Product p=null;
        try {
            p= (Product) Class.forName(clz.getName()).newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return (T) p;
    }
}
```

```
public class Client {
    public static void main(String[] args){
        Factory factory=new ConcreteFactory();
        Product product=factory.createProduct(ConcreteProductA.class);
        product.method();

        Product productB=factory.createProduct(ConcreteProductB.class);
        productB.method();
    }
}
```

### 简单工厂模式

如果工厂只有确定有且仅有一个，那么我们可以做以下简化

```
public class Factory {
    public static Product createProduct(){
        return new ConcreteProductA();
    }
}
```

## 总结

工厂类简单来说，就是名如其名，向外面不断的发出产品。你可以专一，让每个工厂生产不同的产品。也可以精简，让同一个工厂生产不同的产品，然后通过参数或者方法名加以区分。
