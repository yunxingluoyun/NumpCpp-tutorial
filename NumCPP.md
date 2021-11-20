[TOC]

### NumCPP线性代数

**NumCpp**中的主要数据结构是`NdArray`。它本质上是一个 2D 数组类，一维数组实现为1xN数组。还有一个`DataCube`类作为便利容器提供，用于存储2D数组`NdArray`，但它通过简单容器的用途有限。

|                  Numpy                   |                      NumCpp                       |
| :--------------------------------------: | :-----------------------------------------------: |
| `a = np.array([[1, 2], [3, 4], [5, 6]])` | `nc::NdArray<int> a = { {1, 2}, {3, 4}, {5, 6} }` |
|           `a.reshape([2, 3])`            |                 `a.reshape(2, 3)`                 |
|          `a.astype(np.double)`           |               `a.astype<double>()`                |

**示例1：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 简单示例
    int a = 10;
    // 生成2×2的int类型矩阵（NdArray）
    nc::NdArray<int> a0 = { {1, 2}, {3, 4} };
    // 生成3×2的int类型矩阵（NdArray）
    nc::NdArray<int> a1 = { {1, 2}, {3, 4}, {5, 6} };
    
    cout << "查看a数据类型：\n" << typeid(a).name() << endl;
    cout << "查看a0数据类型：\n"<< typeid(a0).name() << endl;
    cout << "a0:\n" << a0 << endl;
    cout << "a1:\n" << a1 << endl;

    a1.reshape(2, 3);
    cout << "改变a1形状（2×3）:\n" << a1 << endl;

    auto a2 = a1.astype<double>();
    cout << "int类型转换duoble类型：\n" << typeid(a2).name() << endl;
    cout << "a2:\n" << a1 << endl;
 
    return 0;
}

```

结果：

```c++
查看a数据类型：
int
查看a0数据类型：
class nc::NdArray<int,class std::allocator<int> >
a0:
[[1, 2, ]
[3, 4, ]]

a1:
[[1, 2, ]
[3, 4, ]
[5, 6, ]]

改变a1形状（2×3）:
[[1, 2, 3, ]
[4, 5, 6, ]]

int类型转换duoble类型：
class nc::NdArray<double,class std::allocator<double> >
a2:
[[1, 2, 3, ]
[4, 5, 6, ]]
```

###### 1. 矩阵初始化

　　NumCpp 提供了许多初始化函数，它们返回`NdArray`。

| NumPy                   | NumCpp                                            |
| ----------------------- | ------------------------------------------------- |
| `np.linspace(1, 10, 5)` | `nc::linspace<dtype>(1, 10, 5)`                   |
| `np.arange(3, 7)`       | `nc::arrange<dtype>(3, 7)`                        |
| `np.eye(4)`             | `nc::eye<dtype>(4)`                               |
| `np.zeros([3, 4])`      | `nc::zeros<dtype>(3, 4)`                          |
|                         | `nc::NdArray<dtype>(3, 4) a = 0`                  |
| `np.ones([3, 4])`       | `nc::ones<dtype>(3, 4)`                           |
|                         | `nc::NdArray<dtype>(3, 4) a = 1`                  |
| `np.nans([3, 4])`       | `nc::nans<dtype>(3, 4)`                           |
|                         | `nc::NdArray<dtype>(3, 4) a = nc::constants::nan` |
| `np.empty([3, 4])`      | `nc::empty<dtype>(3, 4)`                          |
|                         | `nc::NdArray<dtype>(3, 4) a;`                     |

**示例2：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 矩阵初始化
    //在指定的1-10间隔内返回均匀间隔的5个数
    auto a1 = nc::linspace<int>(1, 10, 5);
    //在给定间隔内返回均匀间隔的值
    auto a2 = nc::arange<int>(3, 7);
    //单位矩阵
    auto a3 = nc::eye<int>(4);
    //全零矩阵
    auto a4 = nc::zeros<int>(3, 4);
    auto a5 = nc::NdArray<int>(3, 4) = 0;
    //全1矩阵
    auto a6 = nc::ones<int>(3, 4);
    auto a7 = nc::NdArray<int>(3, 4) = 1;
    //全nan矩阵
    auto a8 = nc::nans(3, 4);
    auto a9 = nc::NdArray<double>(3, 4) = nc::constants::nan;
    //空矩阵
    auto a10 = nc::empty<int>(3, 4);
    auto a11 = nc::NdArray<int>(3, 4);

    cout << "a1:\n" << a1 << endl;
    cout << "a2:\n" << a2 << endl;
    cout << "a3:\n" << a3 << endl;
    cout << "a4:\n" << a4 << endl;
    cout << "a5:\n" << a5 << endl;
    cout << "a6:\n" << a6 << endl;
    cout << "a7:\n" << a7 << endl;
    cout << "a8:\n" << a8 << endl;
    cout << "a9:\n" << a9 << endl;
    cout << "a10:\n" << a10 << endl;
    cout << "a11:\n" << a11 << endl;
 
    return 0;
}

```

