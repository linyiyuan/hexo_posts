---
layout: PHP设计模式——建造者模式
title: PHP设计模式——建造者模式
date: 2019-08-15 14:42:45
categories: "PHP"
abbrlink: 005a
tags: 
- PHP
- 设计模式
---

<img src="http://images.linyiyuan.top/3897939-3d84c8a531e21505.png" style="width:900px;height:400px" />

> 建造者模式(Builder Pattern)：将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

建造者模式是一步一步创建一个复杂的对象，它允许用户只通过指定复杂对象的类型和内容就可以构建它们，用户不需要知道内部的具体构建细节。建造者模式属于对象创建型模式。根据中文翻译的不同，建造者模式又可以称为生成器模式。

<!--less-->

## 前言
 建造者模式也称生成器模式，核心思想是将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示，这样的设计模式被称为建造者模式。


![27194147](http://images.linyiyuan.top/2025309-b87a0396c5193070.png)


建造者模式一般认为有四个角色：

1. 产品角色，产品角色定义自身的组成属性

2. 抽象建造者，抽象建造者定义了产品的创建过程以及如何返回一个产品

3. 具体建造者，具体建造者实现了抽象建造者创建产品过程的方法，给产品的具体属性进行赋值定义

4. 指挥者，指挥者负责与调用客户端交互，决定创建什么样的产品

为什么使用：
1. 对象的生产需要复杂的初始化，比如给一大堆类成员属性赋初值，设置一下其他的系统环境变量。使用建造者模式可以将这些初始化工作封装起来。
2. 对象的生成时可根据初始化的顺序或数据不同，而生成不同角色。
3. 例如：汽车，他的发动机引擎有好多品牌，轮胎也有各种材质，内饰更是千奇百怪；鸟，他的头、翅膀以及脚有各种颜色和形状，在创建这种复杂对象的时候，我们建议使用建造者模式。

## 应用

在很多游戏软件中，地图包括天空、地面、背景等组成部分，人物角色包括人体、服装、装备等组成部分，可以使用建造者模式对其进行设计，通过不同的具体建造者创建不同类型的地图或人物

## 实例
以下将用一个大家比较熟悉的游戏《英雄联盟》 作为一个简单的例子，现在我们想创建一个角色，我们通过使用建造者模式去设计如果创建不同的属性的角色

```php
/**
 * Class Role
 * @Author YiYuan-LIn
 * @Date: 2019/8/15
 * 定义一个角色类
 */
class Role
{
    /**
     * 姓名
     * @var string
     */
    public $name;

    /**
     * 攻击力
     * @var integer
     */
    public $power;

    /**
     * 攻速
     * @var float
     */
    public $attack_speed;

    /**
     * 暴击
     * @var integer
     */
    public $crit;

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @description 显示信息
     */
    public function display()
    {
        echo '姓名：' . $this->name . '<br>';
        echo '攻击力：' . $this->power . '<br>';
        echo '攻击速度：' . $this->attack_speed . '<br>';
        echo '暴击几率：' . $this->crit . '<br>';
    }
}

/**
 * Class RoleBuilder
 * @Author YiYuan-LIn
 * @Date: 2019/8/15
 * 定义一个射手抽象类
 */
abstract class RoleBuilder
{
    public $_role;

    /**
     * ShooterBuilder constructor.
     * 初始化一个射手
     */
    public function __construct()
    {
        $this->_role = new Role();
    }

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @enumeration:
     * @return mixed
     * @description 设置名字
     */
    abstract public function setName();

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @enumeration:
     * @return mixed
     * @description 设置力量值
     */
    abstract public function setPower();

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @enumeration:
     * @return mixed
     * @description 设置攻击速度
     */
    abstract public function setAttackSpeed();

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @enumeration:
     * @return mixed
     * @description 设置暴击几率
     */
    abstract public function setCrit();

    /**
     * @Author YiYuan-LIn
     * @Date: 2019/8/15
     * @enumeration:
     * @return mixed
     * @description 建立角色
     */
    abstract public function builder();

}


/**
 * Class Ez
 * @Author YiYuan-LIn
 * @Date: 2019/8/15
 * 定义个伊泽瑞尔射手
 */
class Ez extends RoleBuilder
{
    public function setName()
    {
        // TODO: Implement setName() method.
        $this->_role->name = '伊泽瑞尔';
    }

    public function setPower()
    {
        // TODO: Implement setPower() method.
        $this->_role->power = 100;
    }

    public function setAttackSpeed()
    {
        // TODO: Implement setAttackSpeed() method.
        $this->_role->attack_speed = 0.75;
    }

    public function setCrit()
    {
        // TODO: Implement setCrit() method.
        $this->_role->crit = 0;
    }

    public function builder()
    {
        // TODO: Implement builder() method.
        return $this->_role;
    }
}

/**
 * Class AoBaMa
 * @Author YiYuan-LIn
 * @Date: 2019/8/15
 * 定义一个奥巴马的角色
 */
class AoBaMa extends RoleBuilder
{
    public function setName()
    {
        // TODO: Implement setName() method.
        $this->_role->name = '奥巴马';
    }

    public function setPower()
    {
        // TODO: Implement setPower() method.
        $this->_role->power = 200;
    }

    public function setAttackSpeed()
    {
        // TODO: Implement setAttackSpeed() method.
        $this->_role->attack_speed = 2.50;
    }

    public function setCrit()
    {
        // TODO: Implement setCrit() method.
        $this->_role->crit = 100;
    }

    public function builder()
    {
        // TODO: Implement builder() method.
        return $this->_role;
    }
}

class RoleDirector
{
    private $_role;
    public function __construct(RoleBuilder $role)
    {
        $this->_role = $role;
    }

    public function build()
    {
        $this->_role->setName();
        $this->_role->setPower();
        $this->_role->setAttackSpeed();
        $this->_role->setCrit();

        return $this->_role->builder();
    }
}


$anBaMaRole = new RoleDirector(new AoBaMa());
$anBaMaRole = $anBaMaRole->build();
$anBaMaRole->display();

//var_dump($anBaMaRole);
echo '<br>';

$ezRole = new RoleDirector(new Ez());
$ezRole = $ezRole->build();
$ezRole->display();

//var_dump($ezRole);

```

## 总结
使用建造者模式时，我们把创建一个role实例的过程分为了两步.

一步是先交给对应角色的建造者，如Ez建造者。这样的好处就把角色的属性设置封装了起来，我们不用在new一个Role时，因为要得到一个Ez角色的实例，而在外面写了一堆$ez->power=70。

另一步是交给了一个建造指挥者，调了一个built方法，通过先设置power，再设置Crit的顺序，初始化这个角色。当然在这个例子中，初始化的顺序，是无所谓的。但是如果对于一个建造汉堡，或是地图，初始化的顺序不同，可能就会得到不同的结果。

也许，你会说，我直接设置也很方便呀。是的，对于某些情况是这样的。但是如果你考虑，我现在想增加一个寒冰角色呢？如果我现在想让建造有初始化有三种不同的顺序呢？

如果你使用了建造者模式，这两个问题就简单了，增加一个寒冰角色，那就增加一个寒冰建造者类。初始化三种不同的顺序，那么就在指挥建造者中增加两种建造方法。
