`vector`是一个变长数组，就是说他的长度会随着加入进去的元素而增长，而且他支持多种类型的元素插入，但是对于某一确定的`vector`就只能插入所定义类型的元素。并且他也和数组一样，可以用下标访问，下标也是从0开始。

## 头文件

```c++
#include<vector>
```

## 定义

```c++
vector<[type]> [vector_name];//创建了一个名为[vector_name]的vector，可以往内部插入[type]型的元素
vector<[type]> [vector_name][10];//创建了一个vector数组
```

## 使用

### 插入

往`vector`中最后面插入元素t，插入后`vector`的长度会增加一。

```c++
[vector_name].push_back(t); 
```

### 删除

`vector`有两种删除方式，一个是单个删除一个是区间删除，都是同一个函数，他们的参数都是`vector`类型的迭代器。删除元素之后vector的长度也会减小，后面的元素也会跟上来。

```c++
[vector_name].erase(iterator); //删除iterator位置的元素
[vector_name].erase(iterator_l, iterator_r); //删除[l,r]区间的所有元素
```

因为`[vector_name].begin()`返回的是`vector`第一个元素的迭代器，那么我们可以用`[vector_name].begin()+下标`表示目标位置的迭代器。就可以简单实现删除操作了。

```c++
[vector_name].erase([vector_name].begin()+8); //删除下标为8的元素
[vector_name].erase([vector_name].begin()+l, [vector_name].begin()+r); //删除下标为[l,r)的所有元素
```

### 获取长度

`[vector_name].size()`返回的是`vector`的长度，即内部有多少元素。

```c++
int len = [vector_name].size(); //获取长度
```

### 排序

`sort`也可以对`vector`排序，用`[vector_name].begin()`做左边界，`[vector_name].end()`做右边界即可

```c++
sort([vector_name].begin(), [vector_name].end());
```

### 去重

`STL`自带的`unique`函数能够去除`vector`内部相同元素的函数，但是他并不是严格的去除重复，而是把重复的扔到后面去了，保证前面的部分元素不重复。参数为左右区间迭代器，返回值为**去重后容器中不重复序列的最后一个元素的下一个元素**，简单点就是从这个元素开始与前面的元素会重复，前方的部分保证不重复。

```c++
iterator unique(iterator it_1,iterator it_2);
```

然后利用这个的返回值，删除原vector从当前迭代器之后的元素，即可达成去重操作

```c++
[vector_name].erase(unique([vector_name].begin(), [vector_name].end()), [vector_name].end());//保留下来的元素即为去重之后的元素。
```

### 清空

利用`clear`函数可以一键清空`vector`中的所有元素

```c++
[vector_name].clear();
```

## 运用

最普通的就是插入元素。

`vector`因为可以装下很多种类型的元素，也可以装下自己所定义的类型，比如类，结构体，或者`STL`。

比如可以在`vector`中装入一个`vector`来实现二维数组。

```c++
vector<vector<int> > v;//创建了一个可以放下vector的vector，可以用来模拟二维数组，为了防止直接定义二维数组爆内存
```

也可以定义`vector`数组来模拟二维数组

```c++
vector<int> v[10];//创建了一个含有10个vector的vector数组
```

