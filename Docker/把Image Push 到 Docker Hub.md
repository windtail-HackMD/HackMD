# 把Image Push 到 Docker Hub
接續[建立dockerfile範例2](/LXFfgtE5S7mxjglqLsU11w)
說明如何把Image Push 到 Docker Hub

```
docker images
```
![image](https://hackmd.io/_uploads/SJmA-2S3Je.png)


## 建立＋推送(push)自己的 Tags(標籤)
PS. Tags(標籤)可以包括字母、數字、橫線(-)和下劃線(_)，但不建議使用特殊符號(.、:、/等)
```
# 指令docker tag <原本的Image Name>:<原本的Tag> DockerHub帳號/<新的Image Name>:<新的Tag>
docker tag my-tomcat:latest windtail365/my-tomcat:latest
```
![image](https://hackmd.io/_uploads/S1YiNWD3ke.png)

## 上傳
先登入
```
docker login
```
![image](https://hackmd.io/_uploads/H1-0qbvnkl.png)


上傳
```
docker push windtail365/my-tomcat
```
![image](https://hackmd.io/_uploads/HJicCWD2Je.png)

![image](https://hackmd.io/_uploads/SJpvyMw2Jg.png)


## 測試一下
先刪除本機Imagse
```
docker rm df1800ad1ab4
docker rmi windtail365/my-tomcat
docker rmi my-tomcat
docker images
```
![image](https://hackmd.io/_uploads/HJaOlMD31e.png)

從雲端下載
```
docker pull windtail365/my-tomcat
```
![image](https://hackmd.io/_uploads/Hkh1ZGD3Jg.png)


建立Container
```
docker run -d -p 8080:8080 windtail365/my-tomcat
```
![image](https://hackmd.io/_uploads/SyIH-zw2Jx.png)


在筆電上輸入 http://localhost:8080，看到網頁即成功
![image](https://hackmd.io/_uploads/SJEPZfwnkx.png)

## 參考資料
https://ithelp.ithome.com.tw/articles/10191139