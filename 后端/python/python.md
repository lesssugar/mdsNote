# 编码方式

python2需要修改为utf-8

```python
#-*- encoding:utf-8 -*-
```

# 注释

```python
#	//单行注释
var x = 、、、
、、、			#多行注释,可以赋值给变量，一个大字符串
```

# 基础知识

```python
print(x,y,z);	#一次输入多个值
AGE	= 20;	#常量
** #次幂运算
name = input("请输入你的名字:")	#用户交互,返回的都是str类型
```

# 数据类型

```python
type()
int	123
str	"123"	#字符串可以相乘	print("x"*3)	xxx
bool True	False
tup1 = (1,2,3,4,5);	#元祖，只读
list = [10,20,30];
集合 = {1,2,3,4,"sss"}
num = '12';	num = int(num);	#类型转换，str(x)
#与java不同字符串不能喝数值型相加
#字符串模板
msg = "我叫%s,今年%d,学习进度3%%" %(name,age) print(msg);	或	print("我叫"+str(name))
%%转意
//整除，int型整除
非0转换为bool是True,0是False
print(1 or 2)	#1
print(0 or 2)	#2
print(1 and 2)	#2
print(0 and 2)	#0

0 #False
1 #True
""	#False
"0"	#True
```

# 条件选择

```python
if 条件：		#如果结果只有1行可以写成	if 条件：结果
	结果
elif 条件:
	结果
else:
	结果
and,or,not	#&&,||
```

# 循环

```python
while True:		#while 1效率高于while True
    操作
    break;	#终端循环
    continue；
else:
    print("循环正常执行完了");		#当循环执行完毕后才会执行该else
    
for i in s:
    操作
```

 # 字符串操作

```bash
str = "ABCDEFG";
strIndex0 = str[0];	#A
str[-1];		#G
str[0:2]	#ABC
s.capitalize(); #首字母大写
s.upper();  #全大写
s.lower();  #全小写
s.startswith("ABC");
s.endswith("F");
s.find("A");	#返回索引
s.strip(" ");   #删除输入的元素
s.count("A");	#查询个数
s.replace("A","X",1)	#替换几次字符
s.isalnum() #字符串和数字混合
s.isalpha() #字符串
s.isdigit() #数字
s = "我叫{name},今年{age}".format(name="王玉强",age=16);
```

