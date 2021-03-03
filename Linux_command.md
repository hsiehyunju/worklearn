# Linux 指令紀錄

## 目錄
- [刪除](#rm)
- [SVN](#svn)





### RM

+ 刪除檔案：`rm` + `filename`
+ 刪除目錄：`rm` + `-r` + `directory`
+ 刪除空目錄：`rm` + `-d` + `directory`
+ 刪除且詢問：`rm` + `-i` + `filename`
+ 強制刪除：`rm` + `-f` + `filenaame`
+ 最好別嘗試：`rm` + `-rf` 

參考網址：[https://www.opencli.com/linux/rm-delete-files-directory-command](https://www.opencli.com/linux/rm-delete-files-directory-command)

### SVN

#### 如何刪除 .svn 資料夾

到與 `.svn` 資料夾同一層目錄，使用以下指令

```shell
$ find . -type d -name ".svn"|xargs rm -rf
```

參考網址：[https://www.apharmony.com/software-sagacity/2014/10/clearing-all-svn-folders-in-windows-or-linux/](https://www.apharmony.com/software-sagacity/2014/10/clearing-all-svn-folders-in-windows-or-linux/)
