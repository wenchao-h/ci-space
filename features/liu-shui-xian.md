# 流水线

流水线：通过新建流水线，根据用户需求使用特定功能和插件，制定执行流程步骤。

### 新建流水线

![](<../.gitbook/assets/image (5) (1) (1).png>)

建立空白流水线，使用模板新建流水线的方式参考步骤xxx



### **流水线配置**

流水线基础功能介绍

*   Trigger设置

    点击Trigger，包含推荐版本号以及流水线变量

    推荐版本号：默认不需要更改，可以自定义版本号/构建号

    流水线变量：定义执行流水线前需要输入的变量，例如：param1，同时在流水线中通过${param1}引用变量

    注：Trigger定义流水线的触发方式，包含手动执行、远程触发、定时任务等

![](../.gitbook/assets/image-20211128111038280.png)

*   添加stage，选择job类型为Linux环境

    概念说明：

    Stage：称为阶段，由多个 Jobs（作业）组成，同一个 Stage 下的 Job 执行方式为并行，由于 Job 之间是相互独立的，某个 Job 失败后，其它的 Job 会被运行到完成，一个 Job 失败，则该 Stage 失败.

    Job：运行的构建环境，分为有编译和无编译环境，有编译分为Windows/macOS/Linux，同时指定插件只能包含

    有编译和无编译的区别：

    * 有编译环境包含的依赖比较多，如python环境、gcc编译环境等，无编译环境只有基础java运行环境
    * 有编译环境的工作空间会映射到本地磁盘，无编译环境不会做映射

![](../.gitbook/assets/image-20211128140557840.png)

*   job配置

    选择构建资源类型为Docker公共构建机，镜像使用默认镜像bkci:latest，通过在构建机上启动容器作为构建环境

    构建资源类型分为：公共构建机和私有构建机

    区别：

    * 公共构建机是蓝盾自带的构建机，通过在构建机启动容器作为job的构建环境
    * 私有构建机是用户根据项目，将mac/linux/windows机器通过安装agent的方式接入到蓝盾作为构建机，私有构建机不启动容器，无需指定镜像
* 两者使用上区别不大，看具体项目使用需求

![](<../.gitbook/assets/image-20211128143559476 (1).png>)

*   添加插件

    注：每一个task对应一个插件，表示一个单独的任务，插件属于有编译环境或者无编译环境中的一种，每种插件不同的功能，比如拉取 Git 仓库代码，执行shell/bat等、

![](<../.gitbook/assets/image-20211128144444558 (1).png>)

点击shell script，编写要执行的shell脚本内容，${message}为Trigger定义的变量&#x20;

![](../.gitbook/assets/image-20211128145802260.png)

*   通知

    勾选通知方式之后，可以在流水线构建成功或者失败时，发送通知到指定人员的企业微信

![](../.gitbook/assets/image-20211212200740590.png)

*   基础设置

    默认【可同时运行多个构建任务】，可以多次执行同一流水线

    【锁定流水线，，任何触发方式都无法运行】：锁定后无法执行该流水线

![](../.gitbook/assets/image-20211212200924560.png)

【同一时间最多只能运行一个构建任务】：同一流水线同一时间只能一个任务在运行，其余执行的流水线会加入到排队机制中，进行排队执行

![](../.gitbook/assets/image-20211212201111977.png)

*   执行流水线

    点击保存并执行，输入message变量内容，点击执行

![](<../.gitbook/assets/image-20211128150039392 (1).png>)

查看执行结果

![](../.gitbook/assets/image-20211128150153728.png)

点击Shell Script查看脚本执行的输出结果

![](../.gitbook/assets/image-20211128150227922.png)

### 功能

#### 代码库

1. 源代码地址：可以通过bk-ci服务端正常访问的gitlab仓库地址，以http/https开头，以.git结尾；
2. 别名：关联后在bk-ci里显示的名字，这个别名会在流水线中关联代码库时显示，整个项目下唯一；
3. 访问凭证：点击右侧**新增**按钮可跳转到凭证管理中添加凭证。

![](<../.gitbook/assets/image-20211205223225943 (1).png>)

#### 凭证管理

配置的凭证可以在拉取代码库、调用代码库API、通过账号密码访问第三方服务时使用。

| 类型            | 描述                            |
| ------------- | ----------------------------- |
| 密码            | 定义后会作为流水线变量在各种插件中引用           |
| 用户名+密码        | 一般在关联SVN代码库时用到                |
| SSH私钥         | 关联GitLab代码库时（不需要GitLab事件触发）使用 |
| SSH私钥+私有Token | 关联GitLab代码库时（需要GitLab事件触发）使用  |
| 用户名密码+私有token | 关联GitLab代码库时（需要GitLab事件触发）使用  |