结果：

```python
a1:
[[1, 3, 5, 7, 10, ]]

a2:
[[3, 4, 5, 6, ]]

a3:
[[1, 0, 0, 0, ]
[0, 1, 0, 0, ]
[0, 0, 1, 0, ]
[0, 0, 0, 1, ]]

a4:
[[0, 0, 0, 0, ]
[0, 0, 0, 0, ]
[0, 0, 0, 0, ]]

a5:
[[0, 0, 0, 0, ]
[0, 0, 0, 0, ]
[0, 0, 0, 0, ]]

a6:
[[1, 1, 1, 1, ]
[1, 1, 1, 1, ]
[1, 1, 1, 1, ]]

a7:
[[1, 1, 1, 1, ]
[1, 1, 1, 1, ]
[1, 1, 1, 1, ]]

a8:
[[nan, nan, nan, nan, ]
[nan, nan, nan, nan, ]
[nan, nan, nan, nan, ]]

a9:
[[nan, nan, nan, nan, ]
[nan, nan, nan, nan, ]
[nan, nan, nan, nan, ]]

a10:
[[-1030135744, 741, -1030129280, 741, ]
[1, 1, 1, 1, ]
[1, 1, 1, 1, ]]

a11:
[[-1030138000, 741, -1030129280, 741, ]
[-1030142624, 741, -1030142624, 741, ]
[-1030142624, 741, -1030142624, 741, ]]
```

###### 2.切片与广播

**NumCpp** 提供类似 **NumPy** 的切片与广播。

|   **NumPy**    |              **NumCpp**               |
| :------------: | :-----------------------------------: |
|   `a[2, 3]`    |               `a(2, 3)`               |
| `a[2:5, 5:8]`  | `a(nc::Slice(2, 5), nc::Slice(5, 8))` |
|                |          `a({2, 5}, {5, 8})`          |
|   `a[:, 7]`    |          `a(a.rSlice(), 7)`           |
|   `a[a > 5]`   |              `a[a > 0]`               |
| `a[a > 5] = 0` |         `a.putMask(a > 5, 0)`         |

**示例3：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 切片与广播
    //随机生成6×6的int矩阵
    auto a1 = nc::random::randInt<int>({ 6, 6 }, 0, 100);
    //获取第3行第4列的值
    auto value = a1(2, 3);
    //获取第3-5行和第3-5列的值
    auto slice = a1({ 2, 5 }, { 2, 5 });
    //获取第3、5行和第3、5列的值
    auto slice1 = a1({ 2, 5 ,2}, { 2, 5 ,2});
    //获取第6列的值
    auto rowSlice = a1(a1.rSlice(), 5);
    //获取大于50的值
    auto values = a1[a1 > 50];

    cout << "a1:\n" << a1 << endl;
    cout << "value:\n" << value << endl;
    cout << "slice:\n" << slice << endl;
    cout << "slice1:\n" << slice1 << endl;
    cout << "rowSlice:\n" << rowSlice << endl;
    cout << "values :\n" << values << endl;

    //将大于50的值替换为666
    a1.putMask(a1 > 50, 666);
    cout << "a1:\n" << a1 << endl;

    return 0;
}

