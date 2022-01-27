# Valgrind
###### tags: `Linux`


> valgrind --leak-check=full --show-leak-kinds=all \
         --track-origins=yes \
         --verbose \
         --log-file=valgrind-out.txt \
         ./executable exampleParam1



valgrind --tool=callgrind [program to be executed] [program option]







## Tool Selection
--tool=<toolname> [default: memcheck]
### --tool=callgrind
valgrind --tool=callgrind [program to be executed] --callgrind-out-file=/var/callgrind.out --dump-instr=yes --trace-jump=yes --auto=yes
    
valgrind --callgrind-out-file=/var/callgrind.out.%p --tool=callgrind /userfs/bin/wapp -e br0 