## 示例

### 示例一(代码拉取+制品库上传下载)

* 点击代码库，（代码库介绍详情见【代码库】目录）

![](../.gitbook/assets/image-20211212214606792.png)

* 选择关联Gitlab代码库

![](../.gitbook/assets/image-20211205163725597.png)

* 点击新增，会自动跳转到凭证管理--新增凭据，获取gitlab上的AccessToken进行填写，点击确定&#x20;

![](../.gitbook/assets/image-20211205164052362.png)

* 回到代码库，访问凭据上选择凭据【demo】，确定

![](../.gitbook/assets/image-20211205164627965.png)

* 创建流水线

![](../.gitbook/assets/image-20211213100550657.png)

添加新的stage

![](../.gitbook/assets/image-20211213100650213.png)

添加checkout gitlab插件，关联创建好的代码库，按分支拉取

![](../.gitbook/assets/image-20211205170423284.png)

添加shell插件，查看当前工作空间，以及拉取的代码目录，对代码进行打包

![](../.gitbook/assets/image-20211209202103728.png)

把当前工作空间的ShortUrl.zip文件保存下来放到制品库，可以使用upload artifacts插件

注：当在使用多种构建机类型的情况下(公共构建机+私有构建机或多个不同私有构建机)，不同构建机之间需要依赖使用构建产物时(不仅限于拉取的代码)，可以把文件/文件夹上传到制品库，在从需要使用到该文件的Job下，下载制品库对应的文件

* 添加upload artifacts插件

![](../.gitbook/assets/image-20211209202425994.png)

*   download artifacts插件---从制品库上下载文件到当前Job下的工作空间

    添加新stage

![](../.gitbook/assets/image-20211209202505648.png)

添加Download artifacts插件，会根据填写好的内容自动找到制品库的源路径，然后下载到当前工作空间

![](../.gitbook/assets/image-20211209203143751.png)

### 示例二（流水线触发方式使用-手动执行/定时任务/远程触发等）

触发方式是在Trigger下通过选择不同类型的插件使用

*   手动执行

    创建流水线默认的触发方式为手动执行，这里简单添加一个stage\
    Manual插件：手动执行流水线，指流水线建立之后，可以在右上角点击保存，并且手动点击执行流水线的按钮

![](../.gitbook/assets/image-20211209205014251.png)

*   定时任务

    添加Timer插件，定义crontab表达式

![](../.gitbook/assets/image-20211209211036492.png)

因为没有选择Manual手动触发的方式，所以无法手动点击【执行】按钮来执行流水线

![](../.gitbook/assets/image-20211209211118734.png)

查看执行历史，可以看到流水线以每分钟自动定时执行一次

![](../.gitbook/assets/image-20211209211239418.png)

*   远程触发

    添加Remote插件--可以通过执行命令进行远程触发

    复制示例命令，如果trigger有定义变量的话，命令行会自动生成含变量参数的示例

![](../.gitbook/assets/image-20211209211650003.png)

命令行中执行curl命令

![](<../.gitbook/assets/image-20211209211620739 (1).png>)

流水线执行历史上可以查看到执行记录

![](../.gitbook/assets/image-20211209211547148.png)

### 示例三（流水线变量使用）

* 变量使用类型：
  * 全局变量引用，shell/python/windows变量引用方法
  * 插件变量设置方法以及跨插件变量引用
* 点击Trigger，新增变量message

![](<../.gitbook/assets/image-20211210111540640 (1).png>)

* 添加stage，Job下选择Python脚本执行插件，通过${message}使用变量

![](../.gitbook/assets/image-20211210111540640.png)

添加shell插件，使用${message}使用变量

![](../.gitbook/assets/image-20211212170436642.png)

添加stage，Job类型选择windows

![](../.gitbook/assets/image-20211212170545118.png)

点击windows，选择windows私有构建机节点

![](../.gitbook/assets/image-20211212170801357.png)

添加Batch Script插件，使用%message%引用变量

![](../.gitbook/assets/image-20211212171003864.png)

*   插件设置全局变量方法

    在python脚本执行插件上，使用print("setEnv demo test1")，即设置了一个环境变量：demo（注：demo变量不能在当前插件使用）

![](../.gitbook/assets/image-20211212171417261.png)

在shell script以及batch script插件引用变量demo，跟trigger配置的变量引用的方式一样

![](../.gitbook/assets/image-20211212171708729.png)

![](<../.gitbook/assets/image-20211212171725307 (1).png>)

windows设置全局变量的方式，call:setEnv "test" "testmessage"，引用test变量的方式如上

