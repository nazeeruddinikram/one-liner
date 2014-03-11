Line spacing
========

* Append a blank line to each line

    ```
        sed G
        awk '1;{print ""}'
        perl -ne 'print "$_\n"'
    ```


* Prepend a blank line to each line

    ```
        sed '{x;p;x}'
        awk '{printf("\n%s\n",$0)}'
        perl -ne 'print "\n$_"'
    ```

* Delete blank lines

    ```
        sed '/^$/d'
        awk NF
        perl -ne 'print if(!/^$/)'
    ```

Numbering
========

* Number each line of a file

    ```
        sed = filename | sed 'N;s/\n/\t/'
        awk '{print NR "\t" $0}'
        perl -pe 'print "$.\t"'    OR    perl -ne 'print "$.\t$_"'
    ```

* Count lines

    ```
        sed -n '$='
        awk 'END {print NR}'
        perl -nle 'END {print $.}'
    ```

Text substitution
========

* Delete leading whitespace from each line

    ```
        sed 's/^[ \t]*//'
        awk '{sub(/^[ \t]*/,"")} 1'
        perl -ple 's/^\s*//'
    ```

* Delete both leading and trailing whitespace from each line

    ```
        sed 's/^[ \t]*//;s/[ \t]*$//'
        awk '{sub(/^[ \t]*/,"");sub(/^[ \t]*/,"")} 1'
        perl -ple 's/^\s*(.*)\s*$/\1/'
    ```

* Insert 5 blank spaces at the begining of each line

    ```
        sed 's/^/     /'
        awk '{printf("     %s\n",$0)}'
        perl -pe 'print " "x5'
    ```

* Substitute "foo" with "bar" on each line

    ```
        sed 's/foo/bar/'  # replaces only 1st instance
        sed 's/foo/bar/4' # replaces only 4th instance
        sed 's/foo/bar/g' # replaces all instances
        sed 's/\(.*\)foo/\1bar/' # replace the last instance
        sed 's/\(.*\)foo\(.*foo\)/\1bar\2/' # replace the next-to-last instance

        awk '{gsub(/foo/,"bar")} 1'

        perl -pe 's/foo/bar/' # replace only 1st instance
        perl -pe 's/foo/bar/g' # replace all instances
        perl -pe 's/(.*)foo/\1bar/' # replace the last instance
        perl -pe 's/(.*)foo(.*foo)/\1bar\2/' # replace the next-to-last instance
    ```

* Substitute "foo" with bar only for lines which contains "baz"

    ```
        sed '/baz/s/foo/bar/g'
        awk '/baz/ {gsub(/foo/,"bar")} 1'
        perl -pe 's/foo/bar/g if /baz/'
    ```

* Substitute "foo" with bar except for lines which contains "baz"

    ```
        sed '/baz/!s/foo/bar/g'
        awk '!/baz/ {gsub(/foo/,"bar")} 1'
        perl -pe 's/foo/bar/g unless /baz/'
    ```

* Change "a" or "b" or "c" to "d"

    ```
        sed 's/a/d/g;s/b/d/g;s/c/d/g'
        sed 's/a\|b\|c/d/g' # GNU sed only
        awk '{gsub(/a|b|c/,"d")} 1'
        perl -pe 's/a|b|c/d/g'
    ```

* Reverse order of lines (emulates "tac")

    ```
        sed '1!G;h;$!d'
        awk '{a[i++]=$0} END {for(j=i-1;j>=0;j--) print a[j]}'
        perl -e 'print reverse <>'
    ```

* Reverse each chracter on the line (emulates "rev")

    ```
        sed '/\n/!G;s/\(.\)\(.*\n\)/&\2\1/;//D;s/.//'
        awk '{for(i=length($0);i>0;i--) printf("%s",substr($0,i,1));print ""}'
        perl -ple '$_=reverse'
    ```

* Join pairs of lines side-by-side (emulates "paste")

    ```
        sed '$!N;s/\n//'
        awk '{f=!f; printf("%s%s",$0,f ? "" : "\n")}'
        perl -pe 's/\n// if $.%2'
    ```
