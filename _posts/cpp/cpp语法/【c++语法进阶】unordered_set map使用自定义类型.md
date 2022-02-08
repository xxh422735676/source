---
title: unordered_map set自定义对象的hash
author: MoonXu
description: 懒癌犯了，摘要同标题~
comments: true
sticky: '0'
tags:
  - cpp语法
categories:
  - cpp
  - cpp语法
password: ''
abbrlink: 14298
date: 2022-02-08 18:03:23
---

`unordered_map/set`使用hash进行存储，因此存储自定义对象前，必须：

1. `hash`告知此容器如何生成hash值，
2. `equal_to`告知容器当出现hash冲突时，如何区分hash值相同的不同对象。

**具体有4种方案**：

1. 定义两个函数对象ObjectHash，以及ObjectCmp，分别实现对Object进行hash，以及比较两个对象是否相同。
2. 定义两个普通的函数，实现hash以及对象比较，与==1==不同的是普通函数在构建`unordered_map/set`时，需要decltype来减少声明它的类型（或手动指定，`std::function <size_t(const Object&)>`说明hash类型，`std::function <bool(const Object&, const Object&)>`说明比较cmp类型
3. 定义两个lambda表达式（仿函数），与2类似
4. 对Object对象进行模板特化定制

```cpp
# unordered_set的声明
template<
    class Key, //类型
    class Hash = std::hash<Key>,
    class KeyEqual = std::equal_to<Key>,
    class Allocator = std::allocator<Key>
> class unordered_set;

#unordered_map的声明
template<class Key, //key的类型
    class Ty, //val的类型
    class Hash = std::hash<Key>,
    class Pred = std::equal_to<Key>,
    class Alloc = std::allocator<std::pair<const Key, Ty> > >
    class unordered_map;
> class unordered_map
```



### 示例

```cpp
# 自定义struct的类型
struct Object{
    string name;
    int val;
};
```

#### 定义两个函数对象

```cpp

struct ObjectHash{
	size_t operator()(const Object& rhs)const{
        return hash<string>()(rhs.name) ^ hash<int>()(rhs.val);
    }
};

struct ObjectCmp{
    bool operator()(const Object& lhs,const Object& rhs)const{
        return lhs.name == rhs.name && lhs.val ==rhs.val;
    }
};

unordered_set<Object,ObjectHash,ObjectCmp> objects;
```

#### 定义两个普通函数，重写hash和cmp

```cpp

size_t ObjectHash(const Object& rhs){
    return hash<string>()(rhs.name) ^ hash<int>()(rhs.val);
}

bool ObjectCmp(const Object& lhs, const Record& rhs){
    return lhs.name == rhs.name && lhs.val == rhs.val;
}

unordered_set<Object , decltype(&ObjectHash), decltype(&ObjectCmp)> objects(0,ObjectHash,ObjectCmp);
```

#### 使用lambda函数

```cpp
auto ObjectHash = [](const Object& rhs){
    return hash<string>(rhs.name) ^ hash<int>()(rhs.val);
};

auto ObjectCmp = [](const Object& lhs, const Object& rhs){
    return lhs.name==rhs.name && lhs.val==rhs.val;
};

unordered_set<Object, decltype(&ObjectHash),decltype(&ObjectCmp)>object(0,ObjectHash,ObjectCmp);
```

#### 模板定制重写

```cpp
//注意namespace ，必要时可以指定namespace std{}
template<>
struct hash<Object>{
    size_t operator()(const Object& rhs)const{
		return hash<string>()(rhs.name) ^ hash<int>()(rhs.val);
    }
};

template<>
struct equal_to<Object>{
    bool operator()(const Object& lhs,const Object& rhs)const{
		return lhs.name == rhs.name && lhs.val == rhs.val;
    }
};

unordered_set<Object> objects;
```





### 参考资料

https://blog.csdn.net/lpstudy/article/details/54345050