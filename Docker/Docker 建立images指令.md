# Docker 建立images指令
這個 Dockerfile 我們就可以把它想像成這個模型的說明書，這本說明書包含了一系列指令，告訴 Docker 如何建立一个映像。當我們要 run docker 時，他會根據現在專案的根目錄，找尋 Dockerfile ，並且根據 Dockerfile 裡面一步一步的說明，來安裝好相關套件資料庫等環境，然後製作成一個 Image(映像)。

```
### Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.7.3-stretch

# Set the working directory to /app
WORKDIR /app

# Install any needed packages specified in requirements.txt
COPY requirements.txt ./
COPY gunicorn.py ./
RUN pip install --trusted-host pypi.python.org -r requirements.txt \
    && python3 -O -m compileall -b ./app \
    && find ./app -name "*.py"|xargs rm -rf \
    && python3 -O -m compileall -b ./config.py \
    && rm ./config.py

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["gunicorn", "-c", "gunicorn.py", "run:app"]
```

## Dockerfile 裡面的關鍵字
### FROM 
也就是我們在 Docker Hub 上可以看到的基礎鏡像版本
### AS 
我們可以把它想像成暱稱，也就是我們可以在這個檔案後面以 app 這個暱稱來稱呼 ruby:3.2.2
```
FROM ruby:3.2.2 AS app
```
### MAINTAINER(目前建議使用 LABEL)
Dockerfile 的作者/維護者（1.13 版本以後，建議改用LABEL）
```
MAINTAINER Krystal <krystal@example.com>
```
### LABEL
以 key value 的形式，來描述鏡像的屬性(作者、版本、描述等)
```
LABEL maintainer="Krystal <krystal@example.com>"
LABEL version="1.0"
LABEL description="My rails project"
LABEL website="https://www.example.com"
```

### RUN 
1. 建立 image 時，安裝軟體套件、下載依賴項、設定環境。
2. 每個 RUN 指令都會生成一層新的鏡像層疊上去，所以根據下面範例會生成兩層
(也代表每個RUN指令都是各自獨立)
```
RUN apk add --update --no-cache postgresql-dev

RUN gem install bundler:2.3.19 &&  bundle install -j4 --retry 3 && bundle clean --force
```
PS. && 其實就是如果前一個命令執行成功，才會執行下一個命令。如果前一個命令執行失敗，那後面的命令就不會執行。

### CMD
1.容器啟動時要執行的預設命令，每個 Dockerfile 只能有一個 CMD 指令
2.容器啟動時，若用戶提供了CMD命令，則會覆蓋容器的 CMD。
```
//範例: CMD ["echo", "Hello, Docker!"]
//測試: docker run test-cmd echo "Hi!"     # 覆蓋 CMD，輸出: Hi!
CMD ["<command>", "<arg1>", "<arg2>"]
```

### ENTRYPOINT
1.定義容器的主要執行命令，且會在容器啟動時執行
2.即使用戶提供命令行參數，也不會覆蓋 ENTRYPOINT 的命令，而是將參數附加在 ENTRYPOINT 之後。
```
//範例: ENTRYPOINT ["echo", "Hello, Docker!"]
//測試: docker run test-entrypoint "Hi!"     # 輸出: Hello, Docker! Hi!
ENTRYPOINT ["<command>", "<arg1>", "<arg2>"]
```

