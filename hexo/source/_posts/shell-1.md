---
title: shell复习-1 基础内容
date: 2019-01-22 23:25:40
tags:
	- shell
categories: 代码
archives: /archives
---

### bash,csh,sh等bash解释器区别

- sh(Bourne shell),最老的shell，也是最基础的
- bash(Bourne Again Shell),Linux 默认，shell的扩展版本，包含部分csh和ksh的特性
- csh，语法和c比较类似
- tsch，c shell的扩展版本，支持补全，校正等便捷操作
-ksh(korn shell),结合csh和bash特点兼容bash
<!-- more -->
### bash基础相关

- 输入相关
  `
  	< filename(文本输入)
  	<< stop(持续输入知道遇到stop)
  	read param(接受控制台读取一个参数)
  	0 (标准输入描述符)
  `
- 输出相关
  `
  	> filename(文本输出,覆盖)
  	>> filename(追加到文本文件)
  	2>&1(把错误输出重定向到标准输出)：find xx > test.log 2>&1
  	1 (标准输出描述符)
  	2 (错误输出描述符)
  `
- 变量
	**可以通过下面知道$ # @ * 这些符号含义在变量中基本是一致的**
  1) 普通变量
  `
  	var=xx|var="xx" (赋值)
  	$var|${var}|"$var" (注意单引号会使得$无效,${}可以作为正则表达式)
  	$* $@ $# #? (* 表示所有参数取值,视为一个参数五区段。 @还存在着区段，#为参数数量,?为上个脚本执行结果)
  	$0|$1···(0表示脚本自己，1等表示参数下标,两位书以上需要用{})
  	( ) (类似 · · 和 eval,执行群组指令获取结果，但是()也可用于数组定义)
  	(( )) (类似let，bash内建指令，算术计算)
  	{ } (单纯只使用大括号时，作用就像是个没有指定名称的函数一般,另一种用法常用在字串的组合
  	mkdir {userA,userB,userC}-{home,bin,data}会创建两个大括号的正交组合的目录 )
  	[ ] (判断内容，类似test，[ $a -eq $b ],数值 -eq -ne -lt -le -gt -ge,文件 -r -l -w -x -f -e -d -s -nt -ot,字符 = ！= -n -z ,逻辑 -a -o !)
  	[[ ]] (判断，可以直接 $a==$b 的方式使用)
  `
  2) 数组变量
  `
  	array=(1 2 3)|readarray array (数组赋值)
  	${array[@]}|${array[@]:1:4}|${array[1]} (取所有值|取1-4下标的值|取下标1的值)
  	${#aray[@]} ${#array[*]} (获取数组长度)
  	array[1]=xx (数组赋值)
  	unset array[1] unset array (清除下标1的数组值)
  `
- 逻辑判断
	 **逻辑判断主要包括条件判断，多分支**
  1)if判断
   *if []需要空格分离,[]中间也需要空格*
  `
  	if [ $a -eq $b ];then echo $a ;elif [ -f $c ];then echo $c;else echo $b;fi
  `
  2)case判断
  `
  	case $opt in
  		$a) xx
  		;;
  		$b | $c ) xx
  		;;
  		*)
  		;;
  	esac
  `
- 循环语句
  1)for循环
  	for i in $(seq 1 10) | for ((i=0;i<10;i++)) | for i in {1..10}
  	do
  	xx
  	done
  2)while循环
  	while :;
  	do
  		xx
  	done
  3)until循环
  		count=1
  		until [[ $count == 10 ]]
  		do
  			let count+=1
  		done
  4)break 和continue
  	break n （跳出几层循环，n=2时即会退出2层循环)
  	continue n (中断几层循环，n=2时会中断2层循环重新开始内两层的循环)
- 脚本内调用其他脚本的方式
  1)fork
  	fork是最普通的, 就是直接在脚本里面用/directory/script.sh来调用script.sh这个脚本. 
  	运行的时候开一个sub-shell执行调用的脚本，sub-shell执行的时候,parent-shell还在。
  	sub-shell执行完毕后返回parent-shell. sub-shell从parent-shell继承环境变量.
  	但是sub-shell中的环境变量不会带回parent-shell
  2)exec
  	执行子级的命令后，不再执行父级命令。exec与fork不同，不需要新开一个
  	sub-shell来执行被调用的脚本.  被调用的脚本与父脚本在同一个shell内执行。
  	但是使用exec调用一个新脚本以后, 父脚本中exec行之后的内容就不会再执行了。
  	这是exec和source的区别
  3)source
  	执行子级命令后继续执行父级命令，同时子级设置的环境变量会影响到父级的环境变量。
  	fork的区别是不新开一个sub-shell来执行被调用的脚本，而是在同一个shell中执行. 
  	所以被调用的脚本中声明的变量和环境变量, 都可以在主脚本中得到和使用.
- 后台运行脚本的几种方式
  1) nohup
  2) { }&
  3) jobs,fg,bg
