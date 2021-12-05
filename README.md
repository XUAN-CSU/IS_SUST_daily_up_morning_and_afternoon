# 陕西科技大学_IS_SUST
陕西科技大学早午检打卡脚本

#目的：

  希望大家只是用来代替人工打卡，能够专心于科研，不用每天早午两次执行中断去打卡，浪费大脑的中断，

切不可：

  欺骗学校，谎报位置，谎报自身情况
 
感谢：
 
  感谢984.9学校(祝愿早日升为985)：西安电子科技大学的各位同学的代码参考
  
  特别感谢 https://github.com/cunzao/ncov 同学的代码,祝这位同学学习顺利，身体健康
  
  还要感谢 python3 以及 request库的开发者 https://github.com/psf/requests
  
  最后感谢陕西科技大学福慧图书馆303某位女同学，如果不是周五(20211203)早上你经过时，我被你吸引了，看出神了，但是大脑中提醒打卡的中断打断了我的美好想象，从而我有点厌恶这每天的打卡造成的思考的中断，这才给我力量下定决心去写这个打卡的脚本，祝你和你的小伙伴考研顺利，都能考上心仪大学的研究生
  
20211205211204:The girl is just stangding opposite to me 6 meters away,i just want to share the good feeling with the whole world!
20211205211717:Let's say: Good luck to her and her little friend!

说明：

  我的脚本是挂在我的阿里云的ECS服务器上运行 .sh文件 shell脚本的，如果没有的话可以像我提到的那位cunzao同学 https://github.com/cunzao/ncov 的运行方式一样在腾讯云免费运行函数
 
创新点：

  1.利用阿里云服务器的24小时运行的高稳定性，每次只执行一次提交，不利用python3的time库的time.sleep(30),来占用机器以便重复提交
  
  2.利用Centos系统中crontab,计划任务，设定提交时间
  
  3.利用Centos系统中的mail命令，当提交失败时，利用qq邮箱有的SMTP功能，发送邮件报告提交失败情况，提醒手动打卡，这里是通过微信绑定邮箱，接受邮件，建议创建新的QQ小号，申请邮箱，从而不会有垃圾邮件，从而不会打扰微信接受消息的正常情况
    
    3.1可以参考mail命令
    
    echo "Hello the girl" | mail -s "Love Letter" 520303@qq.com  
  
    3.2可以参考 https://segmentfault.com/a/1190000015143877
      
      进行mail命令的配置
 
 
详细用法：

  把整体文件夹上传到阿里云的的/root/下，并且更换文件夹的名字为IS_SUST，

  在自己的阿里云主机上/root/IS_SUST/data/下的config.json中添加自己的登录信息，
  
  复制微信企业号中的填报信息的界面的网址到chrome浏览器，如果以前没复制过的话是会跳转到登录界面，如果以前PC端填报过的话，可以删除相关cookies
  
  默认是https://app.sust.edu.cn/site/ncov/dailyup 会自动跳转到登录界面
  
  在chrome中是使用F12进行调试，选择上方的Network进行跟踪
  
  输入帐号和密码，第一次不要输对，便于跟踪Network动向，登录时留意调试界面的报告，注意查看关键信息，Request URL, Request Headers栏下的关键信息
  
  在自己的阿里云下的/root/IS_SUST/utils/下的User.py文件中修改自己登录时的主机信息，便于拿到cookie,
  
  然后等某次到打卡时间，但是还没打卡的时候，登录PC端，从登录界面进去，填好信息，F12进入调试模式，点击提交时，注意留意调试界面，Request URL,Request Headers,以及最重要的是向某个地址提交的表单信息
  
  在自己的阿里去下的/root/IS_SUST/utils/下的Utils.py文件中修改提交时的主机信息，向哪个地址提交表单，修改表单，注意格式和\"的应用，可以参照我的Utils.py
  
  在计划任务中，我的阿里云是装的Centos7.9 everything 版本，故可以直接使用命令行
  
  crontab -e #进行修改计划任务
  
  添加执行/root/IS_SUST/shell_command/下的IS_SUST_daily_up_morning_and_afternoon.sh shell脚本的命令
  
  大致是这样的：
  
  25 7 * * * /root/IS_SUST/shell_command/IS_SUST_daily_morning_and_afternoon_clock_in.sh
  
  5 12 * * * /root/IS_SUST/shell_command/IS_SUST_daily_morning_and_afternoon_clock_in.sh
  
  
  crontab -l 是查看任务计划
  

202112051736:just go for dinner,and i will use the Chinese to explain the porject.

202112051854：吃完饭回来，终于用上中文了，还是王永民的王码，哈哈

先总体讲一下这个脚本的作用：

1.通过PC端模拟手机登录，拿到cookies,

2.利用某次正常提交表单时，跟踪到表单提交的网址，以及相关表单

3.执行脚本，模拟登录以及每天的提交信息上报

相关表单的意思如下：

sfzx    是否在校

tw      体温

area

city

province

address

geo_api_info 位置信息情况 注意这一项一定要和提交时用F12调试模式下拿到的geo_api_info项一样，并且注意Utils.py中修改相关wlc_upload_msg中的信息的格式，特别是\\"的应用

sfcyglq 是否处于隔离期

sfyzz 是否有症状

qtqk 其他情况

ymtys 这一项我也没搞懂



  
