# 责任链模式

## 介绍

责任链模式，内部的实现，有点像一个链表。每个节点收到请求后，都会尝试对请求进行处理，如果无法处理，则会将请求转发给下一个节点。

- 多个对象可以处理同一请求，但具体由哪个对象处理则在运行时动态决定
- 在请求处理者不明确的情况下向多个对象中的一个提交一个请求
- 需要动态指定一组对象处理请求

## 定义

使多个对象都有机会处理请求，从而避免了请求的发出者和接收者直接的耦合关系。将这些对象连成一条链，并沿着这条链传递该请求，直到有对象处理它为止。

## 代码

```
public abstract class AbstactRequest {
    private Object obj;

    public AbstactRequest(Object obj){
        this.obj=obj;
    }

    public Object getObj() {
        return obj;
    }

    public abstract int getRequestLevel();
}

```

```
public class Request1 extends AbstactRequest {
    public Request1(Object obj) {
        super(obj);
    }

    @Override
    public int getRequestLevel() {
        return 1;
    }
}

public class Request2 extends AbstactRequest {
    public Request2(Object obj) {
        super(obj);
    }

    @Override
    public int getRequestLevel() {
        return 2;
    }
}
public class Request3 extends AbstactRequest {
    public Request3(Object obj) {
        super(obj);
    }

    @Override
    public int getRequestLevel() {
        return 3;
    }
}


```

```
public abstract class AbstractHandler {
    protected AbstractHandler nextHandler;
    public final void handleRequest(AbstactRequest request){
        if (getHandleLevel()==request.getRequestLevel()){
            handle(request);
        }else {
            if (nextHandler!=null){
                nextHandler.handleRequest(request);
            }else {
                System.out.println("所有的handler都不能处理改请求 ");
            }

        }
    }

    protected abstract int getHandleLevel();

    protected abstract void handle(AbstactRequest request);
}
```

```
public class Handler1 extends AbstractHandler {
    @Override
    protected int getHandleLevel() {
        return 1;
    }

    @Override
    protected void handle(AbstactRequest request) {
        System.out.println("我Handler1处理了"+request.getRequestLevel());
    }
}

public class Handler2 extends AbstractHandler {
    @Override
    protected int getHandleLevel() {
        return 2;
    }

    @Override
    protected void handle(AbstactRequest request) {
        System.out.println("我Handler2处理了"+request.getRequestLevel());
    }
}


public class Handler3 extends AbstractHandler {
    @Override
    protected int getHandleLevel() {
        return 3;
    }

    @Override
    protected void handle(AbstactRequest request) {
        System.out.println("我Handler3处理了"+request.getRequestLevel());
    }
}


```


```
public class Client {
    public static void main(String[] args){
        AbstractHandler handler1=new Handler1();
        AbstractHandler handler2=new Handler2();
        AbstractHandler handler3=new Handler3();

        handler1.nextHandler=handler2;
        handler2.nextHandler=handler3;

        AbstactRequest request1=new Request1("Request1");
        AbstactRequest request2=new Request2("Request2");
        AbstactRequest request3=new Request3("Request3");

        //链式请求总是从首端发起请求
        handler1.handleRequest(request1);
        handler1.handleRequest(request2);
        handler1.handleRequest(request3);
    }
}

```

## 总结

#### 优点 
请求者和处理者关系进行了解耦，提高了代码的灵活性

#### 缺点
处理者太多的话，会影响处理性能 