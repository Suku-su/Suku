`以下是修改文章的一些准则，你将基于他修改文章`

`1.完善我提供的文章,我需要在dscn上发布`

`3.扩展文章，使文章结构连贯，逻辑自然，添加必要描述，以及标题`

`4.如果遇见不明白的问题需要向我提问，有错误的地方需要指出并修正`

`5.添加代码时可以添加特征我的用户名为YA33`

`6.在文章原本的基础上进行扩展，包括深度与广度`

`7.如果是纯文字内容可以添加图解，举例子等方便理解，并添加必要的思维导图`

`8.采用正常人的口吻`

`9.按点区分，标题不要使用特殊符号`

`10.需要markdown格式的文件，对各级标题进行划分`

`11.添加注解代码使用java语言`



---

---

---



# 创建者模式：构建复杂对象的优雅之道

## 概念与定义

创建者模式（Builder Pattern）是一种设计模式，属于创建型模式的一种。它的核心思想是**将一个复杂对象的构造与其表示分离**，使得同样的构建过程可以创建不同的表示。这种模式通过将复杂对象分解为多个相对简单的部分，然后一步一步构建而成，从而提高了代码的灵活性和可读性。

简单来说，创建者模式就像是建筑工程师与施工队的关系：工程师负责设计蓝图和施工步骤（构建过程），而施工队根据蓝图一步步施工（创建对象），最终完成建筑（复杂对象）。这种分离使得我们能够更容易地管理复杂对象的创建过程，特别是在对象有很多组成部分或配置选项时。

## 适用场景分析

创建者模式特别适用于以下场景：

1. **对象具有多个组成部分**：当对象由多个部分组成，且每个部分可能有不同的实现或配置时。
2. **需要创建不同的对象表示**：同样的构建过程需要产生不同的结果对象。
3. **避免构造函数参数过多**：当构造函数参数过多或可选参数复杂时，使用创建者模式可以提高代码的可读性和易用性。
4. **对象创建过程需要精细化控制**：当对象的创建步骤需要精确控制，或者某些步骤需要根据不同情况省略或添加时。

在本文中，我们将通过一个汽车制造的示例来详细讲解创建者模式的应用。

## 示例场景：汽车制造

假设我们正在设计一个汽车制造系统，汽车由多个零件组成，包括车窗、座椅和轮胎。每种零件又分为A、B两种类型，分别对应不同的配置和价格。我们需要能够灵活地创建不同类型的汽车（如A型车、B型车），并且能够方便地扩展新的类型。

### 原始实现方式及其问题

首先，我们来看一种简单的实现方式。我们定义一个`Matter`接口表示汽车零件，然后为每种零件创建实现类（如`Aseats`、`Bseats`等）。最后，在一个创建类（如`Ycreate`）中通过条件判断来组装不同类型的汽车。

#### 零件接口定义

```java
package com.YA33.desgin.creator;

import java.math.BigDecimal;

public interface Matter {
    String scene();      // 使用场景
    String brand();      // 品牌
    String model();      // 型号
    BigDecimal price();  // 价格
    String desc();       // 描述
}
```

#### 具体零件实现

以座椅为例，A类和B类座椅的实现如下：

```java
package com.YA33.desgin.creator.seats;

import com.YA33.desgin.creator.Matter;
import java.math.BigDecimal;

public class Aseats implements Matter {
    @Override
    public String scene() { return "A类座椅"; }
    @Override
    public String brand() { return "原车自带"; }
    @Override
    public String model() { return "A级"; }
    @Override
    public BigDecimal price() { return new BigDecimal(0); }
    @Override
    public String desc() { return "A类座椅原车自带，适配好，不需要额外消费"; }
}

public class Bseats implements Matter {
    @Override
    public String scene() { return "B类座椅"; }
    @Override
    public String brand() { return "需要您额外购买"; }
    @Override
    public String model() { return "S级"; }
    @Override
    public BigDecimal price() { return new BigDecimal(10000); }
    @Override
    public String desc() { return "B类座椅原采用荷兰小牛皮，舒适度高"; }
}
```

#### 原始创建类

```java
package com.YA33.desgin.creator;

import com.YA33.desgin.creator.seats.Aseats;
import com.YA33.desgin.creator.seats.Bseats;
// 其他零件导入略

import java.math.BigDecimal;
import java.util.ArrayList;

public class Ycreate {
    public String getCar(String type) {
        ArrayList<Matter> list = new ArrayList<>();
        BigDecimal price = new BigDecimal(10000); // 基础价格

        if ("A".equals(type)) {
            Aseats aseats = new Aseats();
            Atries atries = new Atries();
            Awindows awindows = new Awindows();
            list.add(aseats);
            list.add(atries);
            list.add(awindows);
            BigDecimal extraPrice = aseats.price().add(atries.price()).add(awindows.price());
            return list + "总价格" + price.add(extraPrice);
        } else if ("B".equals(type)) {
            // 类似A类型的处理，略
        } else {
            return null;
        }
    }
}
```

#### 原始方式存在的问题

1. **代码重复**：A型和B型车的创建代码几乎完全相同，只有具体零件类型不同。
2. **难以扩展**：如果需要新增C型、D型车，必须修改`getCar`方法，添加更多的条件分支，违反开闭原则。
3. **可读性差**：当零件类型增多时，条件判断会变得冗长且难以维护。
4. **构建过程固化**：构建过程与具体零件类型紧密耦合，无法灵活调整构建步骤。

## 创建者模式的解决方案

为了解决上述问题，我们引入创建者模式。创建者模式通过将对象的构建过程抽象出来，使得我们可以通过不同的构建器来创建不同的对象表示。

