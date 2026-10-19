**把Dev分支的全部内容提交到main分支，但是不附带dev中提交信息，只需要所有文件的修改内容**

点击【取消】 关闭当前的弹窗。

在 SourceTree 界面右上角，点击 “终端” (Terminal) 按钮。

在弹出的黑色命令行窗口中，输入以下命令并回车（直接复制即可）：

Bash
git merge --squash dev
(这条命令的意思是：把 dev 的所有修改拿过来，捏在一起，放在暂存区，但不生成提交，也不移动指针)

关闭终端窗口。

回到 SourceTree 界面，稍等几秒，你会看到 “文件状态” 里出现了一大堆“已暂存”的文件。

这时候点击左上角的 “提交”，输入 V1.3，然后提交。

/verify  CK-FXMA-YW8H-EVNC 19074967774@163.com 670BmRNZNjko l3tcwlieuotplhd4uomarjxkcykpekk3
/upgrade ec1861f6812c6f1d550195be NXE7-MMLT-TTXH-SGUE

**把Dev分支内容合并到Main。丢弃Main中的内容以Dev为主**

1. 切换到 Main 分支（确保你站在接收内容的一方）
   git checkout main
2. 发起合并，把 Dev 的内容拉过来（此时会提示冲突 conflict）
   git merge dev
3. 遇到冲突！执行这行命令，把所有冲突文件直接变成 Dev（theirs）的内容
   git checkout --theirs .
4. 把这些解决完冲突的文件重新添加到暂存区
   git add .
5. 完成这次合并提交
   git commit -m "merge: 合并 Dev 分支，冲突部分完全保留 Dev 的内容"
6. 将合并后的 Main 分支推送到远程仓库
   git push origin main


**获取某一个提交的内容到当前的分支**

在要合并的分支执行

git cherry-pick -n 提交ID


**Cocos插件问题**

rm -Recurse -Force node_modules, package-lock.json
npm cache clean --force
npm install

npm install typescript --save-dev
