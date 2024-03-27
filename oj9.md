# oj赛第九周|MT3011骑士锦标赛、2.序列加法、3.剪纸、4.构造原理、5.小码妹排序

### 1. MT3011 骑士锦标赛

##### 题目链接：<https://www.matiji.net/exam/dohomework/5983/1>

* 新一届的卡西米尔骑士锦标赛又开始了，本次比赛有 n位骑士参加，为了保证公平，采用循环赛的赛制，每人每天进行一场比赛。每位骑士都会进行 n-1 场比赛，他们每人都可以为自己未来的对手安排一个挑战的顺序，所有人的要求都会被满足。请你计算最少的比赛天数是多少，若无法满足所有人的要求，输出-1。

---

##### 代码描述

``` python3

def main():
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

##### 补充知识点
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
	for i in range(n):  # 记录比赛要求
    	line_1[i] = [j+1 for j in line_1[i]]     # 将数组中的每一行化为整型
	print(line_1)
	print(line)
	```
	


