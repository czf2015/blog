scp是linux常用的命令，它可以方便的进行文件的传输。
利用scp进行文件传输时，通过指定的加密算法还可以提升传输速度。

1. 什么是scp?
scp（Secure Copy）允许不同的主机之间进行文件传输。scp使用ssh进行数据的传输，提供了和ssh相同的身份认证和同一级别的安全策略。

2. 从本地到远程
# 从本地复制单个文件
scp pet_local.txt root@192.168.10.174:/root
 
# 从本地复制多个文件
scp foo.txt bar.txt your_username@remotehost.edu:~
 
# 从本地复制文件，指定端口，22为默认端口
scp -P 22 pet_local.txt root@192.168.10.174:/root
 
# 从本地复制文件夹，需要加参数-r
scp -r Music root@192.168.10.174:/root

3. 从远程到本地
# 从远程复制单个文件
scp root@192.168.10.174:/root/pet.txt .
 
# 从远程复制多个文件
scp root@192.168.10.174:/root/\{pet.txt,pet1.txt\} .
 
# 从远程复制文件，指定端口，22为默认端口
scp -P 22 root@192.168.10.174:/root/pet.txt .
 
# 从远程复制文件夹，需要加参数-r
scp -r root@192.168.10.174:/root/soft .

4. 从远程到远程
# 从远程复制文件到远程
scp -3 root@192.168.10.174:/root/pet.txt root@192.168.10.177:/root

5. scp性能
# scp默认的情况下使用Triple-DES加密算法来加密发送的数据，如果想提升传输的速度，你可以使用Blowfish加密算法，命令行输入-c blowfish即可。
scp -c blowfish pet_local.txt root@192.168.10.177:/root
 
# 通过使用压缩命令，可以显著的提高速度，但是会增加CPU的负荷，命令输入-C即可。
scp -c blowfish -C pet_local.txt root@192.168.10.177:/root
 
# 这里我用了390M的文件进行传输的测试, 通过比较使用压缩后的命令会传输的快些。
scp -c blowfish scp_390M.dmg root@192.168.10.177:/root
>> scp_390M.dmg             100%  390MB   4.6MB/s   01:24
 
scp -c blowfish -C scp_390M.dmg root@192.168.10.177:/root
>> scp_390M.dmg             100%  390MB   5.7MB/s   01:09

————————————————
版权声明：本文为CSDN博主「开心的冰屋」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/libingxin/article/details/51120021