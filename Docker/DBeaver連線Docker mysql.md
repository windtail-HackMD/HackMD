# DBeaver連線Docker mysql
## Docker
1.先下載並執行
```
//MYSQL_ROOT_PASSWORD設定root的密碼
docker run --name bill-mysql -e MYSQL_ROOT_PASSWORD=mysecretpw -d  -p 3306:3306 mysql
```
![image](https://hackmd.io/_uploads/BkBMTL0Okg.png)

進入容器指令
```
docker exec -it bill-mysql bash
```
![image](https://hackmd.io/_uploads/BJjoSLki1e.png)

登入mysql(使用root帳號)
```
mysql -u 使用者帳號 -p

//登出指令EXIT;
```
![image](https://hackmd.io/_uploads/rJ90HH-oyg.png)


## DBeaver
![圖片一](https://hackmd.io/_uploads/SJqc080u1x.png)
![圖片一](https://hackmd.io/_uploads/H1eaAL0u1e.png)
![image](https://hackmd.io/_uploads/B1z-lDRukx.png)
測試時，遇到如下問題，是身份驗證問題，可以加入allowPublicKeyRetrieval = true，暫時跳過資安的問題
![image](https://hackmd.io/_uploads/HykGlvA_yg.png)
![image](https://hackmd.io/_uploads/HyJ_MD0ukl.png)
![image](https://hackmd.io/_uploads/r1nOfw0OJl.png)
![image](https://hackmd.io/_uploads/H16YzwRuJg.png)

## 建立資料庫
```
CREATE database billDB;
```
![image](https://hackmd.io/_uploads/HyD6B8Wo1g.png)

## 建立帳號權限
```
//建立帳號，可遠端連線，任意IP可登入
CREATE USER 'bill'@'%' IDENTIFIED BY 'password';
```
![image](https://hackmd.io/_uploads/rJmNsr-syl.png)

- 創建角色範例
```
-- 創建一個角色，名為 'developer'
CREATE ROLE 'developer';

-- 授予角色 'developer' 讀寫資料庫的權限
GRANT CREATE, SELECT, INSERT, UPDATE, DELETE ON billDB.* TO 'developer';

-- 將角色 'developer' 賦給使用者 'john'
GRANT 'developer' TO 'bill'@'%';

-- 設定 'developer' 角色為 'bill' 的預設角色
-- 預設角色是登入後自動使用的角色(其他角色的權限不會自動啟用)
SET DEFAULT ROLE 'developer' TO 'bill'@'%';

```


- 權限授予範例
```
-- 授予使用者 John 在資料庫 `mydb` 上的所有操作權限
GRANT ALL PRIVILEGES ON billDB.* TO 'bill'@'%';
```

- 查看當前使用者的權限
```
-- 查看使用者 'john' 的所有授予權限
SHOW GRANTS FOR 'bill'@'%';
```

## 登入bill帳號創建Table
![image](https://hackmd.io/_uploads/Hy8tcUWsye.png)

登入後進行密碼變更
```
ALTER USER 'bill'@'%' IDENTIFIED BY 'password';
```

建立table
```
USE billDB;
create table app_info (
    id int auto_increment primary key ,
    name nvarchar(50),
    version nvarchar(30),
    remark nvarchar(100)
);
```
![image](https://hackmd.io/_uploads/BJgqp8bj1e.png)

建立1筆資料
```
USE billDB;
INSERT INTO app_info(name, version, remark) VALUES('app', '1.0.0', 'bill-create'); 
```
![image](https://hackmd.io/_uploads/BJS7gwWj1l.png)

