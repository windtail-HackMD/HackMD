# Docker NetWork

查看本機 docker 網路列表
```
docker network ls
```

- Bridge Network (橋接網絡)
- Host Network（主機網路）
- None Network（無網路）

![image](https://hackmd.io/_uploads/S1roH-9_Jl.png)


Container (容器)的詳細資訊，例如：使用哪個網路、環境變數
```
docker inspect [container名稱]
```
![image](https://hackmd.io/_uploads/Ska0wZ9OJl.png)
![image](https://hackmd.io/_uploads/r1vzdb9dke.png)

## 建立、刪除網路
建立網路
```
docker network create [名稱]
```
![image](https://hackmd.io/_uploads/ByKeqW5ukl.png)

刪除網路
```
docker network rm [名稱]
```
![image](https://hackmd.io/_uploads/H1SPqWq_Jl.png)


##  連接、斷開container natwork
連接 Network（網路）與 Container (容器)
```
docker network connect <network_name> <container_id_or_name>
```
![image](https://hackmd.io/_uploads/HkXb3bcO1g.png)
![image](https://hackmd.io/_uploads/SkvYhb5_Jx.png)
![image](https://hackmd.io/_uploads/B1qq3-qOke.png)

斷開 Network（網路）與 Container (容器) 的連接
```
docker network disconnect <network_name> <container_id_or_name>
```
![image](https://hackmd.io/_uploads/HywIRW5dkl.png)
![image](https://hackmd.io/_uploads/SJNv0bcu1x.png)
![image](https://hackmd.io/_uploads/r1JYAWqO1g.png)



## 參考資料
https://hackmd.io/@Hualiteq/H15Vrl5F_
