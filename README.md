# exsh
Execlineb but interactive (bad!)

different things:

interactive mode variables

they are stored in /tmp/exsh (i know its insecure but i havent found better way of doing it) and because of that they are shared between all instances of exsh

setvar functions sets variable that is shared between multiple commands, for example:

`setvar a b` and then running `echo $a` will print b,

unlike `define` it cant execute command directly after it, and define unlike `setvar` cant share variables between multiple commands in interactive mode

reset resets all interactive mode variables

customizing:

~/.exshrc can be written in any lang as long as ~/.exshrc prints wanted $PS1 when ran without args, when ran with arg function then print all defined functions and when ran with fn <function> then execute given function

example configuration can look like:
```
#!/usr/bin/env execlineb
importas first 1
importas fn 2
shift shift
backtick all { dollarat -d " " }
importas -s all all

#some basic prompt showing user and hostname
ifelse { test -z $first } {
	importas user USER
	backtick hostname { hostname }
	importas hostname hostname
	foreground { printf "\033[38;2;0;255;127m${user}@${hostname}$ \033[0;0m" } }

#list all functions
ifelse { test $first = functions } {
echo "
hello
print
ls
" }

#function definitions
if { test $first = fn } 
	#when running hello, run dollarat
	ifelse { test $fn = hello  } { dollarat }
	#print is alias to echo
	ifelse { test $fn = print  } { echo $all }
	#always run ls with -l flag
	ifelse { test $fn = ls  } { ls -l $all }
	#when running nondefined function, echo this:
	echo no function defined
```

more?


you can use https://github.com/hanslub42/rlwrap with exsh: `rlwrap -c exsh` to get proper line editing, history and tab completation
