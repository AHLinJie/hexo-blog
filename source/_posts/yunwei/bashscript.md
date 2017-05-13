---
layout: post
title: bashrc中的脚本添加内容
date: 2014-10-31
category: shell
tag: bashrc
---
**摘要:**
在bash中修改一些配置可以让开发变得更高效和方便,比如说添加下快捷命令,当前的
git branch 呀,剪短当前的目录路径显示呀等等.这里做一些记录.自己电脑可以任意
修改自定义的别名,不然的话会给比人添麻烦的.适度自由!!!有些命令有些风险建议
不适用别名的好.


####  Linux Terminal Show Git Branch Shell Script

- show git beranch
- show current dir only
- rename the promote
- alias cmd 

```
alias install="sudo apt-get install"

find_git_branch () {
    local dir=. head
    until [ "$dir" -ef / ]; do
        if [ -f "$dir/.git/HEAD" ]; then
            head=$(< "$dir/.git/HEAD")
            if [[ $head = ref:\ refs/heads/* ]]; then
                git_branch="(${head#*/*/})"
            elif [[ $head != '' ]]; then
                git_branch="((detached))"
            else
                git_branch="((unknow))"
            fi  
            return
        fi  
        dir="../$dir"
    done
    git_branch=''
}
PROMPT_COMMAND="find_git_branch; $PROMPT_COMMAND"
PS1="\u@\h:\W\$git_branch\$ "
```

