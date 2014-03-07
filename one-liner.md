* Append a blank line to every line
```
    sed G
    awk '1;{print ""}'    # At default, function print would provide 'ORS'
    perl -ne 'print "$_\n"'
```
