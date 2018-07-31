---
layout: post
title:  "Thread concurrency mutex semaphore"
date:   2018-07-31
---

<p class="intro">Threads are lightweight processes that share the same address space(of a parent process), therefore can context-switch really fast. Process is an instance of a proram that is being executed. Processes are generally made up of threads. There are <b>kernel</b> and <b>user</b> threads in unix that are called <b>kthread</b> and <b>pthread</b>. 
<br><br>
<b>Mutex</b>, only the thread that locked or acquired the mutex can unlock it. In the case of a semaphore, a thread waiting on a semaphore can be signaled by a different thread. Some operating system supports using mutex & semaphores between process. 
</p>

<h3>Understanding Threads</h3>
Threads, also known as light weight processes are the basic unit of CPU initialization. So, why do we call them as light weight processes? One of the reason is that the context switch between the threads takes much lesser time as compared to processes, which results from the fact that all the threads within the process share the same address space, so you don?t need to switch the address space. 
<br><br>
In the user space, threads are created using the POSIX APIs and are known as pthreads. Some of the advantages of the thread, is that since all the threads within the processes share the same address space, the communication between the threads is far easier and less time consuming as compared to processes. And usually is done through the global variables. This approach has one disadvantage though. It leads to several concurrency issues and require the synchronization mechanisms to handle the same.
<br><br>
Having said that, why do we require threads? Need for the multiple threads arises, when we need to achieve the parallelism within the process. To give you the simple example, while working on the word processor, if we enable the spell and grammar check, as we key in the words, you will see red/green lines appear, if we type something syntactically/grammatically incorrect. This can most probably be implemented as threads.
<br><br>
The above material comes from here: <a href="https://sysplay.in/blog/linux-kernel-internals/2015/04/kernel-threads/">sysplay.in</a>
<br>
Just as a process is identified through a process ID, a thread is identified by a thread ID. Suppose there is a case where a link list contains data for different threads. Every node in the list contains a thread ID and the corresponding data. Now whenever a thread tries to fetch its data from linked list, it first gets its own ID by calling ?pthread_self()? and then it calls the ?pthread_equal()? on every node to see if the node contains data for it or not.
<br>
 * A process ID is unique across the system where as a thread ID is unique only in context of a single process.
 * A process ID is an integer value but the thread ID is not necessarily an integer value. It could well be a structure
 * A process ID can be printed very easily while a thread ID is not easy to print.


<h4>POSIX User Space Threads Example</h4>

{% highlight c %}
#include<pthread.h>
int pthread_create(pthread_t *restrict tidp, const pthread_attr_t *restrict attr, void *(*start_rtn)(void), void *restrict arg)
{% endhighlight %}


{% highlight c %}
#include<stdio.h>
#include<string.h>
#include<pthread.h>
#include<stdlib.h>
#include<unistd.h>

pthread_t tid[2];

void* doSomeThing(void *arg) {
    unsigned long i = 0;
    pthread_t id = pthread_self();

    if(pthread_equal(id,tid[0])) {
        printf("\n First thread processing\n");
    }
    else {
        printf("\n Second thread processing\n");
    }

    for(i=0; i<(0xFFFFFFFF);i++);
    return NULL;
}

int main(void) {
    int i = 0;
    int err;

    while(i < 2) {
        err = pthread_create(&(tid[i]), NULL, &doSomeThing, NULL);
        if (err != 0)
            printf("\ncan't create thread :[%s]", strerror(err));
        else
            printf("\n Thread created successfully\n");

        i++;
    }

    sleep(5);
    return 0;
}
{% endhighlight %}


<b>Output</b><br>
<pre>
$ ./threads
Thread created successfully
First thread processing
Thread created successfully
Second thread processing
</pre>

<b>Tips</b>: pthread_join(thread_id) for main to wait for the worker thread to finish.

<br> 
<b>Pthread Sample 2</b>
{% highlight c %}
#include<stdio.h>
#include<string.h>
#include<pthread.h>
#include<stdlib.h>
#include<unistd.h>

void * func(void * param){
  printf(?Inside Thread function\n?);
  char ** var = (char **)param;
  printf(?The passed argument is \?%s\?\n?, *var);
  int * i;
  i=(int *)malloc(4);
  *i=100;
  pthread_exit((void*)i);
}

int main(int argc, char * argv[]){ //Argument to be passed as command line argument
  int * retval;
  retval = (int *)malloc(4);
  pthread_t tid;
  pthread_attr_t attr; 
  pthread_attr_init(&attr);
  pthread_create(&tid,&attr,func,&argv[1]);
  pthread_join(tid, (void **)&retval);
  printf(?retval is %d\n?, *retval);
}
{% endhighlight %}

