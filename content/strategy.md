# 策略模式

## 介绍



- 针对同一类型问题的多种处理方式，仅仅是具体行为有差别
- 需要安全的封装同一类型的操作
- 出现同一抽象类，有多个子类，并且又需要使用if-else或者switch来选择具体子类时

## 定义

策略模式属于对象的行为模式。其用意是针对一组算法，将每一个算法封装到具有共同接口的独立的类中，从而使得它们可以相互替换。策略模式使得算法可以在不影响到客户端的情况下发生变化

## 代码

```
public interface CalculateStrategy {
    int calculatePrice(int km);
}

//公交车计算策略
public class BusStrategy implements CalculateStrategy {
    @Override
    public int calculatePrice(int km) {
        int extraTotal=km-10;
        int extraFactor=extraTotal/5;
        int fraction=extraTotal%5;
        int price=1+extraFactor*1;
        return fraction>0?++price:price;
    }
}

//地铁价格计算策略
public class SubwayStrategy implements CalculateStrategy {
    @Override
    public int calculatePrice(int km) {
        if (km<=6){
            return 3;
        }else if (km>6&&km<12){
            return 4;
        }else if (km>12&&km<22){
            return 5;
        }else if (km>22&&km<32){
            return 6;
        }
        return 7;
    }
}
```
```
public class TranficCalculator {
    CalculateStrategy mStrategy;

    public void setmStrategy(CalculateStrategy mStrategy) {
        this.mStrategy = mStrategy;
    }

    public int calculatePrice(int km) {
        return mStrategy.calculatePrice(km);
    }

    public static void main(String[] args){
        TranficCalculator calculator=new TranficCalculator();
        calculator.setmStrategy(new BusStrategy());
        System.out.println("公交车乘16km的价格"+calculator.calculatePrice(16));
    }
}
```

## 总结
策略模式较为简单，使用场景也只需要如定义那般即可。一般来说包含一个抽象的接口，和若干个实现类，以及一个调用方，即可