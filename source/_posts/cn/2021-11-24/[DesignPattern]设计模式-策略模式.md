---
title: '[DesignPattern]设计模式-策略模式'
catalog: true
date: 2021-11-24
subtitle: DesignPattern | Strategy
lang: cn
header-img: 1.png
tags:
- Java
- Design Pattern
categories:
- Design Pattern
---

--- 
### 介绍
简单的说，策略模式帮你优化了大量的if/else，传统的if/else扩展性很差，随着代码量的增多也不易于维护。
最近工作中用到了这个模式，因为业务的复杂性，用这个模式省去了大量的判断和实现，业务有增加的需求也只是增加实现的策略类。

### 概念
策略模式是一种行为型模式，针对一组算法，将每一个算法封装到具有共同接口的独立的类中，使得它们可以互换。

### 模型
Context：上下文角色，持有一个Strategy的引用。
Strategy：抽象策略角色，这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口
ConcreteStrategy：具体策略角色，包装了相关的算法或行为，实现或继承了抽象策略角色。

### 代码实现
> 我们拿巫师3的女术士打桩机杰洛特举例，杰洛特有两把剑，钢剑和银剑，钢剑砍人，银剑砍怪，十字镐在水里射水妖。
> 也就是我们面对不同的对手要有不同的攻击策略。

#### 首先定义抽象的策略接口
```java
public interface SlayerStrategy {
    
  void attack();
}
```

#### 具体的策略实现
```java
public class HumansStrategy implements SlayerStrategy{

    @Override
    public void attack() {
        System.out.println(" 使用钢剑攻击对手!!! ");
    }
}

public class MonsterStrategy implements SlayerStrategy {

  @Override
  public void attack() {
    System.out.println(" 使用银剑攻击怪物!!! ");
  }
}

public class BansheeStrategy implements SlayerStrategy{

  @Override
  public void attack() {
    System.out.println(" 使用十字镐射击女妖!!! ");
  }
}
```

#### 上下文类，你可以根据目标选择战斗策略
```java
public class SlayerContext {

    private SlayerStrategy strategy;

    public void changeStrategy(SlayerStrategy strategy) {
        this.strategy = strategy;
    }

    public SlayerContext(SlayerStrategy strategy) {
        this.strategy = strategy;
    }

    public void goToBattle() {
        strategy.attack();
    }
}
```

### 测试类
```java
public class Application {

  public static void main(String[] args) {
    // 遇到人类，使用钢剑
    System.out.println(" 遇到人类，切换钢剑");
    var slayer = new SlayerContext(new HumansStrategy());
    slayer.goToBattle();

    // 遇到怪物，使用银剑
    System.out.println(" 遇到怪物，切换银剑");
    slayer.changeStrategy(new MonsterStrategy());
    slayer.goToBattle();

    // 遇到女妖，使用十字镐
    System.out.println(" 遇到女妖，切换十字镐");
    slayer.changeStrategy(new BansheeStrategy());
    slayer.goToBattle();
  }
}

```
运行结果
```
 遇到人类，切换钢剑
 使用钢剑攻击对手!!! 
 遇到怪物，切换银剑
 使用银剑攻击怪物!!! 
 遇到女妖，切换十字镐
 使用十字镐射击女妖!!! 
```