<br>
### Kernel Threads
Kernel threads are same as user space threads in many aspects, but one of the biggest difference is that they exist in the kernel space and execute in a privileged mode and have full access to the kernel data structures. These are basically used to implement background tasks inside the kernel. The task can be handling of asynchronous events or waiting for an event to occur. Device drivers utilize the services of kernel threads to handle such tasks. For example, the ksoftirqd/0 thread is used to implement the Soft IRQs in kernel. The khubdkernel thread monitors the usb hubs and helps in configuring  usb devices during hot-plugging.
<br>APIs for creating the Kernel thread
<br>Below is the API for creating the thread:

{% highlight c %}
//#include <kthread.h>
//kthread_create(int (*function)(void *data), void *data, const char name[], ...)
{% endhighlight %}


##### Parameters
* function: The function that the thread has to execute
* data:  The data? to be passed to the function
* name: The name by which the process will be recognized in the kernel
* Retuns: Pointer to a structure of type task_struct

Below is an example code which creates a kernel thread:

{% highlight c %}
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/delay.h>

static struct task_struct *thread_st;

// Function executed by kernel thread
static int thread_fn(void *unused)
{
    while (1)
    {
        printk(KERN_INFO "Thread Running\n");
        ssleep(5);
    }
    printk(KERN_INFO "Thread Stopping\n");
    do_exit(0);
    return 0;
}

// Module Initialization
static int __init init_thread(void)
{
    printk(KERN_INFO "Creating Thread\n");
    //Create the kernel thread with name 'mythread'
    thread_st = kthread_create(thread_fn, NULL, "mythread");
    if (thread_st)
        printk("Thread Created successfully\n");
    else
        printk(KERN_INFO "Thread creation failed\n");
    return 0;
}

// Module Exit
static void __exit cleanup_thread(void)
{
    printk("Cleaning Up\n");
}
{% endhighlight %}

In the above code, thread is created in the init_thread(). Created thread executes the function thread_fn(). Compile the above code and insert the module with insmod. Below is the output, you get:
Thread Created successfully
Thats all we get as an output. Now, you might be wondering why is thread_fn() not executing? The reason for this is, when we create the thread with kthread_create(), it creates the thread in sleep state and thus nothing is executed. So, how do we wake up the thread. We have a API wake_up_process() for this. Below is modified code which uses this API.

{% highlight c %}
// Module Initialization
static struct task_struct *thread_st;
{
     printk(KERN_INFO "Creating Thread\n");
    //Create the kernel thread with name 'mythread'
    thread_st = kthread_create(thread_fn, NULL, "mythread");
    if (thread_st)
    {
        printk("Thread Created successfully\n");
        wake_up_process(thread_st);
    }
    else
        printk(KERN_INFO "Thread creation failed\n");
    return 0;
}
{% endhighlight %}

As you might notice, wake_up_process() takes pointer to task_struct as an argument, which in turn is returned from kthread_create(). Below is the output:<br>

<pre>
Thread Created successfully
Thread Running
Thread Running
...
</pre>

As seen, running a thread is a two step process ? First create a thread and wake it up using wake_up_process(). However, kernel provides an API, which performs both these steps in one go as shown below:

{% highlight c %}
#include <kthread.h>
kthread_run(int (*function)(void *data), void *data, const char name[], ...)
{% endhighlight %}

<b>Parameters</b>:<br>
function ? The function that the thread has to execute
data ? The ?data? to be passed to the function
name ? The name by which the process will be recognized in the kernel
Returns: Pointer to a structure of type task_struct
So, just replace the kthread_create() and wake_up_process() calls in above code with kthread_run and you will notice that thread starts running immediately.

<b>"mutex"</b> is the actual low-level synchronizing primitive. You can take a mutex and then release it, and only one thread can take it at any single time (hence it is a synchronizing primitive). A recursive mutex is one which can be taken by the same thread multiple times, and then it needs to be released as many times by the same thread before others can take it.
A "lock" here is just a C++ wrapper class that takes a mutex in its constructor and releases it at the destructor. It is useful for establishing synchronizing for C++ scopes.
A condition variable is a more advanced / high-level form of synchronizing primitive which combines a lock with a "signaling" mechanism. It is used when threads need to wait for a resource to become available. A thread can "wait" on a CV and then the resource producer can "signal" the variable, in which case the threads who wait for the CV get notified and can continue execution. A mutex is combined with CV to avoid the race condition where a thread starts to wait on a CV at the same time another thread wants to signal it; then it is not controllable whether the signal is delivered or gets lost.
By- Antti Huima  https://stackoverflow.com/questions/1055398/differences-between-conditional-variables-mutexes-and-locks

<b>Read more at:</b><br>
https://www.thegeekstuff.com/2012/05/c-mutex-examples/?refcom

