# oj赛第九周|MT3011骑士锦标赛、MT3012序列加法、3.剪纸、MT3015构造原理、5.小码妹排序

### 1. MT3011 骑士锦标赛

##### 题目链接：<https://www.matiji.net/exam/dohomework/5983/1>

* 新一届的卡西米尔骑士锦标赛又开始了，本次比赛有 n位骑士参加，为了保证公平，采用循环赛的赛制，每人每天进行一场比赛。每位骑士都会进行 n-1 场比赛，他们每人都可以为自己未来的对手安排一个挑战的顺序，所有人的要求都会被满足。请你计算最少的比赛天数是多少，若无法满足所有人的要求，输出-1。

---

##### 代码描述

``` python3

def main():
	''' 贪心算法
	核心逻辑主要在于确保每个人在一天内只参与一场挑战，并且只有当双方互为对手时才能进行挑战。
	'''
    n = int(input())    # 输入人数n
    line = [[0 for _ in range(n-1)] for _ in range(n)]    # 初始化二维数组
    for i in range(n):  # 记录比赛要求
        line[i] = input().split(" ")    # 同行数字用空格隔开
        line[i] = [int(j) for j in line[i]]     # 将数组中的每一行化为整型
    
    # op = [0 for  _ in range(n)]     # 记录每个人当天是否战斗
    days = 0     # 记录战斗天数
    while True:
        op = [0 for  _ in range(n)]     # 记录每个人当天是否战斗
        flag = 0    # 今天是否有战斗
        for i in range(n):
            if (op[i] == 0 and len(line[i]) > 0):      # 第i人今天没有进行挑战且挑战未完成
                riv = line[i][0] - 1   # 记录他的对手riv
                if (op[riv] == 0 and len(line[riv]) > 0 and line[riv][0] == i + 1 ):       # riv今天没有进行挑战且挑战未完成且对手是i
                    op[i] = 1
                    op[riv] = 1
                    del line[i][0]
                    del line[riv][0]
                    flag = 1
        if flag:
            days += 1
        else:
            break
    
    # 检查是否还有未完成的比赛
    for i in range(n):
        if len(line[i]) > 0:
            print("-1")
            return
    print(days)  # 所有挑战都完成了

if __name__ == '__main__':
    main();
```

---

##### 补充知识及注意点
1. 二维数组的构造方式
	1. 浅拷贝与深拷贝
		* 直接赋值：是对对象的引用（别名）
		* 浅拷贝(copy): 拷贝父对象，不会拷贝对象的内部的子对象
		* 深拷贝(deepcopy)： copy 模块的 deepcopy 方法，完全拷贝了父对象及其子对象。
	2. 区别
		* 直接引用时，两个对象修改会保持一致；浅拷贝时，修改最外层的dict可以，但是对某个key(如果是子对象，例如list或者dict)添加或去除元素时会保持一致；深拷贝时，可以任意修改。
		* `[0] * n`是把列表重复n次，是`=`号复制（不是浅拷贝，=与浅拷贝的list id不同）。`[[0] * n] * m` 是将`[0] * n`复制了m遍
		* `[0 for _ in range(n)]`是创建，是深拷贝
	3. 二维数组的三种创建方式
	```python3
	m,n = 3,4
	dp1 = [[0] * n ] * m
	dp2 = [[0 for _ in range(n) ] for _ in range(m)]
	dp3 = [[0] * n for _ in range(m)]
	dp1[0][2] = 3
	dp2[0][2] = 3
	dp3[0][2] = 3
	print('dp1:',dp1)
	print('dp2:',dp2)
	print('dp2:',dp3)
	```
	结果为：
	```python3
	dp1: [[0, 0, 3, 0], [0, 0, 3, 0], [0, 0, 3, 0]]
	dp2: [[0, 0, 3, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
	dp2: [[0, 0, 3, 0], [0, 0, 0, 0], [0, 0, 0, 0]]
	```
	
	* 第一种方法不行，每一行的改变都会改变其他行;第二种、第三种方法均可
	4. 实战中的问题
	```python3
	line_1 = [[2, 3], [1, 3], [1, 2]]
	print(line_1)
	line = line_1
 	print(line)
	for i in range(n):  # 记录比赛要求
    		line_1[i] = [j+1 for j in line_1[i]]     # 将数组中的每一行化为整型
	print(line_1)
	print(line)
	```
	输出为：
	```python3
 	[[2, 3], [1, 3], [1, 2]]
 	[[2, 3], [1, 3], [1, 2]]
	[[3, 4], [2, 4], [2, 3]]
	[[3, 4], [2, 4], [2, 3]]
 	```
 
	* 当你执行了 line = line_1 这行代码时，你并没有创建 line_1 的一个新的拷贝，而是让 line 指向了和 line_1 相同的对象。在 Python 中，变量是对对象的引用。所以，当你修改 line_1 的内容时，因为 line 和 line_1 指向的是同一个对象，你也就看到了 line 的内容随之改变。如果你想要 line 和 line_1 指向不同的对象，你需要创建 line_1 的一个拷贝。对于二维数组（或更深层的嵌套数组），你需要执行一个“深拷贝”（deep copy），因为二维数组是一个由数组组成的数组，仅仅复制最外层是不够的（这称为“浅拷贝”（shallow copy））。在 Python 中，你可以使用 copy 模块的 deepcopy 函数来实现深拷贝：

   ```python3
   import copy
   line = copy.deepcopy(line_1)
   ```

	5. 参考链接：<https://blog.csdn.net/weixin_42042056/article/details/106738462?depth_1->

