## 应用

把两类数据组合成一组

其实结构体可以实现同样效果，但是pair写法更简单

## 头文件

$<utility>$

## 定义

```c++
pair<int,double> p1;
//创建一个空的pair对象，括号内2个类型都可以换成任意类型
pair<int,char> p2(0,'a');
//定义的同时初始化，依次赋给对应的类型
make_pair(0,'a');
//把两个类型组合成pair类型
```

## 成员访问

```c++
p1.first
//访问第一个类型的值
p1.second
//访问第二个类型的值
```

## 赋值
```c++
p1.first=1;
//给pair类型里的类型赋值
p1=p2;
//给pair类型整体赋值
```

## 大小比较

```c++
p1<p2 
等价 
p1.first<p2.first || p1.first==p2.first && p1.second<p2.seconde

p1==p2 
等价 
p1.first==p2.first && p1.second==p2.second$
```

## 接收

函数以pair对象作为返回值时，可以通过tie进行接收

或者赋值给pair类型
```c++
pair<int,int> f()
{
    return make_pair(1,1);
}
int a,b;
tie(a,b)=f();

pair<int,int> p1=f();
```
