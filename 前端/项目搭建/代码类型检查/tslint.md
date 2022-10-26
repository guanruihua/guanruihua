# TSLint

> 已经弃用, 建议使用eslint

## 准备工作

```shell
npm uninstall -g typescript
npm install -g typescript tslint
tslint --init
tslint --project
```

## 配置

```js
{
 defaultSeverity: error,
 extends: [
  tslint:recommended
 ],
 rules: {
  member-access: true, // 设置成员对象的访问权限&#xff08;public,private,protect)
  member-ordering: [// 设置修饰符顺序
    true,
    {
      order: [ 
        public-static-field,
        public-static-method,
        protected-static-field,
        protected-static-method,
        private-static-field,
        private-static-method,
        public-instance-field,
        protected-instance-field,
        private-instance-field,
        public-constructor,
        protected-constructor,
        private-constructor,
        public-instance-method,
        protected-instance-method,
        private-instance-method
      ]
    }
  ],
        // no-empty-interface:true,// 不允许空接口
        no-parameter-reassignment:false,// 不允许修改方法输入参数
        prefer-for-of:true,// 如果for循环中没有使用索引&#xff0c;建议是使用for-of

        // 功能特性
        no-namespace:false,
        only-arrow-functions:false, //禁止使用传统&#xff08;非箭头&#xff09;函数表达式
        no-shadowed-variable: true, // 不允许子作用域与外层作用域声明同名变量
        no-string-literal:false,
        ban-types: false,// 禁止内置原始类型
        await-promise:true,// 不允许没有Promise的情况下使用await
        curly:true,// if/for/do/while强制使用大括号
        forin:false,// 使用for in语句时&#xff0c;强制进行hasOwnProperty检查
        no-arg:true,// 不允许使用arguments.callee
        no-bitwise:false, // 不允许使用特殊运算符 &amp;, &amp;&#61;, |, |&#61;, ^, ^&#61;, &lt;&lt;, &lt;&lt;&#61;, &gt;&gt;, &gt;&gt;&#61;, &gt;&gt;&gt;, &gt;&gt;&gt;&#61;, ~
        no-conditional-assignment:true,// do while/for/if/while 语句中将会对例如if(a&#61;b)进行检查
        no-console:true,// 不允许使用console对象
        no-debugger:true,// 不允许使用debugger
        no-duplicate-super:true,// 不允许super() 两次使用在构造函数中
        no-empty:false,// 函数体不允许空
        no-eval:true,// 不允许使用eval
        no-for-in-array:true,// 不允许对Array使用for-in
        no-invalid-template-strings:true,// 只允许在模板字符串中使用${
        // no-invalid-this:true,// 不允许在class之外使用this
        // no-null-keyword:true,// 不允许使用null,使用undefined代替null&#xff0c;指代空指针对象
        no-sparse-arrays:true,// 不允许array中有空元素
        no-string-throw:true,// 不允许throw一个字符串
        no-switch-case-fall-through:true,// 不允许case段落中在没有使用breack的情况下&#xff0c;在新启一段case逻辑
        no-unsafe-finally:true,// 不允许在finally语句中使用return/continue/break/throw
        no-unused-expression:true,// 不允许使用未使用的表达式
        no-use-before-declare:true,// 在使用前必须声明
        no-var-keyword:true,// 不允许使用var
        radix:false,// parseInt时&#xff0c;必须输入radix精度参数
        // restrict-plus-operands:true,// 不允许自动类型转换&#xff0c;如果已设置不允许使用关键字var该设置无效
        triple-equals:false,// 必须使用恒等号&#xff0c;进行等于比较
        use-isnan:true,// 只允许使用isNaN方法检查数字是否有效

        // 维护性功能
        indent:[true, spaces, 4],// 每行开始以4个空格符开始
        max-classes-per-file:[true,1],// 每个文件中可定义类的个数
        max-file-line-count:[true,1000],// 定义每个文件代码行数
        max-line-length:[true,300],// 定义每行代码数
        no-default-export:true,// 禁止使用export default关键字&#xff0c;因为当export对象名称发生变化时&#xff0c;需要修改import中的对象名。https://github.com/palantir/tslint/issues/1182#issue-151780453
        no-duplicate-imports:true,// 禁止在一个文件内&#xff0c;多次引用同一module

        // 格式
        align:[true,parameters,arguments,statements,members,elements],// 定义对齐风格
        array-type:[true,array],// 建议使用T[]方式声明一个数组对象
        class-name:false,// 类名以大驼峰格式命名
        comment-format:[true, check-space],// 定义注释格式
        encoding:false,// 定义编码格式默认utf-8
        import-spacing:true,// import关键字后加空格
        interface-name:[true,always-prefix],// interface必须以I开头
        jsdoc-format:false,// 注释基于jsdoc风格
        new-parens:true,// 调用构造函数时需要用括号
        object-literal-sort-keys:false,
        no-consecutive-blank-lines:[true,2],// 不允许有空行
        // no-trailing-whitespace: [// 不允许空格结尾
        //     true,
        //     ignore-comments,
        //     ignore-jsdoc
        // ],
        no-unnecessary-initializer:true,// 不允许没有必要的初始化
  variable-name:[
     false, check-format,// 定义变量命名规则
  allow-leading-underscore,
  allow-trailing-underscore,
  ban-keywords
      ]
    },
 rulesDirectory: [],
 linterOptions: {
  exclude: [
   e2e/**/*
  ]
 }
}
```
