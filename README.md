怎样在一台机子上设置多个key,并且在github中使用

生成key: 

<code>
ssh-keygen -t -rsa -C 'second@mail.com'
</code>
<code>
ssh-keygen -t rsa -f ~/.ssh/id_rsa.work -C "Key for Work stuff"
</code>

用下面的命令看看有没有新生成的密钥:

<code>
add-ssh -l
</code>

如果没有:

<code>
ssh-add ～/.ssh/id_rsa.github
ssh-add ～/.ssh/id_rsa.work
</code>

接下来
新增一个配置文件,并修改权限:

<code>
touch ~/.ssh/config
chmod 600 ~/.ssh/config
</code>

修改config文件的内容:

<code>
Host github.com
    HostName github.com
    IdentityFile ~/.ssh/id_rsa.github
    User git
</code>
<code>
Host github-work
    HostName github.com
    IdentityFile ~/.ssh/id_rsa.work
    User git
</code>



用下面命令复制一下你的key:
(生成的两个id_rsa.work   and  id_rsa.github)

<code>
xclip -sel clip < ~/.ssh/id_rsa.pub
</code>

将key复制到你的github中ssh中

下一步在你的github里面创建两个项目:
在本地创建一些文件,用id_rsa.work中的key提交

<code>
mkdir work
cd work
git init
touch a.txt
git add a.txt
git commit -m "add a file a.txt"
git remote add origin git@github-work:user_name/repo.git ##并非原来的git@github.com:user_name/repo.git
git push -u origin master
</code>

用id_rsa.github中的key提交:

<code>
mkdir work
cd work
git init
touch a.txt
git add a.txt
git commit -m "add a file a.txt"
git remote add origin git@github.com:user_name/repo.git 
git push -u origin master
</code>