#### Mutex
A Mutex is a lock that we set before using a shared resource and release after using it. When the lock is set, no other thread can access the locked region of code. So we see that even if thread 2 is scheduled while thread 1 was not done accessing the shared resource and the code is locked by thread 1 using mutexes then thread 2 cannot even access that region of code. So this ensures a synchronized access of shared resources in the code.

<b>Internally mutex works as follows</b>:<br>

1. Suppose one thread has locked a region of code using mutex and is executing that piece of code.
2. Now if scheduler decides to do a context switch, then all the other threads which are ready to execute the same region are unblocked.
3. Only one of all the threads would make it to the execution but if this thread tries to execute the same region of code that is already locked then it will again go to sleep.
4. Context switch will take place again and again but no thread would be able to execute the locked region of code until the mutex lock over it is released.
5. Mutex lock will only be released by the thread who locked it.
6. So this ensures that once a thread has locked a piece of code then no other thread can execute the same region until it is unlocked by the thread who locked it.
7. Hence, this system ensures synchronization among the threads while working on shared resources.

A mutex is initialized and then a lock is achieved by calling the following two functions:

{% highlight c %}
/*initialize a mutex*/
int pthread_mutex_init(pthread_mutex_t *restrict mutex, const pthread_mutexattr_t *restrict attr);

/*lock critical code section with the following lock*/
int pthread_mutex_lock(pthread_mutex_t *mutex);

/*the mutex can be unlocked or the lock can be released like following*/
int pthread_mutex_unlock(pthread_mutex_t *mutex);


/*This function destroys the lock so that it cannot be used anywhere in future.*/
int pthread_mutex_destroy(pthread_mutex_t *mutex);
{% endhighlight %}

##### Mutex Thread Synchronization Example

{% highlight c %}
#include<stdio.h>
#include<string.h>
#include<pthread.h>
#include<stdlib.h>
#include<unistd.h>

pthread_t tid[2];
int counter;
pthread_mutex_t lock;

void* doSomeThing(void *arg) {
    pthread_mutex_lock(&lock);

    unsigned long i = 0;
    counter += 1;
    printf("\n Job %d started\n", counter);

    for(i=0; i<(0xFFFFFFFF);i++);

    printf("\n Job %d finished\n", counter);

    pthread_mutex_unlock(&lock);

    return NULL;
}

int main(void) {
    int i = 0;
    int err;

    if (pthread_mutex_init(&lock, NULL) != 0) {
        printf("\n mutex init failed\n");
        return 1;
    }

    while(i < 2) {
        err = pthread_create(&(tid[i]), NULL, &doSomeThing, NULL);
        if (err != 0)
            printf("\ncan't create thread :[%s]", strerror(err));
        i++;
    }

    pthread_join(tid[0], NULL);
    pthread_join(tid[1], NULL);
    pthread_mutex_destroy(&lock);

    return 0;
}
{% endhighlight %}

In the code above :
* A mutex is initialized in the beginning of the main function.
* The same mutex is locked in the ?doSomeThing()? function while using the shared resource ?counter?
* At the end of the function ?doSomeThing()? the same mutex is unlocked.
* At the end of the main function when both the threads are done, the mutex is destroyed.
* Now if we look at the output, we find :

<pre>
$ ./threads
Job 1 started
Job 1 finished
Job 2 started
Job 2 finished
</pre>
So we see that this time the start and finish logs of both the jobs were present. So thread synchronization took place by the use of Mutex.<br>


### Semaphore pthread example

{% highlight c %}
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
#include <semaphore.h>

sem_t semaphore;

void threadfunc() {
    while (1) {
        sem_wait(&semaphore);
        printf("Hello from da thread!\n");
        sem_post(&semaphore);
        sleep(1);
    }
}

int main(void) {
    // initialize semaphore, only to be used with threads in this process, set value to 1
    sem_init(&semaphore, 0, 1);
    
    pthread_t *mythread;
    
    mythread = (pthread_t *)malloc(sizeof(*mythread));
    
    // start the thread
    printf("Starting thread, semaphore is unlocked.\n");
    pthread_create(mythread, NULL, (void*)threadfunc, NULL);
    
    getchar();
    
    sem_wait(&semaphore);
    printf("Semaphore locked.\n");
    
    // do stuff with whatever is shared between threads
    getchar();
    
    printf("Semaphore unlocked.\n");
    sem_post(&semaphore);
    
    getchar();
    
    return 0;
}
{% endhighlight %}


### My references
1. Read more about Mutex/Semaphore on <a href="https://sysplay.in/blog/linux-kernel-internals/2015/06/concurrency-management-in-linux-kernel/">sysplay.in</a>
2. Mutex Example on the <a href="https://www.thegeekstuff.com/2012/05/c-mutex-examples/?refcom">thegeekstuff.com</a>



