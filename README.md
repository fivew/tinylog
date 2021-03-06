TinyLog
==============

[中文REAMDME请看这里](README_zh-CN.md)

Tinylog is a lightweight C-language high performance log component for UNIX environment, It is high performance, asynchronized, thread-safe and process-safe log library for C/C++.

It support log archiving, asynchronized, multithreading writing, multiprocessing writing, non-blocking mode.  

output example:
```
[2018-04-03 21:52:13,485][INFO][        example.c:7   ] This is a log message.
```

archive example:  
```
root@raspberrypi:/home/pi/code/tiny-log/test # ls log/ -l
total 11564
-rw-r----- 1 root root 8754060 Apr 20 21:48 test.log
-rw-r----- 1 root root 1543852 Apr 20 21:48 test.log-20180420-214824.gz
-rw-r----- 1 root root 1539119 Apr 20 21:48 test.log-20180420-214830.gz
```

Features
--------------
1. Log to compressed archive file.
2. Log level output.
3. log format customization.
4. asynchronized log output.
5. non-blocking log mode.
6. Multithreading concurrent write.
7. Multiprcessing concurrent write.

Usage
==============
1. Include the log header file `tlog.h` in the C code.
2. Call `tlog_init` to initialize the log module.
3. Call `tlog` function output log.
4. Call `tlog_exit` to exit and shutdown log component.

Example
--------------
```c
#include <stdio.h>
#include "tlog.h"

int main(int argc, char *argv[]) 
{
    tlog_init("./", "example.log", 1024 * 1024, 8, 1, 0, 0);
    tlog(TLOG_INFO, "This is a log message.\n");
    tlog_exit();
    return 0;
}
```

If you want filename wihout path in log, you can specify the macro BASE_FILE_NAME as the filename and generate as short file name when compiling. example as follows: (please check makefile in example directory.)  
For makefile:   
```
CFLAGS += -DBASE_FILE_NAME=\"$(notdir $<)\"
```

For cmake:   
```
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DBASE_FILE_NAME='\"$(notdir $<)\"'")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DBASE_FILE_NAME='\"$(notdir $<)\"'")
```

API description
----------------
1. int tlog_init(const char *logdir, const char *logname, int maxlogsize, int maxlogcount, int block, int buffsize);    
`Function`：Initialize log module  
`logdir`: Log Output path.    
`logname`: Log name.  
`maxlogsize`: The maximum size of a single log file.    
`maxlogcount`: Number of archived logs.    
`block`: Blocked if buffer is not sufficient.    
`buffsize`: Buffer size  
`multiwrite`: enable multi process write mode. (NOTICE: maxlogsize in all prcesses must be same when enable this mode. )  

2. tlog(level, format, ...)  
`Function`：Print log   
`level`: Current log Levels  
`format`: Log formats    

3. tlog_exit（）  
`Function`：Log component exits    

4. tlog_reg_format_func(tlog_format_func func)  
`Function`：Registers a custom Format function, and the callback function is defined as：tlog_format_func  

5. tlog_setlevel(tlog_level level)  
`Function`：Set log level，valid level are :TLOG_DEBUG, TLOG_INFO, TLOG_NOTICE, TLOG_WARN, TLOG_ERROR, TLOG_FATAL.

6. tlog_setlogscreen(int enable)  
`Function`：set whether the log is output to screen.  

License
===============
MIT License


