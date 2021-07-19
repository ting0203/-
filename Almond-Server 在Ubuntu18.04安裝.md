# Almond-Server 在Ubuntu18.04安裝

## 參考網頁https://github.com/stanford-oval/almond-server上Development setup操作

+ 安裝Ubuntu18版以上，並將檔案從  github 上 clone Almond Server資料夾至 Ubuntu 本地端

  ```
  sudo git clone https://github.com/stanford-oval/almond-server.git
  cd almond-server/
  ```

  + 先至terminal中安裝 git 套件

  + ``` 
    sudo apt update  //更新套件
    sudo apt-get dist-upgrade  //升級現有已安裝的套件及其相依套件，會最近一次「建議安裝套件」一併安裝
    sudo apt install git  //安裝git套件
    ```

  + <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210508235354491.png" alt="image-20210508235354491" style="zoom:50%;" />

+ 使用以下命令安裝依賴項：

  ``` 
  sudo apt -y install nodejs gettext build-essential make g++ graphicsmagick zip unzip
  ```

+ ``` 
  npm install   //Terminal輸入 => 會出現未安裝npm警示
  ```

  <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210518221409801.png" alt="image-20210518221409801" style="zoom:50%;" />
  
  
  
+ **安裝 Node.js 及 NPM 參考** => https://ithelp.ithome.com.tw/articles/10194339

  + 安裝Node.js及NPM：連至Node.js官網 https://nodejs.org/ 點選導覽列的下載

    <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210509000752643.png" alt="image-20210509000752643" style="zoom:50%;" />

  + j網頁下滑至 "選擇其他平台 (Additional Platforms)" 選項裡的從套件管理工具"安裝 (Installing Node.js via package manager) "連結。

    <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210509001201510.png" alt="image-20210509001201510" style="zoom:50%;" />

  + 再點選各 Linux 發行版裡的「Debian and Ubuntu based Linux distributions」後，取得安裝指令。

  + <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210509001425279.png" alt="image-20210509001425279" style="zoom:50%;" />

  + <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210509001529462.png" alt="image-20210509001529462" style="zoom:55%;" />

  + **選定版本，依照指令一一輸入**

    Node.js v16.x :

    ```
    # Using Ubuntu
    curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
    sudo apt-get install -y nodejs 或 sudo apt-get install nodejs
    ```

    <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210518221527006.png" alt="image-20210518221527006" style="zoom:50%;" />

    安裝完成後，可以用指令來確認一下安裝的版本。首先檢查 Node.js 及npm 的版本：
  
    ``` 
    node -v
    npm -v
    sudo apt install build-essential  //編譯套件 (build-essential)
  ```
  
  
  
+ **安裝 Yarn**
  
    依照 Linux 安裝指引，先加入 Yarn 官方 key 及 ppa 來源：
  
    ``` 
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
  echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
  ```
  
    然後就可以直接用 apt 裝 yarn 這個套件了：
  
    ```
    sudo apt update  //確認更新
    sudo apt install yarn  //安裝yarn
  yarn -v  //確認版本
  ```
  
+ **安裝 Global 套件**
  
    網誌筆者示範安裝的 Global 套件就是號稱可以取代 htop 指令的 vtop。
  
    ```
    # 使用 NPM
    $ sudo npm install -g vtop
    
    # 使用 Yarn
  $ sudo yarn global add vtop
  ```
  
  
  
  
  
  + **套件全部安裝完後**
  
    ```
  sudo npm install
    sudo npm install -g npm@7.13.0 //更新到此版本
    ```
  ```
  
    <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210518221843582.png" alt="image-20210518221843582" style="zoom:50%;" />
  
  <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210518221912611.png" alt="image-20210518221912611" style="zoom:50%;" />
  
    
  
  + **最後一步：開啟服務**
  
  ```
    sudo npm start
    ```
  
    <img src="C:\Users\AGS\AppData\Roaming\Typora\typora-user-images\image-20210509002729351.png" alt="image-20210509002729351" style="zoom:50%;" />
    
    
    
    
    ```