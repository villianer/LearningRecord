## 数据类型关键字
char
short
int
long
float
double
unsigned
signed
struct
union
enum 
void
## 控制语句关键字 12
if 
else
switch 
case
default
for
do
while
break
continue
goto
return
## 存储类型 5
auto
extern
register
static
const
## 其他关键字
sizeof  typedef  volatile

## sizeof关键字
1. sizeof 是关键字不是函数 功能是计算一个数据占用内存的大小，返回一个无符号整数 表示占了多少个字节
2. 返回值 size_f
3. size_f在32位操作系统下是unsigned int,是一个无符号整数

## 当输入一个10进制数转换成其他数据输出时，符号位将不会被考虑，只是将补码看作其他数据类型的形式 比如将-1输出位16进制 只是将int类型的-1的补码转化位16进制无符号数而已

## 整型
1. short 
2. unsigned short
3. int 
4. unsigned short
5. long 
6. unsigned long
7. long long
8. unsigned long long

## char类型 占一个字节 以ascii码形式存储 输出位%c
## 转义字符 也是以char类型存储 但是输入时可以以转义字符的形式 char unsigned char

## 浮点类型
float 4byte  double 8byte  float以f结尾 不以f结尾的小数默认为double  输出%f 或者 %lf
输入方式分传统小数输入和科学计数法输入两种
![](https://private-warehouse-1317335037.cos.ap-guangzhou.myqcloud.com/Test/Screenshot%202023-03-24%20132124.png)
