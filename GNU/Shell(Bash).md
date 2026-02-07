
# Shebang
The `shebang` is a special character sequence `#!` at the very beginning of a script file that tells the operating system which interpreter should execute the file. 

``` sh
#!/bin/bash
```

# Double Quotes (`""`) and Single Quotes (`''`)
Double quotes are used for "weak quoting". They hide all special characters from the shell except for the dollar sign ( $ ), backticks ( ` ), and the backslash ( \ ). 

Single quotes are used for "strong quoting". They suppress the meaning of all special characters, treating the enclosed text as a literal string. 

 `if` statement
 --
 
 Function call
 --
 
 ``` sh
 function str2hex() {
      hex_output=$(printf '%s' "$1" | xxd -p)
      echo "$hex_output"
 }
 
 function str2base64() {
      base64_output=$(printf '%s' "$1" | base64)
      echo "$base64_output"
 }
 
 local arg='test'
 local var=$(str2hex "$arg")
 local var2=$(str2base64 "$arg")
 ```
 
# Variable
## Environment variable
``` sh
export ENV_VAR="I am an environment variable"
```

## Global variable
``` sh
#!/bin/bash

# GLOBAL variable (script scope)
GLOBAL_VAR="I am a global variable"
```
 
## Local Variable
``` sh
function my_function {
    # LOCAL variable (function scope)
    local LOCAL_VAR="I am a local variable"
    
    # Access global and local variables inside the function
    echo "Inside function:"
    echo "Local: $LOCAL_VAR"
    echo "Global: $GLOBAL_VAR" # Global is accessible here
    
    # Modifying the global variable from inside the function
    GLOBAL_VAR="I modified the global variable"
}
```


# Console
* Issue command line with variables.
``` console
$ ENVIRONMENT_VARIABLE="1" sh ./shell_script.sh GLOBAL_VARIABLE="2"
```

* Lists all current environment variables.
``` console
$ printenv
$ env
```

* Removes a variable from the environment.
``` console
$ unset VARIABLE_NAME
```