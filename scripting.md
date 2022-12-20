# Bash scripting
- see process ID currently working in with `echo $$`
- see location of last run binary with `$0`
- make a variable an env var with the `export` command
- scripts automate repetitive tasks or codify tasks to run as a scheduled job

## Construction of a bash script
- `.sh` extension
- must start with the shebang which indicates the location of the binary that will interpret the script - commonly the `#!/bin/bash`
- pull in and run other scripts with `source /path/to/external/script`
- must make the file executable by giving the owner(s) the executable bit
- redirecting and pipe characters are meta characters
- can use `$1` and up for positional arguments passed to the file on execution

### Exit codes
- integers that get passed back to their parent processes once commands have completed
- `0` means no errors
- any other non-zero integer means an error of some kind
- the `exit [number]` command can be used for returning a specific error code
- exit codes can help determine whether commands succeed or fail, and then we can take action accordingly
- get the last exit code with `echo $?`
  - use to check if a script ran successfully
- use the `test [statement]` command to test the logic of a statement
  - e.g., `test 1 -eq 1 && echo 'true' || echo 'false'`
    - supply it a condition, then `&&` and something to run on true, and something to run on false
- conditionals

```bash
# basic conditional
if [ condition ]
then 
    [do a thing]
fi
```

```bash
# switch

case $var in 
    1)
        [do a thing]
        ;;
    2)
        [do a thing]
        ;;
    3)
        [do a thing]
        ;;
esac
```

- looping constructs

```bash
for i in [list]; do [a thing]; done

counter=0
while [ $counter -le 10 ]
do 
    [a thing]
    ((counter++))

    # or counter=$[ $counter+1 ]
done
```

- positional parameters
  - `$1 $2 $3 ... $n`
  - can set positional arguments with a space-separated list

```bash
#!/bin/bash

set -- arg1 arg2 arg3

echo -e "$1\n"
echo -e "$2\n"
echo -e "$3\n"

# $0 returns the location of last run binary
echo "the name of the script is $0"
```

- can't override these arguments
- can use them to set default behaviour if no other arguments are given