```

结果：

```c++
a1:
[[30, 8, 20, 22, 96, 98, ]
[29, 78, 56, 2, 33, 7, ]
[0, 67, 68, 32, 62, 31, ]
[57, 13, 58, 34, 25, 66, ]
[11, 5, 78, 30, 80, 41, ]
[56, 98, 73, 90, 76, 47, ]]

value:
32
slice:
[[68, 32, 62, ]
[58, 34, 25, ]
[78, 30, 80, ]]
slice1:
[[68, 62, ]
[78, 80, ]]

rowSlice:
[[98, ]
[7, ]
[31, ]
[66, ]
[41, ]
[47, ]]

values :
[[96, 98, 78, 56, 67, 68, 62, 57, 58, 66, 78, 80, 56, 98, 73, 90, 76, ]]

a1:
[[30, 8, 20, 22, 666, 666, ]
[29, 666, 666, 2, 33, 7, ]
[0, 666, 666, 32, 666, 31, ]
[666, 13, 666, 34, 25, 666, ]
[11, 5, 666, 30, 666, 41, ]
[666, 666, 666, 666, 666, 47, ]]
```

###### 3.随机模块

随机模块提供了创建随机`NdArray`的简单方法

|             **NumPy**              |                     **NumCpp**                     |
| :--------------------------------: | :------------------------------------------------: |
|       `np.random.seed(666)`        |              `nc::random::seed(666)`               |
|      `np.random.randn(3, 4)`       |    `nc::random::randN<double>(nc::Shape(3, 4))`    |
|                                    |        `nc::random::randN<double>({3, 4})`         |
| `np.random.randint(0, 10, [3, 4])` | `nc::random::randInt<int>(nc::Shape(3, 4), 0, 10)` |
|                                    |     `nc::random::randInt<int>({3, 4}, 0, 10)`      |
|       `np.random.rand(3, 4)`       |     `nc::random::rand<double>(nc::Shape(3,4))`     |
|                                    |         `nc::random::rand<double>({3, 4})`         |
|      `np.random.choice(a, 3)`      |             `nc::random::choice(a, 3)`             |

**示例4:**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 随机模块
    //随机数种子
    nc::random::seed(666);
    //randN函数返回一个或一组样本，具有标准正态分布
    auto a1 = nc::random::randN<double>({ 3, 4 });
    //randInt函数返回一个或一组样本，数值在0-10之间
    auto a2 = nc::random::randInt<int>({ 3, 4 }, 0, 10);
    //rand函数根据给定维度生成[0,1)之间的数据，包含0，不包含1
    auto a3 = nc::random::rand<double>({ 3, 4 });
    //从a3中随机选出3个数
    auto a4 = nc::random::choice(a3, 3);

    cout << "a1:\n" << a1 << endl;
    cout << "a2:\n" << a2<< endl;
    cout << "a3:\n" << a3 << endl;
    cout << "a4:\n" << a4 << endl;

    return 0;
}

```

结果：

```c++
a1:
[[-1.16936, -0.153505, -0.142384, -0.275061, ]
[-2.18089, 1.07539, -1.28937, -0.834309, ]
[0.0730356, 0.0880969, -0.168976, -1.6548, ]]

a2:
[[7, 8, 7, 9, ]
[1, 8, 9, 0, ]
[6, 1, 8, 8, ]]

a3:
[[0.181713, 0.79305, 0.570944, 0.0892013, ]
[0.387142, 0.395861, 0.986932, 0.180984, ]
[0.22769, 0.744834, 0.599702, 0.0555396, ]]

a4:
[[0.79305, 0.387142, 0.180984, ]]
```