### 模式结构

创建者模式通常包含以下几个角色：

1. **产品（Product）**：要创建的复杂对象，在本文中是汽车。
2. **抽象建造者（Builder）**：定义创建产品各个部分的抽象接口，在本文中是`IMenu`。
3. **具体建造者（ConcreteBuilder）**：实现抽象建造者接口，构建和装配各个部件，在本文中是`CarPackgeMenu`。
4. **指挥者（Director）**：调用具体建造者来构建产品，它负责管理构建过程，在本文中是`Builder`类。

下面是创建者模式的结构示意图：

```
+----------------+       +-----------------+       +-------------------+
|   Director     |       |   Builder       |       |    Product        |
| (Builder类)    |------>| (IMenu接口)     |+----->| (CarPackgeMenu)   |
+----------------+       +-----------------+       +-------------------+
                                 ^
                                 |
                                 |
                     +-----------+-----------+
                     |                       |
               +----------+             +----------+
               | Concrete |             | Concrete |
               | BuilderA |             | BuilderB |
               +----------+             +----------+
```

### 实现代码

#### 材料目录接口

```java
package com.YA33.desgin.creator;

/**
 * 材料目录接口，定义构建汽车所需的方法
 */
public interface IMenu {
    IMenu appendSeat(Matter matter);    // 添加座椅
    IMenu appendTries(Matter matter);   // 添加轮胎
    IMenu appendWindows(Matter matter); // 添加车窗
}
```

#### 汽车包实现

```java
package com.YA33.desgin.creator;

import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;

/**
 * 汽车包具体实现
 */
public class CarPackgeMenu implements IMenu {
    private List<Matter> list = new ArrayList<>();
    private BigDecimal price = new BigDecimal(10000);
    private String type;

    public CarPackgeMenu(String type) {
        this.type = type;
    }

    @Override
    public IMenu appendSeat(Matter matter) {
        list.add(matter);
        price = price.add(matter.price());
        return this;
    }

    @Override
    public IMenu appendTries(Matter matter) {
        list.add(matter);
        price = price.add(matter.price());
        return this;
    }

    @Override
    public IMenu appendWindows(Matter matter) {
        list.add(matter);
        price = price.add(matter.price());
        return this;
    }

    @Override
    public String toString() {
        return "CarPackgeMenu{list=" + list + ", price=" + price + ", type='" + type + "'}";
    }
}
```

#### 建造者类

```java
package com.YA33.desgin.creator;

import com.YA33.desgin.creator.seats.Aseats;
import com.YA33.desgin.creator.seats.Bseats;
import com.YA33.desgin.creator.tires.Atries;
import com.YA33.desgin.creator.tires.Btries;
import com.YA33.desgin.creator.windos.Awindows;
import com.YA33.desgin.creator.windos.Bwindows;

/**
 * 建造者类，负责构建不同类型的汽车
 */
public class Builder {
    public IMenu typeA() {
        return new CarPackgeMenu("A")
                .appendTries(new Atries())
                .appendSeat(new Aseats())
                .appendWindows(new Awindows());
    }

    public IMenu typeB() {
        return new CarPackgeMenu("B")
                .appendTries(new Btries())
                .appendSeat(new Bseats())
                .appendWindows(new Bwindows());
    }
}
```

#### 使用示例

```java
package com.YA33.desgin.creator;

public class Example {
    public static void main(String[] args) {
        Builder builder = new Builder();
        
        // 构建A型车
        IMenu typeA = builder.typeA();
        System.out.println("A型车配置: " + typeA);
        
        // 构建B型车
        IMenu typeB = builder.typeB();
        System.out.println("B型车配置: " + typeB);
    }
}
```

## 创建者模式的优势

通过使用创建者模式，我们获得了以下好处：

1. **封装性好**：构建过程被封装在具体建造者中，客户端不需要知道内部细节。
2. **扩展性强**：要新增一种汽车类型，只需要增加一个新的方法（如`typeC()`），而不需要修改现有代码。
3. **更好的可读性**：通过方法链式调用，代码更加清晰易懂。
4. **精细控制构建过程**：可以控制构建过程的每一步，甚至可以改变构建顺序。
5. **减少重复代码**：公共的构建过程被抽象出来，避免了代码重复。

## 实际应用场景

创建者模式在实际开发中有广泛的应用，例如：

1. **复杂对象的创建**：如创建复杂的文档、报表等。
2. **配置对象的构建**：如构建具有多个可选参数的配置对象。
3. **测试数据构建**：在测试中构建复杂的测试对象。
4. **API参数构建**：当API参数过多时，使用创建者模式可以提高API的易用性。

## 总结

创建者模式通过将复杂对象的构建过程与其表示分离，提供了一种灵活、可扩展的对象创建方式。它特别适用于那些具有多个组成部分或配置选项的复杂对象。通过本文的汽车制造示例，我们可以看到创建者模式如何帮助我们管理复杂对象的创建过程，提高代码的可维护性和扩展性。

在实际开发中，当遇到复杂对象的创建问题时，不妨考虑使用创建者模式，它可能会为你带来意想不到的好处。

---
*本文由YA33创作，欢迎分享，转载请注明出处。*

---

---

以下内容本次不做展示

# 原型模式

## 概念：

是用于创建重复的对象，同时又能保证性能。这种类型的设计模式属于创建型设计模式，他提供了一种创建对象的最佳方式。

适用：

对象的创建复杂，需要重数据库获取，或者rpc远程调用等使用复制到方式可以节省时间。





