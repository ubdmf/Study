# install

## FeiShu
- sudo dpkg -i  Feishu-linux_x64-5.14.14.deb
## Git
- sudo apt-get install git
## 安装vscode
- 进入VSCode官网Visual Studio Code - Code Editing. Redefined
- 下载Linux x64.deb版本。并将其托到Ubuntu的Downloads文件夹中。
-  sudo dpkg -i vscode
## 安装 ncat
sudo apt -y install ncat
##  安装中文输入法
- - im-config -s ibus
- sudo apt install ibus-gtk ibus-gtk3
- sudo apt install ibus-pinyin
## Teamviewr
-  sudo dpkg -i  teamviewer 
-  sudo apt-get instll -f 

## oh my zsh
- `sudo apt-get install zsh` 安装zshch
- `chsh -s /bin/zsh`  需要重启一次才能生效

- `wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh `
- `source ~/.zshrc `

**有两个插件是很有用的：**
- 自动补全插件
    - git clone https://github.com/sangrealest/zsh-autosuggestions.git ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
    - 将zsh-autosuggestions 加入 ~/.zshrc中的plugins
- 语法高亮 
  - `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/`
  - **zsh-syntax-highlighting**
  - 将 zsh-syntax-highlighting 加入 ~/.zshrc中的plugins
  - `gedit ~/.zshrc` 
  - `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`
  - `ZSH_THEME="ys"`
  - `source ~/.zshrc`