# Docker安裝

docker hub
https://hub.docker.com/

## Win作業系統安裝方式
1. 開啟或關閉Windows功能
![圖片一](https://hackmd.io/_uploads/SkZ6EYDd1l.png)

2. 確認[Window Hypervious平台]、[Window子系統Linux版]已啟動
![圖片一](https://hackmd.io/_uploads/rJkEDYP_Je.png)

3. 官網下載軟體安裝


## Linux作業系統安裝方式
### 安裝
1. 先檢查有無安裝
```
//檢查指令
rpm -qa|grep docker
```
![image](https://hackmd.io/_uploads/BkqZscduJl.png)

2. 開瀏覽器輸入網址https://download.docker.com/
![image](https://hackmd.io/_uploads/SJyqi5Ou1g.png)

3. 選linux-->centos-->對docker-ce.repo右鍵複製網址
![圖片一](https://hackmd.io/_uploads/S12Ah9_uJg.png)

4. 加入檔案庫
```
dnf config-manager --add-repo=網址
```
![image](https://hackmd.io/_uploads/By81eid_ke.png)

5. 安裝
```
dnf install docker-ce
```

### 啟動
1. 啟動指令
```
systemctl start docker
```

2. 運行 "hello-world"測試 
```
docker run hello-world
```
![image](https://hackmd.io/_uploads/SJ9DNsdOJg.png)

PS. 關於docker相關檔案放在/var/lib/docker
![image](https://hackmd.io/_uploads/ryxa4ou_Jl.png)


## 參考資料
### Linux
https://www.aurotek.com.tw/uploads/files/hello.pdf