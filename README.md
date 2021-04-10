# exsh
Execlineb but interactive (bad!)

different things:

interactive mode variables

they are stored in /tmp/exsh (i know its insecure but i havent found better way of doing it) and because of that they are shared between all instances of exsh

setvar functions sets variable that is shared between multiple commands, for example:
`setvar a b` and then running `echo $a` will print b
unlike `define` it cant execute command directly after it, and define unlike `setvar` cant share variables between multiple commands in interactive mode

reset resets all interactive mode variables

customizing:

if ~/.exshrc exists, exsh will instead of printing prompt execute ~/.exshrc and uses its outpu as prompt

example configuration can look like:

```
importas user USER
importas pwd PWD
printf "\033[38;2;0;255;127m:${user}:${pwd}:$ \033[0;0m"
```

more?

i plan to implement function like thing that will cover both functions and aliases and also i plan to implement proper edit line stuff and history
