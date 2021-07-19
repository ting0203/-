# Cloud Almond Installation

1. 安裝 Linux 環境，這邊使用Ubuntu 18 LTS版。
2. 依照官網說明操作，並解決一切衝突與問題。

## STEP 1  獲取相依關係

+ nodejs（> = 8.0）
+ cvc4（任何版本，儘管建議> = 1.5，但建議使用；僅需要二進製文件，而無需庫）
+ gm（由GraphicsMagic提供）

``` bash
#在Ubuntu(>=18.04)上:
$ sudo apt install nodejs cvc4 graphicsmagick libsystemd-dev bubblewrap -y
$ sudo apt-get update
$ sudo apt-get upgrade  #更新套件
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524164155712.png" alt="image-20210524164155712" style="zoom:67%;" />

在本地端運行MySQL服務(這邊依官方安裝，後續在run會有問題)，所以遵循網路上
https://anntiz.github.io/2019/06/19/use-mariadb-10-4-6-with-ubuntu-18-04/：

+ **STEP 1 : 安装 `software-properties-common`**

``` bash
$ sudo apt-get install software-properties-common
```

+ **STEP 2 : 導入MariaDB gpg 密鑰(等比較久)**

```bash
# 運行以下命令將 Repository Key 添加到系統：
$ sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524170253806.png" alt="image-20210524170253806" style="zoom:67%;" />

+ **STEP 3 添加 apt 儲存庫**

  若系統中沒有add-apt-repository，請安裝，參考Ubuntu 18.04/16.04/Debian 9上安裝 add-apt-repository 的方法。

``` bash
# 導入 PGP 密鑰後，繼續添加儲存庫URL
$ sudo add-apt-repository "deb [arch=amd64,arm64,ppc64el] http://mariadb.mirror.liquidtelecom.com/repo/10.4/ubuntu $(lsb_release -cs) main"
```

+ **STEP 4 在Ubuntu 18.04 上安裝MariaDB Server**

``` bash
$ sudo apt-get update
$ sudo apt-get install mariadb-server mariadb-client
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524170358834.png" alt="image-20210524170358834" style="zoom:67%;" />

+ **STEP 5 初始化MariaDB **10.4.6

  默認情況下，新安裝的 `Mariadb` 的密碼為空，執行非管理權限的mysql命令可能出現異常

``` bash
$ mysql -u root -p

$ Enter password:
$ ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524170803549.png" alt="image-20210524170803549" style="zoom:67%;" />

+ **STEP 6 : 使用 `mysql_secure_installation` 命令初始化。**

  上述命令若是使用sudo 的mysql雖然可以登錄，但是從其他客戶端使用普通帳號連接還是有問題，所以剛安裝第一次使用`mysql_secure_installation`，將所有選項都選`y`。

  ``` bash
  $ sudo mysql_secure_installation
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524171237147.png" alt="image-20210524171237147" style="zoom:67%;" />

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524171322468.png" alt="image-20210524171322468" style="zoom:67%;" />

  + **STEP 7 : 設置 MariaDB 的遠端連接**

    初始化成功後，登入帳戶。

    設置當前數據庫。

    配置所有電腦可以透過 root : password 訪問數據庫。

    從mysql數據庫中的授權表重新載入權限。

  ``` bash
  $ mysql -u root -p
  $ MariaDB [(none)]> use mysql;
  $ MariaDB [(mysql)]> GRANT ALL PRIVILEGES ON *.* to 'root'@'%' identified by 'password';
  $ MariaDB [(mysql)]> flush privileges;
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524171907905.png" alt="image-20210524171907905" style="zoom:67%;" />

  ​	**編輯 MariaDB 10.4 的配置文件： /etc/mysql/my.cnf ，找到 “bind-address = 127.0.0.1” ， 這一行註解掉。**

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524172653506.png" alt="image-20210524172653506" style="zoom:67%;" />

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524172812036.png" alt="image-20210524172812036" style="zoom:67%;" />

  + **STEP 8 : 重啟 mysql 服務**

  ``` bash
  $ sudo service mysql restart
  ```

  + **STEP 9 :　建立新的database**

  ``` bash
  $ mysql -u root -p
  $ MariaDB [(none)]> CREATE DATABASE almond;
  $ MariaDB [(none)]> SHOW DATABASES;
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524173200585.png" alt="image-20210524173200585" style="zoom:67%;" />

  + **STEP 10 : 切換DB，加入遠端使用者連接權限**

  ``` bash
  $ MariaDB [(none)]> use almond;
  $ MariaDB [almond]> GRANT ALL PRIVILEGES ON *.* to 'user'@'%' identified by 'password';
  $ MariaDB [almond]> flush privileges;
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524173604409.png" alt="image-20210524173604409" style="zoom:67%;" />

  