###### 4.拼接

|           **NumPy**           |              **NumCpp**               |
| :---------------------------: | :-----------------------------------: |
| `np.stack([a, b, c], axis=0)` | `nc::stack({a, b, c}, nc::Axis::ROW)` |
|    `np.vstack([a, b, c])`     |        `nc::vstack({a, b, c})`        |
|    `np.hstack([a, b, c])`     |        `nc::hstack({a, b, c})`        |
|   `np.append(a, b, axis=1)`   |   `nc::append(a, b, nc::Axis::COL)`   |

**示例5：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 拼接
    auto a = nc::random::randInt<int>({ 3, 4}, 0, 10);
    auto b = nc::random::randInt<int>({ 3, 4 }, 0, 10);
    auto c = nc::random::randInt<int>({ 3, 4 }, 0, 10);
    auto d = nc::random::randInt<int>({ 1, 4 }, 0, 10);

    //在行方向进行拼接
    auto a1 = nc::stack({ a, b, c }, nc::Axis::ROW);
    auto a2 = nc::vstack({ a, b, c });
    //在列方向进行拼接
    auto a3 = nc::hstack({ a, b, c });
    //在列方向追加
    auto a4 = nc::append(a, b, nc::Axis::COL);
    //在行方向追加
    auto a5 = nc::append(a, d, nc::Axis::ROW);

    cout << "a1:\n" << a1 << endl;
    cout << "a2:\n" << a2<< endl;
    cout << "a3:\n" << a3 << endl;
    cout << "a4:\n" << a4 << endl;
    cout << "a5:\n" << a5 << endl;

    return 0;
}

```

结果：

```
a1:
[[0, 8, 0, 2, ]
[6, 8, 9, 8, ]
[6, 2, 3, 7, ]
[0, 7, 8, 2, ]
[2, 1, 7, 3, ]
[8, 4, 5, 6, ]
[1, 5, 8, 0, ]
[0, 1, 6, 8, ]
[3, 0, 6, 7, ]]

a2:
[[0, 8, 0, 2, ]
[6, 8, 9, 8, ]
[6, 2, 3, 7, ]
[0, 7, 8, 2, ]
[2, 1, 7, 3, ]
[8, 4, 5, 6, ]
[1, 5, 8, 0, ]
[0, 1, 6, 8, ]
[3, 0, 6, 7, ]]

a3:
[[0, 8, 0, 2, 0, 7, 8, 2, 1, 5, 8, 0, ]
[6, 8, 9, 8, 2, 1, 7, 3, 0, 1, 6, 8, ]
[6, 2, 3, 7, 8, 4, 5, 6, 3, 0, 6, 7, ]]

a4:
[[0, 8, 0, 2, 0, 7, 8, 2, ]
[6, 8, 9, 8, 2, 1, 7, 3, ]
[6, 2, 3, 7, 8, 4, 5, 6, ]]