2. 在创建数组时，需要注意的是input的是字符串，因此需要用到`int()`
	1. 获取整数n时，`n = int(input())`
	2. 获取二维数组时：
        ```python3
        line = [[0 for _ in range(n-1)] for _ in range(n)]    # 初始化二维数组
        for i in range(n):  # 记录比赛要求
        	line[i] = input().split(" ")    # 同行数字用空格隔开
        	line[i] = [int(j) for j in line[i]]     # 将数组中的每一行化为整型【重要！使用列表推导式化为整型】
        ```
        如果没有最后一句，line的结果将是`[['2', '3'], ['1', '3'], ['1', '2']]`，加上最后一句后便是`[[2, 3], [1, 3], [1, 2]]`

3. 写对手riv时注意数组的index和本身骑士个数的区别，涉及到i+1和i-1
4. 严重影响正确率的代码是最后的输出部分。
	* 原错误代码：
	```python3
	flag = 1
	for i in range(n):      # 判断是否所有人都完成了比赛
   		if len(line[i]) > 0:
   		flag = 0
	if flag:
		return days
	else:
		print("-1")
	```

	* 修正后代码：
 
	```python3
	#判断是否所有人都完成了比赛
	for i in range(n):
	if len(line[i]) > 0:
		print("-1")
		return
	print(days)  #所有挑战都完成了
	```

	* 有一说一还是没弄懂之前写的哪里不行

--- 

### 2. MT3012 序列加法

##### 题目链接：<https://www.matiji.net/exam/dohomework/5983/2>

* 小码哥有一个由n个数构成的序列 a1，a2，a3，…，现在他想要对这个序列进行两个操作:1.1xy，把所有下标是x的倍数的数加上y;2.2i，输出 ai 。

--- 

##### 代码描述
```python3
 def operate1(num, n, x, y):
    # 直接计算需要增加y的索引，然后增加y
    for i in range(x - 1, n, x):
    	num[i] += y
    return num

def operate2(num, n, i):
    print(num[i - 1])


def main():
    n, m = input().split()
    n = int(n)
    m = int(m)
    nums = [int(num) for num in input().split()]

    for j in range(m):
        line = [int(num) for num in input().split()]

        if line[0] == 1:
            x, y = line[1], line[2]
            nums = operate1(nums, n, x, y)
        else:
            i = line[1]
            operate2(nums, n, i)


if __name__ == '__main__':
    main();
```

 --- 

 ##### 补充知识及注意点
