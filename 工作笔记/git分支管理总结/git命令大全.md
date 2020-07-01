git命令大全

- 更改分支名称

  1. 删除远端分支：

     git checkout  oldbname

     git push origin --delete oldname

  2. 更改本地分支名称：git branch -m oldname newname

  3. 推送本地分支至远端：git push origin newname:newname

- 同步服务端已删除分支和本地分支

  

  