### ENTRYPOINT 跟 CMD 的差別
![image](https://hackmd.io/_uploads/ryQWvPo_yx.png)
### ENTRYPOINT 結合 CMD 用法
CMD 的內容被設計為 ENTRYPOINT 的預設參數，兩者一起形成單一指令執行。
```
//以下意思是 不傳入參數情況下啟動後先sleep 10秒
FROM ubuntu
MAINTAINER howhow

ENTRYPOINT ["sleep"]
CMD ["10"]
```


### EXPOSE
指出容器內應用程式，應該使用哪些 port (連接口)進行監聽
```
EXPOSE 3000
```

### ENV
在容器內部設定的環境變數
(是容器啟動時，也可以使用的變數。)
```
//設定一個環境變數叫做 DB_HOST ， value 是 
//範例: ENV DB_HOST=postgresql
ENV <参数名>[=<默认值>]
```
### ARG
在 Dockerfile 中使用的參數，與 ENV 作用很類似，但是作用域不一樣。
ARG 設定的環境變數只有在 Dockerfile 內，及 docker build 的過程中有效
```
//範例: ARG DB_HOST=postgresql
ARG <参数名>[=<默认值>]
```

### WORKDIR
設定工作目錄
```
//範例: WORKDIR /app
WORKDIR <路徑>
```

### COPY
將文件或文件夾複製到 Image(鏡像) 裡
```
//將 app.rb 檔案複製到 app 資料夾裡
COPY app.rb /app
```
### ADD
也是將文件或文件夾複製到 Image(鏡像) 裡，除了複製外，還支援自動解壓縮檔案、URL 下載和複製上下文的檔案
```
//自動解壓縮 https://example.com/file.gz 這個檔案，並下載下來到 app 目錄內
ADD https://example.com/file.gz /app/
```

### VOLUME
可以建立一個或多個永久性儲存的資料夾，內容不會隨容器的生命週期結束而消失，裡面的資料可以在容器與容器間共享。
但 VOLUME 裡面的資料並不是存在容器裡，是存在主機的文件裡。
```
//建立一個 mydata 資料夾，可以把需要一直存在的資料放進來。
/*

這可以應用在資料庫，把資料儲存在主機

*/
VOLUME /mydata
```

### USER
1.指定用哪個使用者身分來執行容器中的命令
2.優點是可以限制容器內部使用者，根據身分來有不同權限執行，可以增加容器的安全性。
```
//範例: USER krystal:baby_team
USER <用户名稱或用户 ID>:<群組名稱或群组 ID>
//也可以單純指定 user
USER krystal
```

### ONBUILD
![image](https://hackmd.io/_uploads/BJuifPj_1g.png)

![image](https://hackmd.io/_uploads/By0dGvid1e.png)


```
/*
    ONBUILD 兩句依序的意思是：

    1.將原本映像的 install_app 複製到新映像的 /app/ 目錄中。
    2.執行新映像中的 /app/install_app。
*/

//這個功能對應現實情境，像PROD環境上版時先將就程式版本備份
FROM ubuntu:latest

ONBUILD ADD install_app /app/
ONBUILD RUN /app/install_app
```

### STOPSIGNAL
如其名，是容器在接收到停止時，需要做的動作，
```
STOPSIGNAL SIGTERM
```
預設是 SIGTERM ，就是單純的停止。
```
//9 就是 立即強制停止。
STOPSIGNAL 9
```

### HEALTHCHECK
可以在容器內部執行一些自訂的指令或檢查，來檢查容器的健康狀態。
```
--interval=30s
1.這表示健康檢查的 時間間隔 為30秒，也就是每30秒健康檢查一次。
2.預設為 30 秒。

--timeout=10s
1.如果健康檢查時間超過10秒，則為失敗。
2.預設為 30 秒

--start-period=90s
1.通常容器啟動會需要一些時間，上面的意思就是容器啟動後再等 90 秒，再開始健康檢查。
2.預設為 0 秒

--start-interval=10s
1.健康檢查之間的間隔時間（即兩次健康檢查的頻率）
2.預設為 5 秒

--retries=5
1.表示若是健康檢查為不健康時，會再重試幾次。
如範例就是如果在 5 次連續的健康檢查中都失敗了，容器將被標記為不健康。
2.預設為 3 次

HEALTHCHECK --interval=30s --timeout=10s --start-period=90s --retries=5 CMD [ "ruby", "health_check.rb" ]
```


### SHELL
在 Dockerfile 中執行命令時使用的預設 shell，通常是用在 Windows 上要基於 Linux 的容器映像時，可以將預設 shell 變為 Bash。
```
SHELL ["/bin/bash", "-c"]
```