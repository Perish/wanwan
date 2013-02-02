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
</code><code>ssh-add ～/.ssh/id_rsa.work
</code>

接下来
新增一个配置文件,并修改权限:

<code>
touch ~/.ssh/config
</code><code>chmod 600 ~/.ssh/config
</code>

修改config文件的内容:

<code>
Host github.com
</code><code>    HostName github.com
</code><code>    IdentityFile ~/.ssh/id_rsa.github
</code><code>    User git
</code>

<code>
Host github-work
</code><code>    HostName github.com
</code><code>    IdentityFile ~/.ssh/id_rsa.work
</code><code>    User git
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
mkdir work</code>
<code>cd work</code>
<code>git init</code>
<code>touch a.txt</code>
<code>git add a.txt</code>
<code>git commit -m "add a file a.txt"</code>
<code>git remote add origin git@github-work:user_name/repo.git</code> ##并非原来的git@github.com:user_name/repo.git
<code>git push -u origin master</code>
</code>

用id_rsa.github中的key提交:

<code>
mkdir work
</code><code>cd work
</code><code>git init
</code><code>touch a.txt
</code><code>git add a.txt
</code><code>git commit -m "add a file a.txt"
</code><code>git remote add origin git@github.com:user_name/repo.git 
</code><code>git push -u origin master
</code>


fatal: remote error: You can't push to git 解决办法

<code>
fatal: remote error: 
</code><code>  You can't push to git://github.com/user_name/user_repo.git
</code><code>  Use git@github.com:user_name/user_repo.git
</code>
  解决办法：
<code>
$ git remote rm origin
</code><code>$ git remote add origin git@github.com:user_name/user_repo.git
</code><code>$ git push origin
</code>
如果在git clone的时候用的是git://github.com:xx/xxx.git 的形式, 那么就会出现这个问题，因为这个protocol是不支持push的