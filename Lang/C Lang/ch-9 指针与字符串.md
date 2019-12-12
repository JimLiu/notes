## 指针与字符串
### 指针是 const
```C
// 表示一旦得到了某个变量的地址，不能再指向其它的变量
int* const q = &i; //q 是 const
*q = 26; //Ok
q++; //Error
```
### 所指的是 const
```C
// 表示不能通过指标修改那个变量（并不能使得那个变量成为 const）
const int *p = &i;
*p = 26; //Error, (*p) 是 const
i = 26; //Ok
p = &j; //Ok
```
### 这些什么意思
```C
// 只有两个意思：
// a. 指针不能修改
// b. 不能通过指针修改
int i;
const int* p1 = &i; // 表示不能通过指标修改变量
int const* p2 = &i; // 同上面
int *const p3 = &i; // 表示指针不能被修改,p3 不能被改变。不能再指向其它的地址
/* 判断哪个被 const 了的标志是 const 在 * 的前面还是后面*/
```
### 转换
```C
// 总是可以将 非 const 的指转换成 const 的
void f(const int* x);
int a = 5;
f(&a) ; // ok, 可以传入一个非 const, 表示函数内部不能修改 *p
const int b = a;
f(&b) ; //ok
```

### const 数组
```C
const int a[] = {1,2,3};
// 数组变量已经是 const 的指针了， 这里的 const 表示数组的每个单元都是 const int;
// 所以必须通过初始化进行赋值
// 如果不希望函数对数据进行修改，可以这样设置函数签名： 
int sum(const int a[], int length)
```

### 指针的计算
#### 动态内存分配
```C
//int *a = (int*) malloc(n*sizeof(int))
#include <stdio.h>
#include <stdlib.h>
int main(void){
    int number;
    int* a;
    int i;
    print("输入数量：");
    scanf("%d", &number);
    a = (int*) malloc(number * sizeof(int));
    for(i=0;i<number;i++){
        scanf("%d",&a[i]);
    }
    for(i=number-1;i>=0;i--){
        printf("%d ", a[i]);
    }
    free(a);
    return 0;
}
```