# 流的基本概念 #

## 什么是流？

> 流是Java8引入的全新概念，他用来处理集合中的数据，暂且可以把它理解为一种高级集合。  
  
> 众所周知，集合的操作非常麻烦，若要对集合进行筛选、投影，需要写大量的代码，而流是以声明的形式操作集合，它就像SQL语句，我们只需要告诉流需要对集合进行什么操作，它就会自动进行操作，并将执行结果交给你，无须我们自己手写代码。  
  
> 因此，流的集合操作对我们来说是透明的，我们只需要向流下达命令，它就会自动把我们想要的结果给我们。由于操作过程完全由java处理，因此它可以根据当前的硬件环境选择最优的方法处理，我们也无须编写复杂又容易出错的多线程代码了。  

## 流的特点
>1. 只能遍历一次  
   我们可以把流想象成一条流水线，流水线的源头是我们的数据源（一个集合），数据源中的元素依次被输送到流水线上，我们可以在流水线上对元素进行各种操作。一旦元素走到了流水线的另一头，那么这些元素就被“消费掉了”，我们无法再对这个流进行操作。当然，我们可以从数据源那里再获得一个新的流重新遍历一遍。  
> 
>2. 采用内部迭代方式  
   若要对集合进行处理，则需我们手写处理代码，这就叫做外部迭代

## 流的操作种类

    流的操作分为两种,分别为中间操作和终端操作。

 > 1. 中间操作：  
 >
 >     当数据源中的数据上了流水线后，这个过程对数据的所有操作都称为“中间操作”。  
 >     中间操作仍然会返回一个流对象，因此多个中间操作可以串联起来行程一个流水线。
 > 2. 终端操作： 
 >
 >     当所有的中间操作完成后，若要将数据从流水线上拿下来，则需要执行终端操作。  
 >     终端操作将返回一个执行结果，这就是你想要的数据。

 ## 流的操作过程

> 使用流一共需要三步：  
> 1. 准备一个数据源
> 2. 执行中间操作  
     中间操作可以有多个它们可以串联起来形成流水线。
> 3. 执行终端操作  
     执行终端操作后本次流结束，你将获得一个执行结果。 

# 流的使用 # 

## 获取流

    在使用流之前，首先需要拥有一个数据源，并通过StreamAPI提供的一些方法获取该数据源的流对象。数据源可以有很多种形式。

    1. 集合：这种数据源较为常用，通过Stream方法即可获取流对象：
```
      List<Person> list = new ArrayList<Person>();
      Stream<Person> stream = list.stream();
```
    2. 数组：通过Arrays类提供的静态函数stream()获取数组的流对象：
```
      String[] names = {"chaimm", "peter", "john"};
      Stream<String> stream = Arrays.stream(names);
```
    3. 直接将几个值变成流对象:
```
      Stream<String> stream = Stream.of("chiamm", "peter", "john");           
```
    4. 文件
```
      try(Stream lines = Files.lines(Path.get("文件路径名"), Charset.defaultCharset())) {
        // 可以对lines做一些操作
      } catch (IOException e)       
```
      PS：Java7简化了IO操作，把打开IO操作放在try后的括号中即可省略关闭IO的代码。

## 筛选filter 
    filter 函数接收一个Lambda表达式作为参数，该表达式返回boolean，在执行过程中，流将元素逐一输送给filter，并筛选出执行结果为true的元素。如筛选出所有学生：
```
    List<Person> result = list.stream()
                        .filter(Person::isStudent)
                        .collect(toList());
```

## 去重distinct
    去掉重复的结果：
```
    List<Person> result = list.stream()
                        .distinct()
                        .collect(toList());
```                                                          

## 截取
    截取流的前N个元素：
```
    List<Person> result = list.stream()
                        .limit(3)
                        .collect(toList());
```

## 跳过
    跳过流的前N个元素：
```
    List<Person> result = list.stream()
                        .skip(3)
                        .collect(toList());
```

## 映射
        对流中的每个元素执行一个函数，使得元素转换成另一种类型输出。流会将每一个元素输送给map函数，并执行map中的lambda表达式，最后将执行结果存入一个新的流中。如获取每个人的姓名（实则是将Person类转换成String类型）：
```
    List<Person> result = list.stream()
                        .map(Person::getName)
                        .collect(toList);
```                                                    

## 合并多个流
    