# Практическая работа №1

## Задача 1

``` bash
grep -o '^[^:]*' /etc/passwd | sort
```

## Задача 2

``` bash
grep -v '^#' /etc/protocols | awk 'NF {print $2, $1}' | sort -rn | head -n 5
```

## Задача 3

``` bash
#!/bin/bash

text="$1"
width=0

while IFS= read -r line
do
    if (( ${#line} > width ))
    then
        width=${#line}
    fi
done <<< "$text"

border=$(printf '%*s' $((width + 2)) '' | tr ' ' '-')

printf '+%s+\n' "$border"

while IFS= read -r line
do
    printf '| %-*s |\n' "$width" "$line"
done <<< "$text"

printf '+%s+\n' "$border"
```

## Задача 4

``` bash
#!/bin/bash

grep -oE '[A-Za-z_][A-Za-z0-9_]*' "$1" | sort -u | paste -sd ' ' -
```

## Задача 5

``` bash
#!/bin/bash

chmod 755 "$1"
sudo cp "$1" /usr/local/bin/
```

## Задача 6

``` bash
#!/bin/bash

for file in "$@"
do
    first_line=$(head -n 1 "$file")

    case "$file" in
        *.c|*.js)
            pattern='^[[:space:]]*(//|/\*)'
            ;;
        *.py)
            pattern='^[[:space:]]*#'
            ;;
        *)
            echo "$file: неподдерживаемое расширение"
            continue
            ;;
    esac

    if [[ "$first_line" =~ $pattern ]]
    then
        echo "$file: комментарий есть"
    else
        echo "$file: комментария нет"
    fi
done
```

## Задача 7

``` bash
#!/bin/bash

find "$1" -type f -exec sha256sum {} + | sort | uniq -w 64 -D
```

## Задача 8

``` bash
#!/bin/bash

find "$1" -maxdepth 1 -type f -name "*.$2" -print0 | tar --null -cf archive.tar -T -
```

## Задача 9

``` bash
#!/bin/bash

sed 's/    /\t/g' "$1" > "$2"
```

## Задача 10

``` bash
#!/bin/bash

find "$1" -maxdepth 1 -type f -empty -printf '%f\n'
```
