# mysql

## mysql启动失败

1. 删除data文件夹

2. 修改my.ini

   ```sh
   basedir ="C:\RUIHUA\env\mysql\5.7.31.2"   # 设置mysql的安装目录 
   datadir ="C:\RUIHUA\env\mysql\5.7.31.2\data"   # 设置mysql数据库的数据的存放目录，必须是data，或者是//xxx/data  
   ```

3. 管理员身份模式进入mysql下的bin目录

4. 移除已注册服务`mysqld remove`

5. 初始化 `mysqld --initialize --console`

   - root@localhost: [ 这里是密码 ]

6. 注册服务: `mysqld -install`

7. 启动服务: `net start mysql`

8. 启动: `mysql -u root -p`

   1. 修改密码

      MySQL8.0.4以前，执行

      ```
      SET PASSWORD=PASSWORD('[修改的密码]');
      
      mysql8.0.4以后
      
      ALTER` `USER` `'root'``@``'localhost'` `IDENTIFIED ``WITH` `mysql_native_password ``BY` `'修改的密码'``;
      ```

目前密码: `ruihuag`
