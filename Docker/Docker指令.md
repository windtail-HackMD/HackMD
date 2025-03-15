# Docker指令

docker hub
https://hub.docker.com/

- 查看Docker狀態
```
docker info
```

- DockerHub 上找image
```
docker search [名稱]
```

- 下載image
```
docker pull [名稱]
```
TAG 若沒有給的話，預設會使用 latest，因此下面這兩個指令是等價的：
```
docker pull [名稱]:latest
```
![image](https://hackmd.io/_uploads/SkytPsduJx.png)

- 列出本機全部的image
```
docker images
```
![image](https://hackmd.io/_uploads/H1SU--qdkg.png)

REPOSITORY：倉庫位置和映像檔名稱
TAG：映像檔標籤(通常是定義版本號)
IMAGE ID：映像檔ID(唯一碼)
CREATED：創建日期
SIZE：映像檔大小


- 建立並啟動Container(加入---namme為容器取名子)
```
/*
-i 是讓 container 的標準輸入保持打開
-t 選項是告訴 Docker 要分配一個虛擬終端機（pseudo-tty）並綁定到 container 的標準輸入上
--rm 當 container 主程序一結束時，立刻移除 container
-d 背景執行 container
-p IP:HOST_PORT:CONTAINER_PORT
給完整格式的話，會把 IP:HOST_PORT 綁定到 CONTAINER_PORT；
如果沒給 IP，則 IP 會代 0.0.0.0；
如果連 HOST_PORT 也沒給，則會使用 0.0.0.0 加上隨機選一個 port，如 0.0.0.0:32768
(
   例如: 主機開2個container的sqlserver可以這樣使用
   docker run -d  --name mssql1 -p 8080:1433 sqlserver
   docker run -d  --name mssql2 -p 8081:1433 sqlserver
   代表主機的8080對應mssql1的sqlserver 1433
   代表主機的8081對應mssql2的sqlserver 1433
)

*/

docker run [名稱]
```
![image](https://hackmd.io/_uploads/B1g7cj_dkg.png)

- 查詢正在執行的容器(加入-a為全部列出不管有無執行)
```
docker ps
```
![image](https://hackmd.io/_uploads/Syj4C3_O1g.png)

- 移除image
有可能會因為 container 未移除導致 image 移除失敗，先把對應的 container 移除在移除image
```
docker rmi [名稱]
```

- 移除Container
只要 container 不是處於 Up 正在執行中的狀態，即可使用 docker rm 移除
```
docker rm [docker ps 查到的 CONTAINER ID 或 NAMES。若需要移除多個 container，可以使用空白隔開每個 container 名稱]

//檢查是否成功
docker ps -a
```

## container
### 建立
```
/*

--name 可指定 container 名稱
-i|--interactive 是讓 container 的標準輸入保持打開
-t|--tty 選項是告訴 Docker 要分配一個虛擬終端機（pseudo-tty）並綁定到 container 的標準輸入上
-d|啟動後背景執行
*/

docker create -i -t --name [容器名稱] [image名稱]
```
![image](https://hackmd.io/_uploads/BJUxBa__kx.png)

### 執行
```
docker start -i [container名稱]
這個只能啟動container，如果docer run或docker create沒有加入-it、-d參數，那docker start也不能使用-it、-d參數

-i 是讓 container 的標準輸入保持打開，切換為前景模式
-t 選項是告訴 Docker 要分配一個虛擬終端機（pseudo-tty）並綁定到 container 的標準輸入上


```
![image](https://hackmd.io/_uploads/Sk0xDpddyx.png)
1.圖片中/ # 這已經在 container 的世界裡了
2.可以使用 Ctrl + P 與 Ctrl + Q 的連續組合鍵來達成「離開 container」，container仍在背景執行
3.使用docker attach進入容器當前的終端，exit指令將會連同container一起停止並離開
4.也可以使用docker exec開啟多個終端。當以 exec 進入 Container 輸入 exit 離開， Container 本身依然在背景執行，並不會被關閉
5.這個指令只能啟動container，不能也沒有修改container的參數

```
docker attach [container名稱]
```
```
docker exec -it [container名稱] bash
```

### 停止 container
主要在於 Exited 後面的狀態碼不同
1. 一種是在 container 裡下結束的指令 exit，狀態碼是0，代表正常結束。它使用正常流程 exit 指令結束 shell
2. 第二個方法使用 docker stop 會發送 SIGTERM 信號給 container 的主程序，等同於呼叫對主程序下 kill -15 指令一樣，是要求 process 強制結束。但因 process 結束遇到問題，因此才會出現不正常結束的狀態碼
```
docker stop [container名稱]
```
### 修改container
只能做以下修改
1.重啟策略（--restart）
2.CPU 限制（--cpu-shares，--cpu-period 等）
3.記憶體限制（--memory，--memory-swap）
```
//範例: docker update --restart always container_name

```
### 刪除container
```
/*
-f 如果是執行中的 container，會強制移除（使用 SIGKILL）
*/

docker rm [container名稱]
```

## 如何檢查容器內的環境變數
啟動容器後，可以用以下命令查看環境變數：
```
docker exec -it <container_id> env
```
![image](https://hackmd.io/_uploads/r1opLOhO1x.png)

## 如何查看容器的詳細資訊
```
docker inspect <container_id_or_name>
```

NetworkSettings.Networks的節點，IPAddress 顯示的是容器的內部 IP 位址，給其他容器連進來，以下範例預設網路模式（bridge 模式）
(除非特殊設定，筆電不能用這個IP連進去要使用127.0.0.1，因為docker隔離了)


```
"NetworkSettings": {
    "Networks": {
        "bridge": {
            "IPAddress": "172.17.0.2"
        }
    }
}

```
如果容器的端口被綁定到主機的端口，相關資訊可以在 NetworkSettings.Ports 節點中找到
(筆電pietty想要連線容器，Port是8080)
```
"Ports": {
    "80/tcp": [
        {
            "HostIp": "0.0.0.0",
            "HostPort": "8080"
        }
    ]
}

```


## 參考資料
https://joshhu.gitbooks.io/dockercommands/content/Containers/DockerRun.html
讓我們來玩玩Docker吧
https://ithelp.ithome.com.tw/m/users/20107537/ironman
30 天與鯨魚先生做好朋友
https://ithelp.ithome.com.tw/users/20102562/ironman/3746
30 天學習 Docker 部署你的專案
https://ithelp.ithome.com.tw/users/20151035/ironman/6311


指令總整理
https://ithelp.ithome.com.tw/articles/10199339