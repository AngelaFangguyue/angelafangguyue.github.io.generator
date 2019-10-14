---
title: "如何使用hugo搭建属于自己的博客"
date: 2019-10-12T16:54:04+08:00
draft: false
---
# 搭建一个属于自己的博客。
## hugo:一个使用go语言写的博客生成器，速度快！
## 官方网站的使用推荐说明：https://gohugo.io/getting-started/quick-start/
下面是我自己动手搭建的总结，也是按照官网安装教程操作的。有一些操作是需要在github上操作，如有什么不清楚的，可以去github上搜索。首先你需要有github账号，下面还需要用到github的名字，这个只要你有github账号，自行去github上查看就可以了。如果是windows系统的电脑，电脑上还需要先安装Git Bash或者Cmder，请自行搜索安装教程。我的是windows10，安装的Cmder。（第一步到第十步，我在后面有具体的操作说明）
### 第一步:在自己电脑上安装hugo，【这一步可以通过使用Cmder，操作命令行去下载执行安装，也可以直接去网站手动下载执行安装】，windows系统的还要去配置系统环境变量Path。
### 第二步:在自己电脑上选择一个合适位置，使用工具Cmder或者Git Bash打开，输入命令hugo new site quickstart。*注意这里的quickstart就是要创建的存放博客文件夹的目录名字，建议将该文件夹的名字起为"你的github名.github.io.creator"或者"你的github名.github.io.generator"*【文件名前面"你的github名字.github.io"这些不要写错，后面的可以随便起,如也可以为"你的github名.github.io-creator"或者"你的github名.github.io-generator"等。github名字请自行去github中查看】
### 第三步:输入 cd quickstart（这里的quickstart就是上面第二步创建的文件夹名字）
### 第四步:输入 git init
### 第五步:输入 git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
### 第六步:输入 echo 'theme = "ananke"' >> config.toml
### 第七步:输入 hugo new posts/my-first-post.md
### 第八步:输入 hugo server -D（这一步操作之后，如果还想继续输入命令，需要先CTRL+c中断后，才可以继续。一旦中断，就无法使用http://localhost:1313 去访问你的博客。若不想中断，又想继续输入命令行操作，可以再新开一个Cmder窗口，进入到当前的目录下，继续操作。）
### 第九步:编辑配置文件 【打开quickstart里的config.toml文件，根据自己的需要自行进行设置】 （这步可以不用操作，使用默认的。）
### 第十步:输入 hugo
上述步骤在hugo的官网上也可找到。上述步骤完成之后，我们可以在自己本地http://localhost:1313 查看到自己的博客。但是想要别人看到还是不行的。下面的步骤是将博客和github结合，使得我们的博客能够在网上被其他人看到。
### 第十一步:在quickstart目录下新建一个名为.gitignore的文件，然后编辑文件，里面写上/public/,保存即可。（*这一步的作用其实是为了在备份angelafangguyue.github.io.creator文件夹，将其上传到github上的时候，不上传public文件夹。public文件夹在后面会自己再初始化一个仓库，上传到github。这两个文件夹上传到github上的仓库也是不一样的。*）
### 第十二步:在public目录下，输入git init，初始化这个本地仓库；git add .；git commit -v(上面这些git命令，如有不懂的请自行搜索))。
### 第十三步:去git hub中创建一个空仓库，名字为"你的github名.github.io"(如我的仓库名就是angelafangguyue.github.io)如图二十一。即在GitHub上创建一个新的空仓库，只需要在红线标注1的地方填上名字，然后点击红线标注2的地方创建即可。
![](/images/createHugo/hugo23.PNG)
上：图二十一
### 第十四步:将本地的public仓库上传到angelafangguyue.github.io的远程仓库。具体操作就是在public目录下输入命令：git remote add origin 你的仓库地址（注意这里请使用ssh地址），git pull。这样就把public文件夹以及里面的内容上传到你创建的名为"你的github名.github.io"的仓库了。这时，去仓库的setting中，在图二十三的GitHub Pages中，选择master保存，图二十二，二十三。然后就可以得到一个网址，通过这个网址，可以打开你的博客。别人通过访问你的这个网址也可以看到你的博客。一般这个网址就是https://username.github.io 。username就是你的github名。
![](/images/createHugo/hugo24.PNG)
上：图二十二
![](/images/createHugo/hugo25.png)
上：图二十三
### 第十五步:绑定自己的域名。先去namesilo购买一个属于域名。照着github上的操作说明或者自行搜索即可。
下面是我第一步到第十步的实际操作具体步骤：
#### 第一步：
去github网址 https://github.com/gohugoio/hugo/releases 下载适合自己电脑操作系统的hugo的版本，如下图一，下图二。下载完成后解压缩，会有一个名为hugo.exe的文件。将其放在合适的目录下，接着去配置系统环境变量。（我的电脑是windows10，64位，我下载的就如图二中的hugo_X.XX.X_Windows-64bit.zip。将解压之后的hugo.exe文件放在一个合适的位置。我将其放在了D:\Software\hugo 下，如下图三。接着将该路径添加到系统环境变量Path中，如下图四。如何添加系统环境变量请自行搜索。）
![](/images/createHugo/hugo1.PNG)
上：图一
![](/images/createHugo/hugo2.PNG)
上：图二
![](/images/createHugo/hugo3.PNG)
上：图三
![](/images/createHugo/hugo4.PNG)
上：图四

