怎样在一台机子上设置多个key,并且在github中使用
<br>
生成key: 
<br>
<code>
ssh-keygen -t -rsa -C 'second@mail.com'
</code><br>
<code>
ssh-keygen -t rsa -f ~/.ssh/id_rsa.work -C "Key for Work stuff"
</code>
<br>
用下面的命令看看有没有新生成的密钥:
<br>
<code>
add-ssh -l
</code>
<br>
如果没有:
<br>
<code>
ssh-add ～/.ssh/id_rsa.github
</code><br><code>ssh-add ～/.ssh/id_rsa.work
</code>
<br>
接下来
新增一个配置文件,并修改权限:
<br>
<code>
touch ~/.ssh/config
</code><br><code>chmod 600 ~/.ssh/config
</code>
<br>
修改config文件的内容:
<br>
<code>
Host github.com
</code><br><code>    HostName github.com
</code><br><code>    IdentityFile ~/.ssh/id_rsa.github
</code><br><code>    User git
</code>
<br>
<code>
Host github-work
</code><br><code>    HostName github.com
</code><br><code>    IdentityFile ~/.ssh/id_rsa.work
</code><br><code>    User git
</code>

<br>

用下面命令复制一下你的key:
(生成的两个id_rsa.work   and  id_rsa.github)
<br>
<code>
xclip -sel clip < ~/.ssh/id_rsa.pub
</code>
<br>
将key复制到你的github中ssh中
<br>
下一步在你的github里面创建两个项目:
在本地创建一些文件,用id_rsa.work中的key提交
<br>
<code>
mkdir work</code><br>
<code>cd work</code><br>
<code>git init</code><br>
<code>touch a.txt</code><br>
<code>git add a.txt</code><br>
<code>git commit -m "add a file a.txt"</code><br>
<code>git remote add origin git@github-work:user_name/repo.git</code> ##并非原来的git@github.com:user_name/repo.git</code><br>
<code>git push -u origin master</code>
<br>
用id_rsa.github中的key提交:
<br>
<code>
mkdir work
</code><br><code>cd work
</code><br><code>git init
</code><br><code>touch a.txt
</code><br><code>git add a.txt
</code><br><code>git commit -m "add a file a.txt"
</code><br><code>git remote add origin git@github.com:user_name/repo.git 
</code><br><code>git push -u origin master
</code><br>


fatal: remote error: You can't push to git 解决办法
<br>
<code>
fatal: remote error: 
</code><br><code>  You can't push to git://github.com/user_name/user_repo.git
</code><br><code>  Use git@github.com:user_name/user_repo.git
</code><br>
  解决办法：<br>
<code>
$ git remote rm origin
</code><br><code>$ git remote add origin git@github.com:user_name/user_repo.git
</code><br><code>$ git push origin
</code><br>
如果在git clone的时候用的是git://github.com:xx/xxx.git 的形式, 那么就会出现这个问题，因为这个protocol是不支持push的