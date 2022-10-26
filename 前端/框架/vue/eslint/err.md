# vue 使用 eslint 报错



## Unexpected tab character(no-tabs)

1. 在创建项目的时候`Use ESlint to lint your code?(Y/n)`这里选no
2. 在`.eslintrc.js`中在`rules`:加入一行`"no-tabs": "off"`
3. 在文件中添加`/* eslint-disable */`