*安装成功后，在Git Bash或者Cmder中输入hugo version查看安装的hugo版本。成功如下图五*
![](/images/createHugo/hugo7.png)
上：图五
#### 第二步：
我选择将创建博客的文件夹存放在D:\yHugoBlog下，如下图六。使用Cmder打开后，输入命令hugo new site angelafangguyue.github.io.creator，如下图七。这一步执行之后，在D:\yHugoBlog下就创建了一个名字为angelafangguyue.github.io.creator的文件夹，如下图八。另进入该文件夹下，可以看到创建了其他一些文件夹，如下图九。 ）
![](/images/createHugo/hugo6.png)
上：图六
![](/images/createHugo/hugo8.png)
![](/images/createHugo/hugo9.PNG)
上：图七
![](/images/createHugo/hugo10.png)
上：图八
![](/images/createHugo/hugo13.PNG)
上：图九
#### 第三步：
继续在Cmder中输入 cd quickstart即cd angelafangguyue.github.io.creator,如下图十。这一步执行完成之后，就会进入到angelafangguyue.github.io.creator下操作，如下图十一。
![](/images/createHugo/hugo11.png)
上：图十
![](/images/createHugo/hugo12.png)
上：图十一
#### 第四步：
继续在Cmder中输入 git init即初始化这个文件夹仓库，如下图十二。执行完之后，可以看到目录angelafangguyue.github.io.creator下多了一个.git文件夹，如下图十三。同时还在.git目录下创建了一些其他文件夹，如下图十四。
![](/images/createHugo/hugo14.png)
上：图十二
![](/images/createHugo/hugo15.PNG)
上：图十三
![](/images/createHugo/hugo16.PNG)
上：图十四
#### 第五步：
继续在Cmder中输入 git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke。如下图十五：
![](/images/createHugo/hugo17.png)
上：图十五
#### 第六步:
继续在Cmder中输入 echo 'theme = "ananke"' >> config.toml

*第五步和第六步就是设置博客的主题，这里我就是按照官方提供的默认的直接去执行的，感兴趣的童鞋可以去搜索其他的主题并设置*
#### 第七步:
继续在Cmder中输入 hugo new posts/my-first-post.md 如下图十六hugo18。这一步是在angelafangguyue.github.io.creator的contents下创建一个posts文件夹，并且创建一个名为my-first-post的markdown文档，这就是我们的第一篇博客。执行完成后文件夹如下图十七hugo19。可以使用VSCode打开angelafangguyue.github.io.creator文件夹。然后编辑my-first-post的文档，编写属于你的第一篇博客，如图十八。
![](/images/createHugo/hugo18.png)
上：图十六
![](/images/createHugo/hugo19.PNG)
上：图十七
![](/images/createHugo/hugo20.png)
上：图十八
#### 第八步:
继续在Cmder中输入 hugo server -D，如图十九。这时在浏览器中输入http://localhost:1313 就可以看到创建的博客了。
![](/images/createHugo/hugo21.png)
上：图十九
#### 第九步:
我是在VSCode中打开的配置文件config.toml,改了一下默认的字体以及标题，如图二十。
![](/images/createHugo/hugo22.PNG)
上：图二十
#### 第十步:
继续在Cmder中输入 hugo。这样是在angelafangguyue.github.io.creator下面创建一个public的文件夹，里面还有一些文件以及文件夹。
#### 后续的步骤请参考上面的即可。
*注意要将最上面的draft值置为 false。这样在使用hugo命令的时候，才会将该博客同步到public文件夹中。*
### 使用 GitHub Pages 预览 HTML：就是进入创建的仓库的setting中，找到GitHub Pages，在source中选择master branch。会生成一个网址，点击即可访问。