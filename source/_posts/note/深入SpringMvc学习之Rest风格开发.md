---
title: 深入SpringMvc学习之路（一）Rest风格开发
link: SpringMvc-Rest
date: 2022-10-10 11:36:44
tags: 后端,SpringMvc
catalog: true
lang: cn
categories: 笔记
---

理解Rest是什么，Rest的优缺点，以及Rest的四个动作（增、删、查、改）的格式，

# Rest是什么

表述性状态转移是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是RESTful。需要注意的是，REST是设计风格而不是标准。REST通常基于使用[HTTP](https://baike.baidu.com/item/HTTP?fromModule=lemma_inlink)，URI，和[XML](https://baike.baidu.com/item/XML?fromModule=lemma_inlink)（[标准通用标记语言](https://baike.baidu.com/item/%E6%A0%87%E5%87%86%E9%80%9A%E7%94%A8%E6%A0%87%E8%AE%B0%E8%AF%AD%E8%A8%80?fromModule=lemma_inlink)下的一个子集）以及[HTML](https://baike.baidu.com/item/HTML?fromModule=lemma_inlink)（标准通用标记语言下的一个应用）这些现有的广泛流行的协议和标准。

# Rest风格描述形式的优点

![简介](简介.png)

明显看到，传统风格对于访问资源的行为并无隐藏，同时，其格式相较Rest来说更加的复杂，杂乱而不统一。

# Rest的四个动作格式

![格式](格式.png)

# Rest入门案例

## 初始注解开发

举个栗子

```java
@Controller
@ResponseBody配置在类上可以简化配置，表示设置当前每个方法的返回值都作为响应体
//@ResponseBody
//@RestController     //使用@RestController注解替换@Controller与@ResponseBody注解，简化书写
@RequestMapping("/books")
public class BookController {

    @RequestMapping( method = RequestMethod.POST)
    public String save(@RequestBody Book book){
        System.out.println("book save..." + book);
        return "{'module':'book save'}";
    }

    @RequestMapping(value = "/{id}" ,method = RequestMethod.DELETE)
//    @DeleteMapping("/{id}")     //使用@DeleteMapping简化DELETE请求方法对应的映射配置
    public String delete(@PathVariable Integer id){
        System.out.println("book delete..." + id);
        return "{'module':'book delete'}";
    }

    @RequestMapping(method = RequestMethod.PUT)
//    @PutMapping         //使用@PutMapping简化Put请求方法对应的映射配置
    public String update(@RequestBody Book book){
        System.out.println("book update..."+book);
        return "{'module':'book update'}";
    }

    @RequestMapping(value = "/{id}" ,method = RequestMethod.GET)
//    @GetMapping("/{id}")    //使用@GetMapping简化GET请求方法对应的映射配置
    public String getById(@PathVariable Integer id){
        System.out.println("book getById..."+id);
        return "{'module':'book getById'}";
    }

    @RequestMapping(method = RequestMethod.GET)
//    @GetMapping             //使用@GetMapping简化GET请求方法对应的映射配置
    public String getAll(){
        System.out.println("book getAll...");
        return "{'module':'book getAll'}";
    }
}
```

## 优化注解开发

` @RestController`注解包含了`@Controller`与`@ResponseBody`注解，实现简化书写

### @RequestMapping优化

1. 增`@RequestMapping( method = RequestMethod.POST)`=>`@PostMapping`

2. 删`@RequestMapping(value = "/{id}" ,method = RequestMethod.DELETE)`=>`@DeleteMapping("/{id}")`

3. 改`@RequestMapping(method = RequestMethod.PUT)`=>`@PutMapping`

4. 查`@RequestMapping(value = "/{id}" ,method = RequestMethod.GET)`=>`@GetMapping("/{id}")`

```java
//标准REST风格控制器开发
@RestController
@RequestMapping("/books")
public class BookController2 {

    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save..." + book);
        return "{'module':'book save'}";
    }

    @DeleteMapping("/{id}")
    public String delete(@PathVariable Integer id){
        System.out.println("book delete..." + id);
        return "{'module':'book delete'}";
    }

    @PutMapping
    public String update(@RequestBody Book book){
        System.out.println("book update..."+book);
        return "{'module':'book update'}";
    }

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println("book getById..."+id);
        return "{'module':'book getById'}";
    }

    @GetMapping
    public String getAll(){
        System.out.println("book getAll...");
        return "{'module':'book getAll'}";
    }
}
```

## 注解理解

### PathVariable

![PathVariable](PathVariable.png)

### 注解选择

![参数](参数.png "注解选择")
