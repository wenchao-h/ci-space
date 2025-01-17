# 已知问题

\[TOC]

#### Q: 用户组和用户自定义权限重叠导致用户对流水线的权限不符合预期，ci <1.7

#### **出现版本：ci<1.7**&#x20;

偶现/必现：必现&#x20;

描述：用户所在的用户组拥有「test」项目全部权限，同时用户自定义权限有「test项目」部分流水线权限，进入流水线页面，**概率性**出现无法查看流水线的情况  &#x20;

![可以查看流水线](<../.gitbook/assets/image (9) (1) (1) (1) (1) (1).png>)

![无法查看流水线](<../.gitbook/assets/image (6) (1) (1) (1).png>)

**Q: gitlab偶现获取凭证失败**

![](../.gitbook/assets/wecom-temp-941115d684647ac6fe940676a7854656.png)

已知问题，**影响版本<=1.5.23**

把ticket/lib/bcprov-jdk15on-1.64.jar这个包删除，然后重启ticket服务`systemctl restart bk-ci-ticket.service`

**Q: 我想把单元测试报告作为产出物报告，上传成功。但是打开报告，无法正常显示，报错「This request有 哈哈是beenblocked； the content must be served over https」**

![](../.gitbook/assets/wecom-temp-76f4802ef5f78b0abfda917c2575106a.png)

已知问题，**影响版本: 1.5.x**

**Q: 使用了模板创建了3个流水线实例，删除了一个，还是显示3个实例，把所有实例删除后，模板也无法删除**

![](../.gitbook/assets/企业微信截图\_16389525588929.png)

![](../.gitbook/assets/企业微信截图\_16389527024197.png)

已知问题，影响范围：版本<**1.5.x**

已在v1.7版本中进行了修复

**Q: 通过流水线创建的代码分析任务，如果流水线删除后，再停掉代码分析任务，会报错**

![](<../.gitbook/assets/企业微信截图\_16395354641542 (1).png>)

![](../.gitbook/assets/企业微信截图\_16395354744740.png)

已知问题，影响范围：**版本1.5.x**

**Q: 蓝盾输出的日志先后顺序混乱**

![](../.gitbook/assets/企业微信截图\_16316936739387.png)

已知问题，影响范围：**版本1.5.x**

**Q: 蓝盾流水线申请权限的时候，点了去申请，页面没有跳转到权限中心，F12查看响应有bkiam v3 failed报错**

![](../.gitbook/assets/企业微信截图\_16384143961812.png)

![](../.gitbook/assets/企业微信截图\_16384146286005.png)

已知问题，影响范围：**版本1.5.x**

**Q: 项目不允许使用TGit插件，请检查插件是否被正确安装**

![](../.gitbook/assets/image-20220125154003687.png)

已知问题，TGit对接的是腾讯的工蜂，社区版使用受限**，影响版本<1.5.35**

****
