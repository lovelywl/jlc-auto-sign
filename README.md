# 嘉立创自动签到脚本
*By zhangMonday*

[立创开源平台](https://oshwhub.com/)

[嘉立创金豆中心](https://activity.jlc.com/goods/goodsList?spm=JLC.MEMBER)

QQ讨论群 1023057538

请确保你的立创账号已经在开源平台设置了昵称，否则开源平台自动签到将无法运行

---

## 项目简介  
该项目利用 **Github Actions** 实现立创开源平台以及立创金豆的每天自动化签到

   - 支持多账号自动签到
   - 支持自动领取开源平台签到7日礼包和每月礼包
   - 支持自动过登录滑块验证
   - 支持自动通过登录信息抓包X-JLC-AccessToken和secretkey实现金豆签到
   - 支持7日金豆奖励领取，**请注意脚本默认领取8金豆奖励，如要领取优惠券等奖励，请在脚本运行前手动领取掉**
   - 支持签到错误自动重试，每个账号最多尝试签到5次（正常1次+重试3次+最终尝试1次）
   - 支持签到失败邮件提醒
   - 支持自动静默更新脚本
   - 理论上支持所有远端的日志推送（默认支持推送到Telegram、企业微信、钉钉、PushPlus、Server酱、酷推，另外也支持自定义推送到任意api）

---

## 配置方法  

### 1. Fork 项目到自己的仓库  
- 访问项目页面，点击 `Fork` 按钮将项目复制到自己的仓库。  

<img src="img/1.jpg" width="600" />

---

### 2. 配置嘉立创账号和密码/签到失败提醒/签到日志推送
1. 进入你 Fork 的仓库，点击 **Settings** → **Secrets** → **New repository secret**。  
2. 添加以下两个密钥：  
   - **Name**: `JLC_USERNAME`  
     **Value**: 你的嘉立创登录账户邮箱/手机号/客编，多账号用英文逗号分割，要和密码一一对应
   - **Name**: `JLC_PASSWORD`  
     **Value**: 你的嘉立创登录密码，多账号用英文逗号分割，要和账号一一对应

<img src="img/2.jpg" width="600" />

3. **返回错误退出代码功能(默认关闭)：**
   开启该功能后，当任何账号签到出现问题时，程序完整运行结束后会传递错误退出代码。Github Actions收到错误退出码后，会显示工作流运行失败，同时发送报错邮件给你。
   
   **因此开启该功能相当于接收签到失败的邮件提醒**
   
   开启方法:
   在secret里再添加一个值
   
   - **Name**: `ERROR`  
     **Value**: `true`

5. **推送签到日志到第三方平台(可选)**

   理论上支持所有远端的日志推送（默认支持推送到Telegram、企业微信、钉钉、PushPlus、Server酱、酷推，另外也支持自定义推送到任意api）
   
   
   [查看日志推送配置教程](pushlog.md)

---

### 3. 配置 Actions 自动化执行签到和更新脚本
1. 在你 Fork 的 GitHub 仓库中点击 **Actions**  
2. 按照图示方法启用 GitHub Actions

<img src="img/3.png" width="600" />

3. 在图中圈出的地方找到**嘉立创自动签到主脚本**和**自动更新并保持签到脚本活跃**，分别点击 **Run workflow** 手动触发一次，如果运行结束出现绿色对勾则为配置成功

<img src="img/4.jpg" width="600" />

当自动更新发邮件报错时，需要手动更新脚本，详见下文**手动获取更新**

---

### 4. 修改自动化执行时间  
在 `.github/workflows/` 里两个yaml文件中，你可以根据注释修改自动签到时间。  

如果不修改，默认自动签到时间为北京时间早上 9 点(01:00 UTC)，默认自动更新脚本时间是每7天0点更新一次

实际运行时间会比设置时间延迟一小时以内，具体取决于当时 GitHub 资源占用情况。

<img src="img/5.png" width="600" />

---

### 5. 手动获取脚本更新

当自动更新发邮件报错时，需要手动更新脚本。在你fork的仓库中找到Sync Fork，点击即可更新，如图

<img src="img/7.png" width="600" />

下一次自动签到会运行更新后的程序，自动更新也会恢复正常

---

### 本地部署说明

如果你希望在本地环境运行此脚本，可以按照以下步骤进行配置：

环境要求

· Python 3.7 或更高版本

· Chrome 浏览器

· ChromeDriver（与Chrome版本匹配）

安装步骤

1. 克隆项目到本地

```bash
git clone https://github.com/zhangMonday/jlc-auto-sign.git
cd jlc-auto-sign
```

2. 安装Python依赖

```bash
pip install selenium requests
```

3. 安装ChromeDriver
   · 访问[ChromeDriver](https://developer.chrome.com/docs/chromedriver/downloads)下载页面
   · 下载与你的Chrome版本匹配的ChromeDriver
   
   · 将ChromeDriver添加到系统PATH中：
   
     · Windows: 解压到 C:\Windows\system32\
   
     · Linux/macOS: 解压到 /usr/local/bin/

5. 运行脚本

```bash
python jlc.py 账号1,账号2,账号3... 密码1,密码2,密码3...
```

---

### 运行日志（节选）
![运行日志](img/6.png)

---

### 捐赠

[打赏支持](https://donate.zhangmonday.top/)

如果本项目对你有点帮助，请考虑给作者捐赠～哪怕0.01也行，我能知道有人在用

你的捐赠是我继续维护和更新该脚本的最大动力！

---

### 致谢  
本项目参考和复用了 [https://github.com/wmathor/Check_In](https://github.com/wmathor/Check_In) 的部分代码，感谢！

本项目参考了 [https://github.com/sudojia/AutoTaskScript](https://github.com/sudojia/AutoTaskScript/) 的嘉立创脚本，感谢！

欢迎提交issue和PR🎉(´∀｀)🎈
 

 
