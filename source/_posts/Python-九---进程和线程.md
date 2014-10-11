title: Python 九 - 进程和线程
date: 2014-10-11 16:09:07
categories: Python
tags: [Python]
---
Unix/Linux操作系统提供了`fork()`系统调用，`fork()`调用一次，返回两次，因为操作系统自动把当前进程(父进程)复制一份(子进程)，分别在父进程和子进程返回。
<!--more-->
子进程永远返回`0`，而父进程返回子进程的ID。一个父进程可以fork出很多子进程，所以父进程要记下每个子进程的ID，而子进程字需要`getppid()`就可以拿出父进程的ID
```python
# multiprocessing.py
import os

print 'Process (%s) start...' % os.getpid()
pid = os.fork()
if pid==0:
    print 'I am child process (%s) and my parent is %s.' % (os.getpid(), os.getppid())
else:
    print 'I (%s) just created a child process (%s).' % (os.getpid(), pid)
```
> 注：Windows没有`fork`调用，所以上面代码在windows无法运行
有了`fork`调用，一个进程在接到新任务时就可以复制出一个子进程来处理新任务

##multiprocessing
`multiprocessing`模块是跨平台版本的多进程模块，它提供了一个`Process`类来代表一个进程对象
```python
from multiprocessing import Process
import os

def run_proc(name):
	print 'Run child process %s (%s)...' % (name,os.getpid())

if __name__=='__main__':
	print 'Run child process %s .' % os.getpid()
	p = Process(target=run_proc,args=('test',))
	print 'Process will start.'
	p.start()	# 启动`start()`
	p.join()	# 等待子进程结束再继续往下运行
	print 'Process end.'
```
###Pool
需要启动大量子进程，可以用进程池的方式批量创建子进程:
```python
from multiprocessing import pool
import os,time,random

def long_time_task(name):
	print 'Run task %s (%s)...' % (name,os.getpid())
	start = time.time()
	time.sleep(random.random() * 3)
	end = time.time()
	print 'Task %s runs %0.2f seconds.' % (name,(end - start))

if __name__=='__main__':
	print 'Parent process %s.' % os.getpid()
	p = Pool()
	for i in range(5):
		p.apply_async(long_time_task,args=(i,))
	print'Waiting for all subprocesses done...'
	p.close()
	p.join()
	print 'All subprocesses done.'
```
`Pool`对象调用`join()`方法会等待所有子进程执行完毕，调用`join()`之前必须先调用`close()`，调用之后就不能继续添加新的`Process`了。`Pool`默认大小是CPU的核数，可以在`Pool()`中设置参数，设置同时进行的任务数
##进程间通信
Python提供了`Queue`、`Pipes`等多种方式来交互数据
```python
from multiprocessing import Process,Queue
import os,time,random

def write(q):
	for value in ['A','B','C']:
		print 'Put %s to queue...' % value
		q.put(value)
		time.sleep(random.random())

def read(q):
	while True:
		value = q.get(True)
		print 'Get %s from queue.' % value

if __name__=='__main__':
	q = Queue()
	pw = Process(target=write,args=(q,))
	pr = Process(target=read,args=(q,))
	pw.start()
	pr.start()
	pw.join() # 等待pw结束
	pr.terminate() # pr进程是死循环，无法等待其结束，只能强制终止
```
> 在Unix/Linux下，multiprocessing模块封装了fork()调用，使我们不需要关注fork()的细节。由于Windows没有fork调用，因此，multiprocessing需要“模拟”出fork的效果，父进程所有Python对象都必须通过pickle序列化再传到子进程去，所有，如果multiprocessing在Windows下调用失败了，要先考虑是不是pickle失败了。