# SQLAlchemy：ObjectNotFound


1. 遇到异常
	pytest的异常内容
	![pytest异常图片](../../asserts/images/2024-03-22_0/1.jpg)
	异常所在的测试代码片段
	![bug所在测试代码片段](../../asserts/images/2024-03-22_0/2.jpg)

2. 异常所测代码，添加number的hex显示
	![bug所测试Repo类](../../asserts/images/2024-03-22_0/3.jpg)
	测试结果
	![第1次修改后测试](../../asserts/images/2024-03-22_0/4.jpg)
	发现没有办法和pycharm中db的数据进行直接比对
	![第1次修改后查看数据库](../../asserts/images/2024-03-22_0/5.jpg)

3. 删除example.db，清空数据库，重新进行测试
	uw文件
	![只运行test_uw文件](../../asserts/images/2024-03-22_0/6.jpg)
	测试
	![第2次修改测试](../../asserts/images/2024-03-22_0/7.jpg)
	发现对不上，但是不清楚是不是sqlite存储数据的显示问题还是别的什么
	![第2次修改后查看数据库](../../asserts/images/2024-03-22_0/8.jpg)

4. 使用底层语句进行测试
	![第3次修改](../../asserts/images/2024-03-22_0/9.jpg)
	底层语句的校验没有问题，确实可以找到对象，但是tu对象的number和查询的number无法对上
	![第3次修改测试结果](../../asserts/images/2024-03-22_0/10.jpg)

5. 只运行之前的测试
	![第4次修改结果](../../asserts/images/2024-03-22_0/11.jpg)

6. 察觉到uw的session获取方式是和之前测试中生成的方式不一样
	![第5次怀疑](../../asserts/images/2024-03-22_0/12.jpg)
	但是修改后，pytest报错没有任何改变

7. 将之前的测试一部分迁移过来
	![第6次修改](../../asserts/images/2024-03-22_0/13.jpg)
	测试居然通过了
	![第6次修改的结果](../../asserts/images/2024-03-22_0/14.jpg)

8. 将迁移过来的注释掉
	![第7次修改](../../asserts/images/2024-03-22_0/15.jpg)

9. 删除掉迁移代码，并且只保留tutorial
	![第8次修改只保留tutorial](../../asserts/images/2024-03-22_0/16.jpg)

10. 找到bug源头
	源头
	![第9次修改找到bug源头](../../asserts/images/2024-03-22_0/17.jpg)
	修改
	![第8次修改后](../../asserts/images/2024-03-22_0/18.jpg)
	测试
	![第8次修改后测试](../../asserts/images/2024-03-22_0/19.jpg)


## 总结

1. 没有看清楚，ObjectNotFound对应的语句是
   ```python
   assert di == uw.tutorial.get(di.number)
   ```

2. 没有一开始简化测试，限定其能够出bug的范围，让一开始的搜索范围就很大。在这个bug中体现的是， 一开始就测试了tutorial、dialogue和practice。过大的测试范围，让错误更加难找

3. 以后要注意这种复制粘贴的测试代码。特别是，那些排列整齐的部分，因为测试不应该有太多的相似之处

   如果不是太长，可以手敲，手敲的时候会应该会比复制粘贴过更多脑子

   ```python
   assert tu == uw.tutorial.get(tu.number)
   assert di == uw.tutorial.get(di.number)
   assert pr == uw.tutorial.get(pr.number)
   ```

   
