### Analogy-1

RSS is the Resident Set Size and is used to show how much memory is allocated to that process and is in RAM. It does not include memory that is swapped out. It does include memory from shared libraries as long as the pages from those libraries are actually in memory. It does include all stack and heap memory.

VSZ is the Virtual Memory Size. It includes all memory that the process can access, including memory that is swapped out, memory that is allocated, but not used, and memory that is from shared libraries.

So if process A has a 500K binary and is linked to 2500K of shared libraries, has 200K of stack/heap allocations of which 100K is actually in memory (rest is swapped or unused), and it has only actually loaded 1000K of the shared libraries and 400K of its own binary then:

**RSS: 400K + 1000K + 100K = 1500K**

**VSZ: 500K + 2500K + 200K = 3200K**

Since part of the memory is shared, many processes may use it, so if you add up all of the RSS values you can easily end up with more space than your system has.

The memory that is allocated also may not be in RSS until it is actually used by the program. So if your program allocated a bunch of memory up front, then uses it over time, you could see RSS going up and VSZ staying the same.

There is also PSS (proportional set size). This is a newer measure which tracks the shared memory as a proportion used by the current process. So if there were two processes using the same shared library from before:

PSS: 400K + (1000K/2) + 100K = 400K + 500K + 100K = 1000K
Threads all share the same address space, so the RSS, VSZ and PSS for each thread is identical to all of the other threads in the process. Use ps or top to view this information in linux/unix.


### Analogy-2


This article explains three indicators that can possibly be used to measure the memory consumption of a single process on Linux. VSZ (Virtual Memory Size), RSS (Resident Set Size), and PSS (Proportional Set Size). 

Although this will lack accuracy, let us consider an allegory to get the idea. There are three people sharing a room. We will consider each person to represent a process, and living expenses to represent memory consumption. Measuring the memory consumption of a single process will be represented as calculating the total living expense for one person in this allegory.

Each person has their own cell phone line that is not being shared. All three indicators, VSZ, RSS, and PSS, will count the cell phone bills as each persons living expense individually, and there is no problem with this.

The shared room comes with a garage space that can be used if they pay for it, but they all don't drive and they are not using it. However, VSZ will count the entire garage space cost as each persons living expense even though they are not using it. VSZ, therefore, represents the total living expense when they spend on every possible thing regardless of the actual usage. RSS and PSS only count expenses that are actually being used, and therefore, they will not count the garage space cost because it is not being used.

Since they are sharing the internet connection and the cable TV, they split up those bills. However, RSS will count the entire amount of the internet connection and cable TV as each persons living expense, even though they are sharing it and splitting up the bill. The idea of RSS is to calculate living expenses as it was not shared with anybody else.

PSS will only count one third of the internet connection and cable TV bill as each persons living expense, because they are sharing it. This is more reasonable than RSS since it is considers the fact that they are sharing it.

Let's make the allegory a little bit more complicated in order to understand the limitations of PSS. One person is always on the internet and doesn't watch TV that much. Hence, that person pays 50% of the internet connection bill and 20% of the cable TV bill upon agreement. PSS, however, cannot handle situations like this. It will simply calculate one third of the internet connection bill and cable TV bill as each persons living expense. 

Using PSS is what I consider most reasonable. However, there are limitations and there are also situations where RSS may work better. RSS is reasonable when you want to know the total living expense when you move out and live on your own.

### Refrences

https://www.google.com
https://web.archive.org/web/20120520221529/http://emilics.com/blog/article/mconsumption.html 
http://manpages.ubuntu.com/manpages/en/man1/ps.1.html
https://web.archive.org/web/20120520221529/http://emilics.com/blog/article/mconsumption.html

