---
title: PowerShell
date: 2025-03-15
---
# PowerShell终端路径显示git分支
1. 运行`echo $PSHOME`找到PowerShell根目录，修改`Microsoft.PowerShell_profile.ps1`文件（没有则新建并修改），添加下面的代码：
```powershell
function prompt {
    $path = (Get-Location).Path
    $branch = git rev-parse --abbrev-ref HEAD 2> $null
    if ($branch) {
        $branch = " ($branch)"
    }
    "$path$branch> "
}
```

2. 保存以上文件，重启终端