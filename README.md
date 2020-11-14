Windows service template in pure C

Originally written and compiled using MSVC98, this code was updated by Michael Maltsev ([@m417z](https://github.com/m417z)) to allow compilation using newer Visual Studio versions

**NOTE**: This code was written in early 2000s, and was used with Windows
XP/2000. If you are looking to write a Windows service, please check updated
MSDN instead.

xou130 add:
This code is useful to test service in pure C language. It contains an intergrated installer/remover. Tested with GCC on windows 10 with default and -std=c17 build option. 
Now the service can stop and remove at running state normally.

Here are changes and reasons:
1. In main function, add 'system("pause")'. This is for double-click on the execution file.
2. In while(1) loop, add Sleep. When testing the original code, I noticed a 25% CPU usage on my i5-8250U. This is a simple fix.
3. while(ssStatus.dwCurrentState == SERVICE_RUNNING)
   and
   ReportStatusToSCMgr(SERVICE_STOPPED, NO_ERROR, 0);
   exit(0);
   
   This change is for a smooth quit by service manager. I am not sure if it is the best preactice. 
   If writing the loop without judging any conditions, the service stuck in SERVICE_STOP_PENDING state after receiving stop signal by sc. 
   The report SERVICE_STOPPED. It seems the report of SERVICE_STOPPED state is not reached in original code, so I add it here.
   The exit. The process is not terminated with a simple return. It still visible in the task manager without a force quit. 