a5:
[[0, 8, 0, 2, ]
[6, 8, 9, 8, ]
[6, 2, 3, 7, ]
[2, 6, 2, 4, ]]
```

###### 5.对角线、上三角下三角和翻转

|      **NumPy**       |          **NumCpp**          |
| :------------------: | :--------------------------: |
|   `np.diagonal(a)`   |      `nc::diagonal(a)`       |
|     `np.triu(a)`     |        `nc::triu(a)`         |
|     `np.tril(a)`     |        `nc::tril(a)`         |
| `np.flip(a, axis=0)` | `nc::flip(a, nc::Axis::ROW)` |
|    `np.flipud(a)`    |       `nc::flipud(a)`        |
|    `np.fliplr(a)`    |       `nc::fliplr(a)`        |

**示例6：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    // 对角矩阵，上、下三角矩阵，翻转
    auto a = nc::random::randInt<int>({ 3, 4}, 0, 10);
    auto d = nc::random::randInt<int>({ 5, 5 }, 0, 10);
    //生成对角矩阵
    auto b = nc::diag<int>({ 1,2,3,4 });
    //获取主对角线的元素
    auto a1 = nc::diagonal(d);
    //上三角矩阵
    auto a2 = nc::triu(a);
    //下三角矩阵
    auto a3 = nc::tril(a);
    //沿着列翻转
    auto a4 = nc::flip(d, nc::Axis::ROW);
    //上下翻转
    auto a5 = nc::flipud(d);
    //左右翻转
    auto a6 = nc::fliplr(d);

    cout << "a:\n" << a << endl;
    cout << "b:\n" << b << endl;
    cout << "d:\n" << d << endl;
    cout << "a1:\n" << a1 << endl;
    cout << "a2:\n" << a2<< endl;
    cout << "a3:\n" << a3 << endl;
    cout << "a4:\n" << a4 << endl;
    cout << "a5:\n" << a5 << endl;
    cout << "a6:\n" << a6 << endl;

    return 0;
}

```

结果：

```
a:
[[0, 8, 0, 2, ]
[6, 8, 9, 8, ]
[6, 2, 3, 7, ]]

b:
[[1, 0, 0, 0, ]
[0, 2, 0, 0, ]
[0, 0, 3, 0, ]
[0, 0, 0, 4, ]]

d:
[[0, 7, 8, 2, 2, ]
[1, 7, 3, 8, 4, ]
[5, 6, 1, 5, 8, ]
[0, 0, 1, 6, 8, ]
[3, 0, 6, 7, 2, ]]

a1:
[[0, 7, 1, 6, 2, ]]

a2:
[[0, 8, 0, 2, ]
[0, 8, 9, 8, ]
[0, 0, 3, 7, ]]

a3:
[[0, 0, 0, 0, ]
[6, 8, 0, 0, ]
[6, 2, 3, 0, ]]

a4:
[[3, 0, 6, 7, 2, ]
[0, 0, 1, 6, 8, ]
[5, 6, 1, 5, 8, ]
[1, 7, 3, 8, 4, ]
[0, 7, 8, 2, 2, ]]

a5:
[[3, 0, 6, 7, 2, ]
[0, 0, 1, 6, 8, ]
[5, 6, 1, 5, 8, ]
[1, 7, 3, 8, 4, ]
[0, 7, 8, 2, 2, ]]

a6:
[[2, 2, 8, 7, 0, ]
[4, 8, 3, 7, 1, ]
[8, 5, 1, 6, 5, ]
[8, 6, 1, 0, 0, ]
[2, 7, 6, 0, 3, ]]
```

###### 7.遍历

**NumCpp**遵循 C++的标准模板库，提供以不同的方式在`NdArray`上进行遍历。

|    **NumPy**     |                   **NumCpp**                   |
| :--------------: | :--------------------------------------------: |
| `for value in a` | `for(auto it = a.begin(); it < a.end(); ++it)` |
|                  |             `for(auto& value : a)`             |

**示例8：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    auto a = nc::random::randInt<int>({ 4, 4}, 0, 255);
    auto b = nc::arange<int>(3, 7);
    cout << "a:\n" << a << endl;

    // 遍历（类似于灰度图像数值取反操作）
    for (auto it = a.begin(); it < a.end(); ++it)
    {
        cout << *it << " ";
        *it = 255 - *it;
    }
    cout << endl;
    cout << "第一次取反:\n" << a << endl;

    for (auto& arrayValue : a)
    {
        cout << arrayValue << " ";
        arrayValue = 255 - arrayValue;
    }
    cout << endl;
    cout << "第二次取反:\n" << a << endl;

    return 0;
}

