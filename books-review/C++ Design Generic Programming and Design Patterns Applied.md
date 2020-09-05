## C++ Design Generic Programming and Design Patterns Applied

*author: Andrei Alexandrescu*



#### 第一章： 基于策略的类设计



注意这里并不是四人帮说的策略模式(strategy design)。Policy指的是编译时的。



1, 比较了继承方式和模板方式的优缺点。

| 设计方式 | 优点                                   | 缺点                                                         |
| -------- | -------------------------------------- | :----------------------------------------------------------- |
| 模板     | 1，类型信息丰富。                      | 1，模板特化扩展性不好。<br>2,   模板特化的成员函数只能有一个默认特化参数。 |
| 类继承   | 1，类继承扩展性较好。<br>2,   通过基类 | 1，丢失类型信息。                                            |
| 组合     | 1，灵活自由。解耦简单。[作者增加]      |                                                              |

模板函数成员不能有多个默认特化参数示例代码：

```
template <class T>
class Widget
{
public:
    void Fun();
};

template<>
void Widget<char>::Fun()
{

}

template<>
void Widget<int>::Fun()
{

}


template <class T, class U>
class Gadget
{
    void Fun();
};

/* Compile Error*/
/* nested name specifier 'Gadget<char, U>::' for declaration does not refer into a class,class template or class template partial specialization  */
  
template<class U>
void Gadget<char,U>::Fun()
{

}
```



模板中的模板参数：

```
template <class T>
struct OpNewCreator
{
    static T* Create()
    {
        return new T;
    }
};


template <class CreatePolicy>
class WidgetManager: public CreatePolicy
{

};

template <template <class CreatePolicy> class CreatePolicy>
class WidgetManager2: public CreatePolicy<Widget<>>
{

};


template <template <class CreatePolicy> class CreatePolicy = OpNewCreator >
class WidgetManager3: public CreatePolicy<Widget<>>
{

};

typedef WidgetManager<OpNewCreator<Widget<>>> WidgetPtr;
typedef WidgetManager2<OpNewCreator> WidgetPtr2;
typedef WidgetManager3<> WidgetPtr3;
```

可以从代码中看出，在编译时刻就确定类型。这对于要求typesafe的业务代码是有好处的。



第一章重点综合的运用 组合+继承+模板特化+模板模板参数 来解决方案设计的多样性问题。 用最少的代码来解决设计中的设计问题。



开门见山，一刀直入核心。