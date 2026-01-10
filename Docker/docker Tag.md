# tag
## 建立自己的 Tags(標籤)
看一下自己現在本機所有的 Image(映像)
![圖片一](https://hackmd.io/_uploads/SkZ6EYDd1l.png)
建立一個有新標籤的 Image(映像)
```
docker tag <本機現有映像 id_or_repository>:<本機現有標籤> <新映像 repository>:<新標籤>
```
PS. Tags(標籤)可以包括字母、數字、橫線(-)和下劃線(_)，但不建議使用特殊符號(.、:、/等)
![圖片一](https://hackmd.io/_uploads/r12GPMj_Jl.png)

## 建立＋推送(push)自己的 Tags(標籤)
```
docker tag <本機現有映像id_or_repository> <Docker 儲存庫的主機名或 IP 地址>:<端口>/<新映像 repository>:<新標籤>
```
