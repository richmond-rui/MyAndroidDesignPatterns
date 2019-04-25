# Builder模式
## 介绍
- 隐藏构建过程的细节
- 构建过程和部件可扩展
- 降低二者的耦合
## 定义
将一个复杂的对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示
## 代码
### 标准Builder模式

```
public abstract class Builder {
    public abstract void buildBoard(String board);

    public abstract void buildDisplay(String display);

    public abstract void buildOs();

    public abstract Computer create();
}
```

```
public class MacbookBuilder extends Builder {
    private Computer mComputer=new MacBook();
    @Override
    public void buildBoard(String board) {
        mComputer.setmBoard(board);
    }

    @Override
    public void buildDisplay(String display) {
        mComputer.setmDisplay(display);
    }

    @Override
    public void buildOs() {
        mComputer.setmOs();
    }

    @Override
    public Computer create() {
        return mComputer;
    }
}

```


```
public abstract class Computer {
    protected String mBoard;
    protected String mDisplay;
    protected String mOs;

    public Computer() {

    }

    public void setmBoard(String mBoard) {
        this.mBoard = mBoard;
    }

    public void setmDisplay(String mDisplay) {
        this.mDisplay = mDisplay;
    }

    public abstract void setmOs();

    @Override
    public String toString() {
        return "Computer{" +
                "mBoard='" + mBoard + '\'' +
                ", mDisplay='" + mDisplay + '\'' +
                ", mOs='" + mOs + '\'' +
                '}';
    }
}

```

```
public class MacBook extends Computer {
    public MacBook() {
    }

    @Override
    public void setmOs() {
        mOs="MAC OS";
    }
}

```

```
public class Director {
    Builder mBuilder=null;

    public Director(Builder mBuilder) {
        this.mBuilder = mBuilder;
    }

    public void construct(String board,String display){
        mBuilder.buildBoard(board);
        mBuilder.buildDisplay(display);
        mBuilder.buildOs();
    }
}
```

```
public class BuilderTest {
    public static void buildTest(){
        Builder builder=new MacbookBuilder();
        Director director=new Director(builder);
        director.construct("龙芯主板","27寸显示器");
        System.out.println(builder.create().toString());
    }
}
```
完整的builder 应该由三个部分组成

1. 两个抽象类

	- 产品,即本例中computer
	- builder

2. 抽象类的具体实现
	
	- 产品的实现，即本例中的macbook
	- builder具体实现，即本例中的macbookBuilder

3. Director

	- Director负责将用户传入的bulder和产品类，组装成具体产品

# 总结

以上只是标准的Builder模式。但是在一般的使用当中，往往会对相关的结构做出灵活的修改。例如android中最常见的AlterDialog的创建。
	
AlterDialog的builer只是作为AlterDialog的内部类来出现，并且这个builder只需要用来构建AlterDialog这一种产品不需要考虑其他的类型。所以在android源码中就省略了builder的抽象类，而且Director的也消失不见，隐匿在了用户创建的时候和builder中。
	
所以在使用该方法的时候需要仔细考虑使用场景，对原有的经典模式进行适当的修改，否则会出现为了设计模式而设计的局面

并且使用builder模式，可以减少产品暴露的接口数量，产品不需要写一堆set。用户只需要给builder设置对应的参数，然后一次性create即可
	



