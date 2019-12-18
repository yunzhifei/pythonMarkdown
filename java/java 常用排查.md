# java 常用排查

1. CPU 超高的排查。先使用 top 然后P来CPU排序找到对应的进程。然后 top -H -p 进程ID找到最消耗CPU的线程，然后jstack 来排查对应的线程。
2. oom 排查
   1. jmap -head 进程ID 可以查看Jvm 的相关信息
   2. **jmap -histo** 进程ID可以查看类信息 jmap -dump:live,format=b,