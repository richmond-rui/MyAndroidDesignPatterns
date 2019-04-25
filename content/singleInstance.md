# 单例模式

## 懒汉模式
### 代码 
```
public class Singleton{
	private static Singleton instance;
	private Singleton(){}
	public static synchronized Singleton getInstance(){
		if(instance==null){
			instance=new Singleton();
		}
		return instance;
	}
}
```
### 优点
只有在使用单例的时候才被初始化，相比直接在静态变量初始化节约了资源

### 缺点
每次使用单例模式的时候都需要进行同步，造成资源浪费。不推荐使用

## Double CheckLock(DCL)模式
### 代码
```
public class Singleton{
	private static Singleton instance;
	private Singleton(){}
	public static  Singleton getInstance(){
		if(instance==null){
			synchronized(Singleton.class){
				if(instance==null){
					instance=new Singleton();
				}
			}
		}
		return instance;
	}
}
```
### 优点
相对于懒汉模式，一般只有在第一次新建实例的时候才会进行同步锁，提高了效率
### 缺点
由于java内存模型的原因，可以会失败。但是发生的概率极小。推荐使用

## 总结
此模式没有什么难点，只需要在使用到单例模式的地方，使用DCL模式替换你原来的实现方式即可。
DCL模式当做公式记下来即可。

