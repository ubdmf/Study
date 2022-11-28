# Linux 指令

- cat minidump_ehy.txt  查看txt文件
- history 查看历史命令
-  终端输出的文件保存到 指定的txt
    - `./minidump_stackwalk_proxy ~/ehy_app.1664078962.ee5cba2.- 1deb537a-3e66-465f-a532-5bd611fdbb46.soc1.boot00000948.dmp ~/symbols >> ~/minidump_ehy.txt`
- ps -ef |grep dds
- ls -ld 查看文件夹权限
- ls -la 查看隐藏文件
- touch *.txt 创建文本
- rm -rf *xxx* 删除所有含xxx的关键字
- 利用echo 写入内容
 -  echo xxx > test.txt
 - chmod u+x test-json.sh    

## SSH
- ssh-keygen   创建一对密钥对
- ssh-keygen -y -f id_rsa_adas_04 可以查看指定的密钥对应的公钥

## pip
- pip show adwsdk 可以查看pip安装的位置

