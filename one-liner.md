Line spacing
========

** Append a blank line to every line **
```
    sed G
    awk '1;{print ""}'
    perl -ne 'print "$_\n"'
```

At default, function print would provide 'ORS' in awk, so we don't have to give it a '\n'.

** Prepend a blank line to every line **

    sed '{x;p;x}'
    awk '{printf("\n%s\n",$0)}'
    perl -ne 'print "\n$_"'

** Delete blank lines **

    sed '/^$/d'
    awk NF
    perl -ne 'print if(!/^$/)'
