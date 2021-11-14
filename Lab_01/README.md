# 实验1_Android开发基础
## 一、创建第一个Android工程并同步至GitHub
### 1、注册一个GitHub账号，安装Git工具
* GitHub官网：https://github.com/
* Git官网：https://github.com/fjnu-cse
### 2、下载Android Studio
* Android Studio官网：https://developer.android.google.cn/studio?hl=zh-cn
### 3、创建Android工程并同步到GitHub
(1) 创建新Android工程<br/>
第一次使用Android Studio的话，打开软件后，选择“Creat New Project”就可以创建了。<br/>
如果是之前创建过安卓项目，想要再次创建的话，依次点击左上角“File”，“New”，“New Project”就可以创建了。<br/>

(2) 同步到GitHub
* 选中你要同步的文件夹，右键选择“Git Bash Here”
* 输入“git init”，创建一个本地git仓库
* 输入“git add .”，将文件添加到本地仓库
* 输入“git commit -m "提示"”，这个操作是添加一个提示，说明这次的更新是干什么的，说明写在“提示”里面
* 输入“git push origin master”，同步到远程仓库里面
* 如果“push”不成功的话，输入“git pull origin master”即可。
* 如果出现以下情况：
<img src="https://z3.ax1x.com/2021/10/25/5hhkp4.png" width=500>
输入“git remote add origin git@github.com:用户名/仓库名.git”，然后重新“push”即可。<br/>

(3) 详细过程参考以下链接：
* 命令行的方式通过git同步：https://blog.csdn.net/fjnu_se/article/details/66472625

### 4、我的“.gitignore”文件
```xml
*.iml
.gradle
/local.properties
/.idea/caches
/.idea/libraries
/.idea/modules.xml
/.idea/workspace.xml
/.idea/navEditor.xml
/.idea/assetWizardSettings.xml
.DS_Store
/build
/captures
.externalNativeBuild
.cxx
local.properties
```
## 二、参考链接
* https://www.twle.cn/l/yufei/android/android-basic-index.html
* https://developer.android.google.cn/guide
