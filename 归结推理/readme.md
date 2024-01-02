## 根据老师的程序创新要求，我实现了谓词逻辑的归结推理（c++实现）

程序主要由两部分组成：

1.处理输入；已知字句与目标命题一同构成字句集

2.迭代归结，最后基于归结推理的判断方法输出TRUE/FALSE

### 输入要求：

example：
```
1                              //目标命题的个数
fail(Bob,final_exam)
5                              //已知字句个数
thu_student(x) & learning_discrete_mathemetics(x) -> sleep(x,late)
learning_discrete_mathemetics(x) -> weary(x)
sleep(x,late) & weary(x) -> fail(x,final_exam)
thu_student(Bob)
learning_discrete_mathemetics(Bob)
```
output:
```
TRUE
```
