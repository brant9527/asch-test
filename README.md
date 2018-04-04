# 1 asch 安装阿希以及钱包
to run asch project and pubilsh dapp on asch（if anynoe need english transler，please connect me）

## 1.1 asch 下载安装步骤
> git clone https://github.com/AschPlatform/asch.git

##安装nvm
> curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash

## 1.2 载入nvm
> export NVM_DIR="$HOME/.nvm"

> [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

> [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

## 1.3 安装node 
> nvm install node 8
## 1.4 查看版本
> node --version
## 1.5 安装asch脚手架
> npm install -g asch-cli
## 1.6 进入asch目录 随后安装
> cd <asch source code dir>
  
> npm install

> node app.js
## 1.7 安装过程中如果遇到 libsodium 加密功能问题
https://www.npmjs.com/package/sodium 查了下官方包，说的是缺少libsodium，需要自己编译好。按照官网上面步骤走（这里有疑问可以随时沟通）
官网教程上 ：
> npm install sodium --unsafe-perm

如果不行就运行

> npm install node-gyp -g

没有权限就

> sudo npm install node-gyp -g

本人是mac 所以用brew进行安装 请注意没有brew 请先安装brew
> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


> brew install libtool autoconf automake

如果是linux ，If you cannot compile libsodium on Linux, try installing libtools with:

> sudo apt-get install libtool-bin

官方项目钱包，缺少编译后的dist目录，需要自己npm install 一下。根据log提示，按需进行安装需要的模块
## 1.8 安装钱包打包工具
- bower (npm install bower -g)
- gulp (npm install gulp  -g)
- browserify (npm install browserify -g)
## 1.9 启动打包编译
> gulp build-test

首先要进入你的asch源码目录

> node app.js

并确保localnet启动（访问http://127.0.0.1:4096,如能打开钱包页面说明启动成功）



# 2 发布dapp

## 2.1 生成五个账户
每个dapp都有独立的受托人，这些受托人也是默认的记账人，他们负责区块的生产，跨链资产的中转，与此同时可以获得交易手续费。
注册dapp的时候，我们只需要收集受托人的公钥就行，为了权力分散，最好每个秘钥分别由一个人保管。
这里为了演示，我们一次性创建5个账户，一个dapp最多有101个受托人，最少是5个。
// 注意这里的密码都是演示用途，且不可用于正式的dapp中

    > asch-cli crypto -g

    #　接下来输入 5 即可生成5个账户
    [ { address: 'AijfU9bAE6Jpt5ve7zG3LoriDamH67eLb', // 地址
        secret: 'easy snap cable harvest plate tone planet yellow spot employ humble what', // 主密码，也叫一级密码，可以生成公钥和地址，实质为私钥的助记词，必须记录下来
        publicKey: 'a437a1d4bedf738e8620920ef29542644e3366c635b16fc9faa6f5db744bcd5c' },// 公钥，用于4.2章节配置受托人公钥
      { address: 'ABGGUL5D2SoBaQTqDMAb3u9RdUjYBcmRxx',
        secret: 'adjust edge exist hurry joke carbon spice envelope battle shuffle hawk thought',
        publicKey: '522cdc822d3bec74aa5c4e972ed6cba84850f9c4d521e43fe08675e9e4759bb9' },
      { address: 'AMg37s4avDUojJd6d3df7HPA3vqtRRwved',
        secret: 'survey spoil submit select warm chapter crazy link actual lonely pig grain',
        publicKey: '6ee3ae36166f69e8b9408d277486c9870f40c1b7c16016328737d6445409b99f' },
      { address: 'AL5p8BHzhCU3e5pkjMYbcjUSz771MrQcQr',
        secret: 'march struggle gap piece entry route kind pistol chunk spell honey summer',
        publicKey: 'ad558e44b347a54981295fcb5ee163c2915ca03536496129103e9d72c5025d69' },
      { address: 'A2WassKticpB7cx15RZfenBekthwmqXRXd',
        secret: 'response modify knife brass excess absurd chronic original digital surge note spare',
        publicKey: '6b2594ebeee9b072087e5f1e89e5c41ee2d73eb788b63abeedf5c04664f0ce5b' } ]

应用模板包括注册dapp必须的元信息、创世块以及一个初始的目录结构

生成应用模板需要使用dapps子命令，如下所示

## 2.2 创建dapp文件夹
> mkdir asch-test-dapp && cd asch-test-dapp
> asch-cli dapps -a
## 2.3 dapp应用消息
接下来，我们要回答一系列的问题，以生成应用的注册信息与创世块

    ? Enter DApp name Asch-test-dapp
    ? Enter DApp description Demo of asch dapp
    ? Enter DApp tags asch,dapp,demo
    ? Choose DApp category Common
    ? Enter DApp link https://github.com/AschPlatform/Asch-test-dapp.zip
    ? Enter DApp icon url https://yourdomain.com/logo.png
    ? Enter public keys of dapp delegates - hex array, use ',' for separator //这里是2.1章节生存的5个受托人对应的公钥
    a437a1d4bedf738e8620920ef29542644e3366c635b16fc9faa6f5db744bcd5c,522cdc822d3bec74aa5c4e972ed6cba84850f9c4d521e43fe08675e9e4759bb9,6ee3ae36166f69e8b9408d277486c9870f40c1b7c16016328737d6445409b99f,ad558e44b347a54981295fcb5ee163c2915ca03536496129103e9d72c5025d69,6b2594ebeee9b072087e5f1e89e5c41ee2d73eb788b63abeedf5c04664f0ce5b
    ? How many delegates are needed to unlock asset of a dapp? 3
    DApp meta information is saved to ./dapp.json ...
    ? Enter master secret of your genesis account [hidden]
    ? Do you want publish a inbuilt asset in this dapp? Yes
    ? Enter asset name, for example: BTC, CNY, USD, MYASSET XCT
    ? Enter asset total amount 1000000
    ? Enter asset precision 8
### 有几个注意事项
1. DApp link是为了方便普通用户自动安装，必须以.zip结尾, 如果您的dapp不打算开源或者没有准备好，可以把这个选项当做占位符，它所在的地址不必真实存在
2. DApp icon url这是在阿希应用中心展示用的应用图标, 必须以.jpg或.png结尾，如果该图片无法访问，阿希应用中心会展示一个默认的图标
3. How many delegates ...这个选项表示从dapp跨链转账资产时需要多少个受托人联合签名，该数字必须大于等于3、小于等于你配置的受托人公钥个数且小于等于101，数字越大越安全，但效率和费用越高
4. Dapp的创世块中可以创建内置资产，但不是必须的，内置资产无法跨链转账，只能在链内使用。在主链发行的UIA（用户自定义资产）可以充值到任意dapp中，也可从dapp提现到主链，这是dapp内置资产和UIA最大的区别。“一链多币，一币多链”指的就是主链可以发行多个UIA，而每个UIA都可以充值到多个dapp中。

## 2.4 注册应用到主链
注意这里的主链不是指mainnet， 每个net下都有相应的主链， 主链是相对Dapp（侧链）而言。

我们可以使用registerdapp注册应用到主链，如下所示

// 先生成一个dapp注册账户
// 注意这里的密码都是演示用途，且不可用于正式的dapp中
    > asch-cli crypto -g
    ? Enter number of accounts to generate 1
    [ { address: 'A9rhsV5xDny4G45gD2TXmFFpeiTfvAAQ7W',
        secret: 'possible melt adapt spoon wing coyote found flower bitter warm tennis easily',
        publicKey: '74db8511d0021206abfdc993a97312e3eb7f8595b8bc855d87b0dc764cdfa5a8' } ]
    Done

    // 在http://127.0.0.1:4096用localnet的创世账户“someone manual strong movie roof episode eight spatial brown soldier soup motor”登陆（该账户中有初始发行的1亿xas token），然后给A9rhsV5xDny4G45gD2TXmFFpeiTfvAAQ7W地址转10000个xas
    
    > asch-cli registerdapp -f dapp.json -e "possible melt adapt spoon wing coyote found flower bitter warm tennis easily"
    # 返回结果如下,这就是应用id。每个应用注册时返回的id不同，请记下你自己的应用id
    0599a6100280df0d296653e89177b9011304d971fb98aba3edcc5b937c4183fb

###（这里请注意，创世号是一创建阿希便有的账号（内有1亿阿希），首先确保我们需要注册的账号里面有10000xas，注意转账地址别填写错误，阿希官网教程上面地址并不正确（瞎填），要填入我们刚生成的地址，即：A9rhsV5xDny4G45gD2TXmFFpeiTfvAAQ7W，转账完成后，即可生成dapp ）

使用浏览器访问http://localhost:4096/api/dapps/get?id=0599a6100280df0d296653e89177b9011304d971fb98aba3edcc5b937c4183fb, 可以查询到该dapp了，下面是返回信息

      {
          "success": true, 
          "dapp": {
              "name": "asch-dapp-helloworld", 
              "description": "A hello world demo for asch dapp", 
              "tags": "asch,dapp,demo", 
              "link": "https://github.com/AschPlatform/asch-dapp-helloworld/archive/master.zip", 
              "type": 0, 
              "category": 1, 
              "icon": "http://o7dyh3w0x.bkt.clouddn.com/hello.png", 
              "delegates": [
                  "a518e4390512e43d71503a02c9912413db6a9ffac4cbefdcd25b8fa2a1d5ca27", 
                  "c7dee266d5c85bf19da8fab1efc93204fed7b35538a3618d7f6a12d022498cab", 
                  "9cac187d70713b33cc4a9bf3ff4c004bfca94802aed4a32e2f23ed662161ea50", 
                  "01944ce58570592250f509214d29171a84f0f9c15129dbea070251512a08f5cc", 
                  "f31d61066c902bebc80155fed318200ffbcfc97792511ed18d85bd5af666639f"
              ], 
              "unlockDelegates": 3, 
              "transactionId": "0599a6100280df0d296653e89177b9011304d971fb98aba3edcc5b937c4183fb"
          }
      }


这样发布成功了！有问题随时交流，经过一些列安装后遇到npm 出现问题重新安装过node 哈呦
