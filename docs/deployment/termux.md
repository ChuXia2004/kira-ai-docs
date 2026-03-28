# Termux + PRoot Debian + NapCat + Kiraai 快速部署脚本与常用命令备忘

# 1. 换源（termux-change-repo，选中国大陆第3项）

# 2. 安装PRoot环境
pkg install proot-distro

# 3. 安装Debian
pd i debian

# 4. 登录Debian超级管理
pd login debian

# 5. Debian换源（选阿里云公网https）
bash <(curl -sSL https://linuxmirrors.cn/main.sh)

# 6. 安装sudo、curl、libgcrypt20
apt install -y sudo curl libgcrypt20

# 7. 防杀后台（Acquire wakelock，选无限制，显示wake lock held）

# 8. 返回termux原生环境
exit

# 9. 添加Debian快捷指令
echo "alias deb='pd login debian'" >> ~/.bashrc && source ~/.bashrc

# 10. 启动提示
echo "echo ' 输入deb或( pd login debian) 进入root@localhost:~#环境'" >> ~/.bashrc && source ~/.bashrc

# 11. 重新加载配置
source ~/.bashrc

# 12. 查看所有快捷指令
alias

# NapCat 安装
# 1. 进入Debian（输入 deb）
# 2. 下载安装脚本
curl -O https://raw.githubusercontent.com/NapNeko/NapCat-Installer/refs/heads/main/script/install.sh

# 3. 赋权并运行
chmod +x install.sh && ./install.sh

# 4. 选shell包安装，能跑就不代理

# 5. 加载配置
source ~/.bashrc

# 6. 启动NapCat
xvfb-run -a /root/Napcat/opt/QQ/qq --no-sandbox

# Kiraai 安装
# 1. 进入Debian（输入 deb）
# 2. 安装git
apt update && apt install -y git

# 3. 验证git
git --version

# 4. 克隆KiraAI
cd /root
git clone https://github.com/xxynet/KiraAI.git

# 5. 安装pyenv编译依赖
apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev xz-utils \
tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

# 6. 下载Python 3.11.11
wget https://mirrors.aliyun.com/python-release/source/Python-3.11.11.tgz

# 7. 解压
tar -xzf Python-3.11.11.tgz

# 8. 编译安装
cd Python-3.11.11
./configure --enable-optimizations --enable-shared
make -j$(nproc)
make altinstall

# 9. 更新库缓存
ldconfig

# 10. 验证安装
ls -l /usr/local/bin/python3.11
/usr/local/bin/python3.11 --version

# 11. 配置python软链接和pip
ln -sf /usr/local/bin/python3.11 /usr/local/bin/python
/usr/local/bin/python3.11 -m ensurepip --upgrade
/usr/local/bin/python3.11 -m pip --version

# 12. 配置Kiraai项目
cd /root/KiraAI
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cd scripts
./run.sh

# Termux原生环境添加快捷命令
# NapCat
echo "alias NC='pd login debian -- bash -c \"xvfb-run -a /root/Napcat/opt/QQ/qq --no-sandbox\"'" >> ~/.bashrc && source ~/.bashrc
echo "echo ' 输入 NC 启动 NapCat (自动进入Debian并运行NapCat)'" >> ~/.bashrc && source ~/.bashrc

# Kiraai
echo "alias Kiraai='pd login debian -- bash -c \"cd /root/KiraAI && source .venv/bin/activate && cd scripts && ./run.sh\"'" >> ~/.bashrc && source ~/.bashrc
echo "echo ' 输入 Kiraai 一键启动 KiraAI (自动进入 Debian 并激活 .venv 环境)'" >> ~/.bashrc && source ~/.bashrc

# 日常使用
# 启动Napcat: 输入 NC
# 启动Kiraai: 新建会话，输入 Kiraai
