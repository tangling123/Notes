# C++类模板

```c++
template <class 类型参数1, class 类型参数2, ...>
class 类模板名{
    成员函数和成员变量
};
```

类模板中的成员函数放到类模板定义**外面**写时的语法如下：

```c++
template<class 类型参数1, class 类型参数2, ...>
返回值类型 类模板名<类型参数名列表>:: 成员函数名(参数表){
...
}
```

用类模板定义对象的写法如下：

```c++
类模板名<真实类型参数表> 对象名(构造函数实际参数表);
```

如果类模板有无参构造函数，那么也可以使用如下写法：

```c++
类模板名 <真实类型参数表> 对象名;
```

利用模板类来求两个相同类型变量的最大值和最小值

```c++
template <class DataType>
class Compare
{
public:
    Compare(DataType x, DataType y){
        this->x = x, this->y = y;
    };  
    DataType max(){
        return x > y ? x : y;
    }
    DataType min(){
        return x < y ? x : y;
    }
private:
    DataType x, y;
};

int main()
{
    Compare<int> c1(1, 2);
    cout << "max: " << c1.max() << endl;
    cout << "min: " << c1.min() << endl;
    
    Compare<float> c2(1.1, 2.2);
    cout << "max: " << c2.max() << endl;
    cout << "min: " << c2.min() << endl;
    return 0;
}
```

使用类模板构建一个Pair类

```c++
#include <iostream>
#include <string>
using namespace std;

template <class T1,class T2>
class Pair
{
public:
    T1 key;  //关键字
    T2 value;  //值
    Pair(T1 k,T2 v):key(k),value(v) { };
    bool operator < (const Pair<T1,T2> & p){
        return key < p.key;
    }
};

int main()
{
    Pair<string,int> s1("Tom",19); //实例化出一个类 Pair<string,int>
    Pair<string,int> s2("Leon",23);
    if (s1 < s2)
        cout << s2.key << " " << s2.value;
    else
        cout << s1.key << " " << s1.value;
    return 0;
}
```