1. 第一次提交，对于一半的用例代码超时，提交chatgpt修改之后发现是因为operate1函数中的循环效率不高。每次调用operate1时，它都会遍历整个nums列表，这在列表很长且需要多次执行该操作时会非常耗时。改进的方法是减少不必要的遍历，尽可能利用数学方法来优化操作。第一次提交代码时的operate1函数如下
   ```python3
   def operate1(num, n, x, y):
   	for i in range(n):
   	if (i + 1) % x == 0:
   		num[i] += y
   	return num
   ```
   改进后的代码为：
   ```python3
   def operate1(num, n, x, y):
   	for i in range(x - 1, n, y):
   		num[i] += y
   	return num
   ```
   优化后的代码利用了范围和步长直接跳过了不需要修改的元素，大大减少了循环次数。优化前的时间复杂度为O(n)，优化后的为O(n/x)，当x相对于n较大时性能提升更为明显。同时暴露了一个问题是对于range()函数的使用方法不太熟悉。
2. range()函数
   * range()函数用来生成一系列连续的整数，多用于for循环语句中。其语法格式为：`range(start, end, step)`
   * start：起始值，可省略，若省略则从0开始
   * end：结束值，不包括该值，不能省略
   * step：步长，可省略，若省略则为1
   * 如果只有一个参数，那么是range(end)；如果是两个参数，那么是range(start, end)；有三个参数时才算步长。

--- 


### 3. MT3013 剪纸

##### 题目链接：<https://www.matiji.net/exam/dohomework/5983/3>

* 给出一个长方体的长、宽、高(分别是 a,b,c )，现在给你n 张不同大小的长方形纸，问:能在多少张纸上裁出这个长方体的某一种侧面展开图。

--- 

不会

---

#### 4. MT3015 构造原理

##### 题目链接：<https://www.matiji.net/exam/dohomework/5983/4>
* 原子核外电子可分为不同能层，能层可直接用数字1，2，3等表示。同一能层的电子又由于所含能量不同可分为不同能级，能级为 s，p，d，f。能级 s 最多容纳2个电子，p6个，d10个，f14个。第1层的s能级就写成1s。第i个能层的能级数最多为i个。
核外电子不一定是按从小数到大数，从 s 到 的顺序排布的。有构造原理来显示真实的
排布顺序。

--- 

##### 代码描述
``` python3
ele = [2, 6, 10, 14]      # 不同能级电子个数
level = [[0 for _ in range(4)] for _ in range(8)]  # 初始化能层

def calculate(x, i ,j):
    level[i][j] = min(ele[j], x)
    y = x - ele[j]
    if y <= 0:
        return
    if j == 0:      # 能级s
        j = (i + 1) // 2
        i = i // 2 + 1
    else:           # 非能级s
        i += 1
        j -= 1
    calculate(y, i, j)      # 继续递归
    
def main():
    energy = ['s', 'p', 'd', 'f']
    n = int(input())
    calculate(n, 0, 0)

    for i in range(8):
        output_line = ""  # 初始化一个空字符串来收集当前行的输出
        for j in range(4):
            if level[i][j]:  # 如果当前位置有电子
                # 构建当前轨道的字符串表示
                string = str(i+1) + energy[j] + str(level[i][j]) + ' '
                output_line += string  # 将当前轨道的字符串添加到当前行的输出中
        if output_line:  # 如果当前行的输出非空，即有数据需要打印
            print(output_line.strip())  # 打印当前行，并去掉尾部的空格
    

if __name__ == '__main__':
    main();
```

---

##### 补充知识及注意点
1. 写出各能层不同能级的坐标变化顺序，用归纳法得出坐标变化规律
2. 递归算法的使用
3. strip()方法
   * strip()方法用于去除字符串左右两侧的空格和特殊字符，语法格式为`str.strip([chars])`
   * chars不指定时默认去除空格、制表符\t、回车符\r、换行符\n等
