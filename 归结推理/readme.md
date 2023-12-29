## 根据老师的程序创新要求，我实现了关于谓词逻辑的归结推理（c++实现）

程序主要由两部分组成：

1.处理输入；已知字句与目标命题一同构成字句集

2.迭代归结，最后基于归结推理的判断方法输出TRUE/FALSE

### 输入要求：

example：
```c++
1
fail(Bob,final_exam)
5
thu_student(x) & learning_discrete_mathemetics(x) -> sleep(x,late)
learning_discrete_mathemetics(x) -> weary(x)
sleep(x,late) & sleep(x,late)
thu_student(Bob)
