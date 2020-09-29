# swu-sign

本项目主要是借助子墨大佬的自动签到脚本[项目指路](https://github.com/ZimoLoveShuang/auto-submit)，并对此进行了修改，能够服务西大学子自动完成今日校园的签到。实现方式更加简洁明确，针对性较强。

# 设计思路

1. 模拟登陆
2. 获取每日未签到任务
3. 获取未签到任务详情
4. 根据配置，自动填写表单
5. 提交未签到任务

# 项目说明

- `config.yml` 默认配置文件
- `index.py` 完成自动提交的py脚本
- `dependency.zip` 项目需要用到的依赖项，在上传腾讯云的时候需要，同时如果你需要在本地先进行测试的时候也可以直接导入，也可以在线现在用到的依赖包。
- 配置经纬度信息，可以访问[这里](http://zuobiao.ay800.com/s/27/index.php)，将经纬度四舍五入保留六位小数之后的放入配置文件对应位置即可。（文件中默认的为西大指定的签到地址对应的坐标）



# 使用

#### 如果一天签到多次，除了问题不一样之外，其他都一样，你又不想配置多个云函数的话，配置文件设置不检查就行了

## 修改基本信息

1. clone 或者 下载 此仓库到本地

2. 在`config.yml`中填好用户名（学号或者工号），密码，学校等.

3. 执行（并非必要的，**可以直接跳转到云端系统的配置步骤**）

   ```
   index.py
   ```

   ，会报错，因为你只填了学校信息，不用管他，然后你会得到类似下面这样的输出

   ```
   {'login-url': 'http://authserverxg.swu.edu.cn/authserver/login?service=https%3A%2F%2Fswu.cpdaily.com%2Fportal%2Flogin', 'host': 'swu.cpdaily.com'}
   ```

   从这个里面拿出host的值

   ```
   swu.cpdaily.com
   ```

4. 打开浏览器（最好是chrome内核的浏览器），输入上面拿到的host的值，按enter

5. 尝试使用用户名（学号或者工号）密码登录，如果登录成功，代表没有被禁用.

   ！[打开成功](https://github.com/TheBigCock/suw_sign/blob/master/image/%E6%88%90%E5%8A%9F%E8%AE%BF%E9%97%AE.png)

### 云端系统可用（配合腾讯云函数）

1. 打开本地仓库文件夹，配置`config.yml`中对应的学号（username）和密码（password）还有地址（address）等等信息，详情请看`config.yml`中的注释说明，**注意这里的学号和密码都是智慧校园的学号和密码**

2. 打开百度搜索[腾讯云函数](https://console.cloud.tencent.com/scf/index?rid=1)，注册认证后，进入控制台，然后搜索云函数。

   ![云函数搜索](https://github.com/TheBigCock/suw_sign/blob/master/image/%E4%BA%91%E5%87%BD%E6%95%B0%E6%90%9C%E7%B4%A2.png)

3. 点进去之后再**点击左边的层**，然后点新建，名称随意，然后点击上传zip，选择release中的`dependency.zip`上传，然后选择运行环境`python3.6`，然后点击确定。![新建腾讯云函数依赖](https://github.com/TheBigCock/suw_sign/blob/master/image/%E5%B1%82%E7%9A%84%E8%AE%BE%E7%BD%AE.png)

4. 点左边的函数服务，新建云函数，名称随意，运行环境选择`python3.6`，创建方式选择空白函数，然后点击下一步 [![新建腾讯云函数](https://github.com/ZimoLoveShuang/auto-submit/raw/master/screenshots/a971478e.png)](https://github.com/ZimoLoveShuang/auto-submit/blob/master/screenshots/a971478e.png)

5. 提交方法选择在线编辑，把本地修改好的`index.py`直接全文复制粘贴到云函数的`index.py`，然后点击文件->新建，文件名命名为`config.yml`，然后把本地配置好的`config.yml`文件中的内容直接全文复制粘贴到云函数的`config.yml`文件，**点击下面的高级设置**，设置超时时间为`60秒`，添加层为刚刚新建的函数依赖层，然后点击完成 。

   ![1601387989141](https://github.com/TheBigCock/suw_sign/blob/master/image/%E4%BA%91%E5%87%BD%E6%95%B0%E7%95%8C%E9%9D%A2.png)

6. 进入新建好的云函数，左边点击触发管理，点击创建触发器，名称随意，触发周期选择自定义，然后配置cron表达式，下面的表达式表示每天晚上9点整执行。（设置早上签到就修改为9）

   ```
   0 0 21 * * * *
   ```

7. 然后就可以测试云函数了，绿色代表云函数执行成功，红色代表云函数执行失败（失败的原因大部分是由于依赖造成的）。返回结果是`success.`，代表自动提交成功，如遇到问题，请仔细查看日志.

   ![1601388490273](https://github.com/TheBigCock/suw_sign/blob/master/image/%E7%BB%93%E6%9E%9C.png) 

8. enjoy it!
