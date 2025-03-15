# 建立dockerfile範例1
此編先以Ubuntu OS為基底，手動安裝設定各種軟體，下篇介紹dockerfile把剛才手動這些過程自動化。

1. 從 Docker Hub 下載 Docker Image 到 local
2. 使用 Docker Image 啟動 Docker Container 並進入 Docker Container的terminal
3. 在 Docker Container 裡面安裝 Apache 的 HTTP Service，並且寫一個 helloworld 的 html 檔
4. 使用 Browser 連到 helloworld.html 確認 Docker Container 有成功的被啟動起來

## 下載 Ubuntu OS
```
docker search ubuntu
```
![image](https://hackmd.io/_uploads/HkVX9oGhkg.png)
```
docker pull ubuntu
```
![image](https://hackmd.io/_uploads/BkE4ojf21g.png)

## 進入容器
```
docker run -it -p 8080:80 --name myUbuntu ubuntu
```
![image](https://hackmd.io/_uploads/SJsql3Gh1e.png)

安裝和啟動 apache 的 http service ，指令如下
```
apt-get update
apt-get install -y apache2
service apache2 start
```
簡單寫一個 hellowolrd.html檔案放在 /var/www/html 的路徑下，指令如下
```
echo "HelloWorld" > /var/www/html/helloworld.html
```
使用cat /etc/hosts指令查看 docker container 的 IP 
```
cat /etc/hosts
```
![image](https://hackmd.io/_uploads/rJrYN2fnkx.png)

使用 browser 輸入 http://127.0.0.1:8080/helloworld.html
![image](https://hackmd.io/_uploads/BJVDnnGhkl.png)

## 參考資料
https://ithelp.ithome.com.tw/articles/10190921
https://ithelp.ithome.com.tw/articles/10191016
