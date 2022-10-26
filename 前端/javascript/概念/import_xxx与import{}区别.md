# import_xxx与import\{\}区别

tags:

- import用法
- front_end
- es6

> ES6中export与export default均可用于导出常量、函数、文件、模块等，你可以在其它文件或模块中通过import的两种方式对其进行导入。一个模块只能有一个export default,但可以有若干个export。 区别就在下面。
>
> 1. export与export default均可用于导出常量、函数、文件、模块等
> 2. 在一个文件或模块中，export 、import可以有多个，export default仅有一个
> 3. 通过export方式导出，**在导入时要加export default则不需要**
> 4. export能直接导出变量表达式，export default不行
