# 2: 遇到多个构造器参数时要考虑用构建器

静态工厂和构造器都有一个局限性它们都不能很好的扩展到大量的可选参数。

## 1.场景

假设我们要用一个java类表示包装食品外面显示营养成分的那个标签。这里有几个必选的域：没分的含量、卡路里含量，除此之外还有20多个可选的域，比如：饱和脂肪量、胆固醇、脂肪含量等等。为了简单起见，我们只选择4个来示范。

```java
class NutritionFactsOne {
    private int servingSize; //require
    private int servings;    //require
    private int calorries;
    private int fat;
    private int sodium;
    private int carbohydrate;

    public NutritionFactsOne(int servingSize, int servings) {
        this.servingSize = servingSize;
        this.servings = servings;
    }

    public NutritionFactsOne(int servingSize, int servings, int calorries) {
        this(servingSize, servings);
        this.calorries = calorries;
    }

    public NutritionFactsOne(int servingSize, int servings, int calorries, int fat) {
        this(servingSize, servings, calorries);
        this.fat = fat;
    }

    public NutritionFactsOne(int servingSize, int servings, int calorries, int fat, int sodium) {
        this(servingSize, servings,calorries, fat);
        this.sodium = sodium;
    }

}

```

这个时候，你可以这样来创建一个对象：

```java
new NutritionFactsOne(240, 8, 100, 0, 35, 27);
```

你自己读着费劲，别人读起来更费劲，不是么。

所以，这么写你肯定会被打死的，机智的你想到了 javabean 的方式：

```java
class NutritionFactsTwo {
    private int servingSize; //require
    private int servings;    //require
    private int calorries;
    private int fat;
    private int sodium;
    private int carbohydrate;

    public void setServingSize(int servingSize) {
        this.servingSize = servingSize;
    }

    public void setServings(int servings) {
        this.servings = servings;
    }

    public void setCalorries(int calorries) {
        this.calorries = calorries;
    }

    public void setFat(int fat) {
        this.fat = fat;
    }

    public void setSodium(int sodium) {
        this.sodium = sodium;
    }

    public void setCarbohydrate(int carbohydrate) {
        this.carbohydrate = carbohydrate;
    }
}

```

创建对象的时候你可以这样：

```java
NutritionFactsTwo n = new NutritionFactsTwo();
n.setServingSize(240);
n.setServings(8);
n.setCalorries(100);
n.setFat(35);

```
但是这样的写法是有着很严重的缺点的。因为初始化的过程被分配到了几个不同的调用中，在构造过程中可能会存在不一致的状况。同时，这样的类无法设计为不可变类，当程序处于多线程状态时，需要付出额外的努力来保证线程安全性。

铺垫了这么多，我们来看看【构建器】是一种什么样的形式：

```java
class NutritionFactsThree {
    private final int servingSize; //require
    private final int servings;    //require
    private final int calorries;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        private final int servingSize; //require
        private final int servings;    //require

        private int calorries = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;


        public  Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }
        
        public Builder calorries(int calorries){
            this.calorries = calorries;
            return this;
        }
        public Builder fat(int fat){
            this.fat = fat;
            return this;
        }
        public Builder sodium(int sodium){
            this.sodium = sodium;
            return this;
        }
        public Builder carbohydrate(int carbohydrate){
            this.carbohydrate = carbohydrate;
            return this;
        }
        
        public NutritionFactsThree build(){
            return new NutritionFactsThree(this);
        }
        
    }
    
    private NutritionFactsThree(Builder builder){
        servingSize = builder.servingSize;
        servings = builder.servings;
        calorries = builder.calorries;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
    
}
```

现在，初始化一个对象，你可以这样：
```java
NutritionFactsThree n = new NutritionFactsThree.Builder(240, 8)
                                    .calorries(0)
                                    .sodium(35)    
                                    .build();
```

这样，客户端代码就很容易编写，并且容易阅读。

## 2.优点