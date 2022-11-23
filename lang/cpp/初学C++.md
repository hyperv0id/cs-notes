# C++学习笔记



## 1.初见C++









## 2.数据类型









## 3.运算符









## 4.程序流程结构









## 5.数组









## 6.函数









## 7.指针

### 7.1指针的基本概念








### 7.2指针变量的定义和使用









### 7.3指针所占内存空间









### 7.4空指针和野指针

#### **空指针**

指针变量指向内存中编号为0的空间

**用途**：初始化指针变量

**注意**：空指针指向的内存是不可用访问的

```c++
//空指针示例
int main()
{
    //指针变量指向编号为0的空间
    int* p = NULL;
    *p = 100；
//访问空指针出错
//内存编号为0-255的是系统占用的内存，用户不可以访问
cout << *p << endl;
}
```

#### **野指针**

指针变量指向非法的内存空间

```c++
//野指针示例
int main()
{
    //指针变量指向编号为0x1100的空间
    int* p = (int *)0x1100;
//访问空指针出错
cout << *p << endl;
}
```









### 7.5 const 修饰指针

1. const修饰指针----**常量指针**（指向的**值**不可以改）
2. const修饰常量----**指针常量**（**指向**不可用修改）
3. const既修饰指针有修饰常量（都不可修改）

```c++
int main() {
    int a = 10, b = 20;
    //const修饰常量----指针常量
    const int* p1 = &a;
    p1 = &b;
    //p1 = 20;错误
    //const修饰指针----常量指针
    int* const p2 = &a;
    *p2 = 100;//p2 = &b；错误
    //const既修饰指针有修饰常量
    const int* const p3 = &a;
    //p3 = &b;
    //p3 = 100;
    return 0;
}
```

<u>const修饰谁谁不能改</u>





### 7.6指针和数组

利用指针访问数组中的每个元素

```c++
int main() {
    int arr[10] = { 1,2,3,4,5,6,7,8,9,0 };
    int* p = arr;
    for (int i = 0; i < sizeof(arr)/sizeof(arr[0]); i++)
    {
        cout << *p << endl;
    }
    return 0;
}
```







### 7.7指针和函数

指针传递可以在函数中改变实参的值

```c++
void swap(int* p, int* q) {
    int temp =  *p;
    *p = *q;
    *q = temp;
}
int main() {
    int a = 1, b = 2;
    swap(&a, &b);
    cout << "a=" << a << "\tb=" << b << endl;
    return 0;
}

```







### 7.8案例：指针数组函数

用指针实现冒泡排序

```c++
void ptrBubber(int* p ,int len) {
    for (int i = 0; i < len-1; i++) {
        for (int j=0; j<len-i-1; j++) {
            if (p[j] < p[j + 1]) {
                int temp = p[j];
                p[j] = p[j + 1];
                p[j + 1] = temp;
            }
        }
    }
}
int main() {
    int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };
    int* p = arr;
    ptrBubber(arr, sizeof(arr) / sizeof(arr[0]));
    for (int i = 0; i < sizeof(arr) / sizeof(arr[0]);i++) {
        cout << arr[i] << endl;
    }
    return 0;
}
```







## 8.结构体

### 8.1基本概念
```c++

```








### 8.2定义和使用
```c++

```









### 8.3结构体数组
```c++

```









### 8.4结构体指针
```c++

```









### 8.5结构体嵌套结构体

如下，screen结构体被嵌套在了computer结构体里面了

```c++
struct screen
{
    unsigned int ppi = 300;
    int Xpixel = 1080;
    int Ypixel = 2400;
};
struct computer
{
    struct screen Screen;
    string brand;
    float CPU = 16;
    struct GPU;
    float memory=1024;
};

int main() {
    computer mycomputer;
    mycomputer.brand = "Lenovo";
    mycomputer.CPU = 13.9;
    mycomputer.memory = 99.9 + 500;
    mycomputer.Screen.ppi = 300;
    system("pause");
    return 0;
}
```









### 8.6做函数参数
```c++

```









### 8.7结构体中const的使用场景

**作用**：防止误操作

```c++
struct people
{
    int age;
    int height;
    int weight;
};
void print(const people *p) {
    //p->age = 1000;报错
    cout << p->age <<"\t"<< p->height <<"\t" <<p->weight << endl;
}
```









### 8.8案例
```c++

```






