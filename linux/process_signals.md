

```
# list process signal
kill -l
```

```
sleep 1200
```
1. crt + z means SIGSTOP (foreground) , stop the process 

```
sleep 1200 &
```
2. for background, 
```
# list job
$ jobs

# stop the job
kill -SIGTSTP <process id>
```

3. continue the process
```
kill -SIGCONT <process id>
# or
kill -SIGCONT %<jobid>
```

4. terminate the process
```
kill -SIGTERM 

kill -SIGKILL
```

5. foreground ctrl+C is SIGINT

6. SIGHUP, when the parent process 结束,its children process get SIGHUP

```
#检查进程关系
pstree -s <process id>
```

```
根据进程名字来杀进程
pkill -15 sleep
```
**nohup 的作用在于放着父进程结束导致子进程HUP**