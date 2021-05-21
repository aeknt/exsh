# exsh
Execlineb but interactive (wip)

different things:

interactive mode variables

they are stored using enviroment variables, namely $vars which have saved what variables should be autoimported

%var function handles interactive mode variables:

`%var -s a b` sets a as b

`%var -x b uname -l` sets b to output of uname -l

`%var -i var var` is like `importas var var`

`%var -e b c` sets enviroment variable b to c

`%var -u a` unsets variable a

`%var -ue b` unsets enviroment variable b

unlike `define` it cant execute command directly after it, and define unlike `var` cant share variables between multiple commands in interactive mode

~~sadly you cant yet do things such as:~~

~~`ifthenelse { test -f /etc/doas.conf } { var -s doas yes } { var -s doas no }` in interactive session, because functions sucks and trying to extract `var` to its own script wont fix it entirely because it execs another instance of exsh when defining vars.~~

edit: you can do that because %var is handled as substitution

customizing:

~/.exshrc can be written in any lang as long as ~/.exshrc prints wanted $PS1 when ran without args, when ran with arg function then print all defined functions and when ran with fn <function> then execute given function

example configuration can look like:
```
#!/usr/bin/env execlineb

#do some stuff to make this work
multisubstitute { importas first 1 importas fn 2 }
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

or `exsh -i` will do it automatically

similiar work:

sysvinit's https://github.com/sysvinit/execshell
