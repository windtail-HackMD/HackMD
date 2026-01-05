# 建立dockerfile範例2
介紹dockerfile

## 建立 Dockerfile
建立資料夾
```
mkdir docker-test
cd docker-test
```
將需要的資源(如 JDK java8)，放在docker-test
![圖片一]([https://hackmd.io/_uploads/S1L0m77nke.png)

建立Dockerfile
```
FROM ubuntu:20.04

LABEL maintainer="bill your_email@example.com"

# 安裝 wget 和 tar 工具
RUN apt-get update && apt-get install -y wget tar

# 使用 COPY 指令將 JDK 8 文件複製到 /opt 目錄
COPY OpenJDK8U-jdk_x64_linux_hotspot_8u442b06.tar.gz /opt/

# 檢查 JDK 文件是否被正確 ADD 到容器中
RUN ls -l /opt/

# 使用 tar 命令解壓縮 JDK 文件
RUN tar -xzvf /opt/OpenJDK8U-jdk_x64_linux_hotspot_8u442b06.tar.gz -C /opt/

# 確認解壓後的目錄
RUN ls /opt/

# 根據實際情況調整 JDK 目錄名稱(先用RUN ls /opt/指令顯示解壓縮後名稱)
RUN mv /opt/jdk8u442-b06 /opt/jdk1.8.0

# 安裝 Tomcat
RUN wget https://archive.apache.org/dist/tomcat/tomcat-7/v7.0.82/bin/apache-tomcat-7.0.82.tar.gz
RUN tar zxvf apache-tomcat-7.0.82.tar.gz -C /opt/

# 設置環境變數 JAVA_HOME 和 PATH
ENV JAVA_HOME=/opt/jdk1.8.0
ENV PATH=$PATH:/opt/jdk1.8.0/bin

# 設置工作目錄為 Tomcat 目錄
WORKDIR /opt/apache-tomcat-7.0.82

# 設置容器啟動時運行 Tomcat
CMD ["bin/catalina.sh", "run"]
```
![image](https://hackmd.io/_uploads/Bk1oE772yl.png)

## Build Docker Image

-t mytomcat: -t 是標籤（tag）的縮寫，這裡的 mytomcat 是為新建立的映像檔指定的名稱（tag）。可以根據需要更改成不同的名稱或版本號，例如 mytomcat:v1。

.: 這個點代表當前目錄。Docker 會在當前目錄中尋找 Dockerfile，並根據其中的指令來建立映像檔。

--no-cache: 這個選項告訴 Docker 在建構過程中不要使用任何緩存。通常 Docker 會利用緩存來加速映像檔的建立，這樣就能跳過一些不需要變動的步驟。加上 --no-cache 之後，Docker 每次都會從頭開始建立映像檔，不會使用先前的緩存。
```
docker build -t my-tomcat . --no-cache
```

確認新建Image
```
docker images
```
![image](https://hackmd.io/_uploads/ryw8-CX3kx.png)

## 使用Image，建立Container
```
docker run -d -p 8080:8080 my-tomcat
docker ps
```
![image](https://hackmd.io/_uploads/HkPwQR73kl.png)

外部筆電測試http://127.0.0.1:8080/
![image](https://hackmd.io/_uploads/SyGK7CX2ke.png)

## 參考資料
https://ithelp.ithome.com.tw/articles/10191016