## STEP 2 安裝Cloud-Almond

+ 使用yarn安裝

``` bash
$ yarn global add github:stanford-oval/almond-cloud
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524173715700.png" alt="image-20210524173715700" style="zoom:67%;" />

+ 先更新node版本並安裝npm，在安裝yarn(參考：https://ithelp.ithome.com.tw/articles/10194339)。

``` bash
$ sudo apt  install curl
$ curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
$ sudo apt-get install nodejs
$ node -v
$ npm -v
$ sudo apt install build-essential
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524174145587.png" alt="image-20210524174145587" style="zoom:67%;" />

``` bash
$ curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
$ echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
$ sudo apt update
$ sudo apt install yarn
$ yarn -v
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524174350425.png" alt="image-20210524174350425" style="zoom:67%;" />

``` bash
#再輸入一次yarn global add github:stanford-oval/almond-cloud 會出現錯誤
#需安裝git
$ sudo apt-get install git
$ sudo yarn global add github:stanford-oval/almond-cloud
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524175049294.png" alt="image-20210524175049294" style="zoom:67%;" />

## STEP 3 設定環境檔

+ 通過編輯`config.js`文件來選擇操作方式

  (路徑：/usr/local/share/.config/yarn/global/node_modules/almond-cloud/config.js)。

  必須首先將設置`DATABASE_URL`為指向您的MySQL服務器。格式為：

  ``` 
  mysql://<user>:<password>@<hostname>/<db_name>?<options>
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524175548103.png" alt="image-20210524175548103" style="zoom:67%;" />

## STEP 4 Database

​	To bootstrap Almond, execute:

```bash
$ almond-cloud bootstrap
```

## STEP 5 WEB ALMOND

+ Web Almond由一個主進程和一些工作進程組成。要啟動主進程，請創建一個工作目錄，例如`/srv/almond-cloud/workdir`，然後執行以下操作：

  ```bash
  # cd /srv/almond-cloud/workdir ; almond-cloud run-almond
  $ sudo mkdir /srv/almond-cloud
  $ cd /srv/almond-cloud
  $ sudo mkdir workdir
  $ cd workdir
  $ almond-cloud run-almond
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524175825468.png" alt="image-20210524175825468" style="zoom:67%;" />

  ## STEP 6 WEB 前端

  + 可以通過以下方式在與主進程相同的工作目錄中運行Web前端：

    如果`--port`未指定，則默認為8080。

  ```bash
  $ sudo almond-cloud run-frontend --port ...
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524180727356.png" alt="image-20210524180727356" style="zoom:67%;" />

  + **因為執行上述命令有誤，所以參考https://community.almond.stanford.edu/t/problem-with-deploying-web-almond/416/3**

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524180829111.png" alt="image-20210524180829111" style="zoom:67%;" />

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524180846983.png" alt="image-20210524180846983" style="zoom:67%;" />

  ``` bash
  # 先將密鑰隨機生成
  # AES_SECRET_KEY必須精確為128位（16字節），格式為32個十六進製字符。SECRET_KEY和JWT_SIGNING_KEY可以是任何格式，只要它們以十六進制格式設置即可。建議選擇128或256位隨機字符串。
  # 在Linux上，這是一個簡單的命令，用於生成32的強隨機密鑰：
  $ dd if=/dev/random of=/dev/stdout bs=32 count=1 | od -t x8 -w
  > 0000000 fd78e0b9b6119638 e712fba802709a08 4262dbdd0b183e0c 3793e15f23ebf94a
  > 0000040
  > 輸入 1+0 個紀錄
  > 輸出 1+0 個紀錄
  > 32 bytes copied, 0.00021305 s, 150 kB/s
  
  # 將其中一半用於AES_SECRET_KEY（128位），將全部64個字符用於其他兩個（要清楚使用三個不同的字符串）
  ```

  <img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524181402572.png" alt="image-20210524181402572" style="zoom:67%;" />

+ **然後在 run 一次web前端執行命令**

``` bash
$ sudo almond-cloud run-frontend
```

<img src="C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20210524181603028.png" alt="image-20210524181603028" style="zoom:67%;" />

