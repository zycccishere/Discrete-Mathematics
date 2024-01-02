## 根据老师的程序创新要求，我实现了包含谓词逻辑的推理(利用了归结)（c++实现）

程序主要由两部分组成：

1.处理输入；已知字句与建立字句集

2.迭代归结，对字句集进行扩充

3.将目标命题与字句集进行比较，判断true/false

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

## 待改进：
1.未引入量词，而是常量形式带入，所以未有化简为skolemn标准型的过程

## 特性：
1.为了实现对多个命题的判断，我没有直接将结论的逆命题加入字句集，而是先进行扩充最后进行判断，这样在同时对多个结论的判断时更有优势
