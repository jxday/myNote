# JAVA约定

![image-20200229134934880](/Users/cty/Library/Application Support/typora-user-images/image-20200229134934880.png)



##### 2.[有return的情况下try catch finally的执行顺序（最有说服力的总结）](https://www.cnblogs.com/fery/p/4709841.html)

1、不管有没有出现异常，finally块中代码都会执行；
2、当try和catch中有return时，finally仍然会执行；
3、finally是在return后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来（浅拷贝/值传递），不管finally中的代码怎么样，返回的值都不会改变，任然是之前保存的值），所以函数返回值是在finally执行前确定的；
4、finally中最好不要包含return，否则程序会提前退出，返回值不是try或catch中保存的返回值。