```

结果：

```
a:
[[175, 33, 200, 247, ]
[86, 163, 184, 228, ]
[231, 7, 108, 72, ]
[175, 52, 68, 167, ]]

175 33 200 247 86 163 184 228 231 7 108 72 175 52 68 167
第一次取反:
[[80, 222, 55, 8, ]
[169, 92, 71, 27, ]
[24, 248, 147, 183, ]
[80, 203, 187, 88, ]]

80 222 55 8 169 92 71 27 24 248 147 183 80 203 187 88
第二次取反:
[[175, 33, 200, 247, ]
[86, 163, 184, 228, ]
[231, 7, 108, 72, ]
[175, 52, 68, 167, ]]
```

###### 8.逻辑

**NumCpp**中的逻辑函数与**NumPy**的行为相同。

|        **NumPy**        |        **NumCpp**        |
| :---------------------: | :----------------------: |
| `np.where(a > 5, a, b)` | `nc::where(a > 5, a, b)` |
|       `np.any(a)`       |       `nc::any(a)`       |
|       `np.all(a)`       |       `nc::all(a)`       |
| `np.logical_and(a, b)`  | `nc::logical_and(a, b)`  |
|  `np.logical_or(a, b)`  |  `nc::logical_or(a, b)`  |
|   `np.isclose(a, b)`    |   `nc::isclose(a, b)`    |
|   `np.allclose(a, b)`   |   `nc::allclose(a, b)`   |

**示例9：**

```c++
#include"NumCpp.hpp"
#include"boost/filesystem.hpp"
#include<iostream>
using namespace std;

int main()
{
    auto a = nc::random::randInt<int>({ 3, 4}, 0, 10);
    auto b = nc::random::randInt<int>({ 3, 4}, 0, 10);
    auto c = nc::random::randN<double>({ 3, 4 });
    auto d = nc::random::randN<double>({ 3, 4 });
    //a中元素大于5保留，小于5用b中元素替代
    auto a1 = nc::where(a > 5, a, b);
    //任意一个元素为True，输出为True
    auto a2 = nc::any(a);
    ////所有元素为True，输出为True
    auto a3 = nc::all(a);
    //与
    auto a4 = nc::logical_and(a, b);
    //或
    auto a5 = nc::logical_or(a, b);
    //比较两个array是不是每一元素都相等，默认在1e-05的误差范围内
    auto a6 = nc::isclose(c, d);
    //比较两个array是不是所有都相等，默认在1e-05的误差范围内
    auto a7 = nc::allclose(a, b);

    cout << "a:\n" << a << endl;
    cout << "b:\n" << b << endl;
    cout << "a1:\n" << a1 << endl;
    cout << "a2:\n" << a2 << endl;
    cout << "a3:\n" << a3 << endl;
    cout << "a4:\n" << a4 << endl;
    cout << "a5:\n" << a5 << endl;
    cout << "a6:\n" << a6 << endl;
    cout << "a7:\n" << a7 << endl;

    return 0;
}

```

结果：

```
a:
[[0, 8, 0, 2, ]
[6, 8, 9, 8, ]
[6, 2, 3, 7, ]]

b:
[[0, 7, 8, 2, ]
[2, 1, 7, 3, ]
[8, 4, 5, 6, ]]

a1:
[[0, 8, 8, 2, ]
[6, 8, 9, 8, ]
[6, 4, 5, 7, ]]

a2:
[[1, ]]

a3:
[[0, ]]

a4:
[[0, 1, 0, 1, ]
[1, 1, 1, 1, ]
[1, 1, 1, 1, ]]

a5:
[[0, 1, 1, 1, ]
[1, 1, 1, 1, ]
[1, 1, 1, 1, ]]

a6:
[[0, 0, 0, 0, ]
[0, 0, 0, 0, ]
[0, 0, 0, 0, ]]

a7:
0
```

![](https://cdn.jsdelivr.net/gh/yunxingluoyun/blog-img/QQ截图20211120002727.png)

