# Clean up swap safe

To find out if anything is swapping in/out you can use the ```vmstat``` command. Leave it running a few seconds to settle down and watch the ```si``` (swapin) and ```so``` (swapout) columns. If nothing is happening then there is no reason to be concerned. 

```bash
watch vmstat
```

To make swap free
```bash
swapoff -a
swapon -a
```

Tags:
```
#Linux #swap #vmstat #swapoff
```

Related:
```
* https://askubuntu.com/questions/1357/how-to-empty-swap-if-there-is-free-ram
```
