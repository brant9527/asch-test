# asch-test
to run asch project
asch 下载安装步骤
git clone https://github.com/AschPlatform/asch.git
安装nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
载入nvm
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

安装node 
nvm install node 8
查看版本
node --version
安装asch脚手架
npm install -g asch-cli
进入asch目录 随后安装
> cd <asch source code dir>
> npm install
> node app.js
安装过程中如果遇到 libsodium 加密功能问题
https://www.npmjs.com/package/sodium 查了下官方包，说的是缺少libsodium，需要自己编译好。按照官网上面步骤走

官方项目钱包，缺少编译后的dist目录，需要自己npm install 一下。根据log提示，按需进行安装需要的模块
经过一些列安装后遇到npm 出现问题重新安装过node 哈呦