![](../.gitbook/assets/image-20211212172006260.png)

*   流水线默认全局变量

    点击任意插件，右上角有一个引用变量，流水线默认定义了一些变量，可以直接在插件上进行使用，变量引用方式如上

![](../.gitbook/assets/image-20211212172109877.png)



### 示例四（使用模板创建流水线）

模板：流水线模板，通过创建流水线模板，预先设置好通用的流水线，新建流水线时可以选择模板来创建流水线。

* 流水线--》右上角选择模板管理，

![](../.gitbook/assets/image-20211212172908537.png)

![](../.gitbook/assets/image-20211212174045747.png)

简单新建一个stage测试

![](../.gitbook/assets/image-20211212193540068.png)

点击保存，保存为新版本v1（当前版本默认为init版本，也可以保存在当前版本)

![](../.gitbook/assets/image-20211212193812300.png)

点击右上角版本列表，可以查看当前版本，并且点击【加载】可以切换到其它版本

![](../.gitbook/assets/image-20211212194017063.png)

也可以在【设置】里针对流水线预先设置通知等

![](../.gitbook/assets/image-20211212215433258.png)

* 使用模板新建流水线，选择创建好的模板

![](../.gitbook/assets/image-20211212215811153.png)

创建之后可以看到预先在模板配置的内容

![](../.gitbook/assets/image-20211212215943119.png)

### 示例五（流水线条件判断）

* 新建多个stage

![](../.gitbook/assets/image-20211215150951360.png)

添加shell script插件，随便输入脚本：echo 1

![](../.gitbook/assets/image-20211226153407230.png)

在添加一个并行的stage&#x20;

![](../.gitbook/assets/image-20211226153240321.png)

![](<../.gitbook/assets/image-20211226153513910 (2).png>)

后面新加一个stage

![](../.gitbook/assets/image-20211226153714984.png)

![](../.gitbook/assets/image-20211226153730961.png)

添加finally stage

Finally stage: 流水线执行的最后一步，无论流水线执行失败还是成功，都会执行finally stage定义的步骤

![](../.gitbook/assets/image-20211226153811917.png)

![](../.gitbook/assets/image-20211226153848275.png)

*   场景：配置好上述的流水线之后，假如有这么一个使用需求，在满足一定条件下，只执行2-1的Job，并且跳过执行2-2和3-1

    通过配置变量的方式实现

    * 定义流水线变量，点击trigger，定义test1和test2两个变量

![](<../.gitbook/assets/image-20211226154320301 (3).png>)

* 配置2-1的Job，选择【自定义变量全部满足时才运行】，输入刚刚自定义的两个变量test1、test2，变量值与trigger定义的值相同

![](<../.gitbook/assets/image-20211226154559254 (1).png>)

* 配置2-2的Job，点击2-2 Linux，选择【自定义变量全部满足时才运行】，输入刚刚自定义的两个变量test1、test2，值随便写，与刚刚trigger定义的值不同即可

![](../.gitbook/assets/image-20211226154729382.png)

*   配置3-1 stage，选择【自定义变量全部满足时才运行】，输入刚刚自定义的两个变量test1、test2，值随便写，与刚刚trigger定义的值不同即可

    注：流水线可以针对stage/Job/插件来配置执行条件

![](../.gitbook/assets/image-20211226154952271.png)

* 执行流水线，test1和test2变量值保持默认

![](../.gitbook/assets/image-20211226155258274.png)

查看执行结果，可以看到2-2Job和stage3因为变量条件不满足，直接跳过不执行

![](../.gitbook/assets/image-20211226155240606.png)

### 示例六（子流水线）

子流水线：可以在当前流水线调用项目下的其它流水线

* 新建stage

![](../.gitbook/assets/image-20211218153126542.png)

* 选择call pipeline插件，这里选择调用的流水线为【子流水线demo】，同时还分同步和异步调用执行，填写子流水线的变量参数

![](../.gitbook/assets/image-20211218153213103.png)

![](../.gitbook/assets/image-20211218161418247.png)

* 添加shell script插件，睡眠20s

![](../.gitbook/assets/image-20211218161316700.png)

* 子流水线demo，执行sleep 10的操作

![](../.gitbook/assets/image-20211218161541806.png)

* 执行流水线，call pipeline在调用子流水线之后会立即成功，然后执行下一步操作，而此时【子流水线demo】会异步在执行（如果同步的情况下，会等待子流水线执行完成之后，在执行下一个）

![](../.gitbook/assets/image-20211218161727205.png)

![](<../.gitbook/assets/image-20211218161710687 (1).png>)
