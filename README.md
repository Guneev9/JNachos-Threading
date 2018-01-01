# JNachos-Threading
Implementing Threads

I have created the following files for Threading Implementation:(I have used the my Lab-2 JNachos zip file and I have removed multiprogramming code from main.java as this project is independent of previous code)

1) NachosThreading.java:
The file has basic operations which need to be performed on threads:

I have added the Thread States (JUST_CREATED, RUNNING, READY, BLOCKED) using Enumeration.

Following are the class Methods:

a) Default Constructor –
It is used to initialize the Thread block, it basically initializes the thread.

b) CreateThread(VoidFunctionPtr pFunc, Object pArg):

*This function is used in new System calls as per implementation requirements.
*It first disables the interrupts
*Send the thread to scheduling, I will cover it below.
*Then, the interrupts are enabled
*Thread is called using call().

c) switchThread(NachosThreading T_NextThread) :

*It will dispatch the CPU to the next thread.
*It will then Save the state of the old thread, and load the state of the new thread, by calling the machine dependent

d) start_Thread():

*It first disables the interrupt
*It will destroy the previously running thread if finished
*It then enables the interrupt to its previous state

e) resume():
*it will resume the thread, if it has already started or else will start it
*Set the mstarted variable to True

f) suspend():

*it will suspend the JNACHOS thread.

g) Yield():

*It first disables the interrupt
*It will yield the currently running thread
*It will check for the next thread to be run and if its there, it will mark it as Ready
*It will switch the Scheduler to this thread
*It then enables the interrupt to its old state

h) sleep():

*It will put the thread to sleep
*It will set its status as “BLOCKED”
*It then switches to next thread

i) finish():

*It will turn off the interrupt
*It will kill the thread
*Then put it to sleep()
*I have set is_finish=true

These are the major functions in this class. There are other utility functions as well in order to save or restore thread’s state, or getting the name or status of thread.

3) CreatedThread.java:
This is the class in order to implement function pointer and retrieve thread state. I have made changes to the following java files as per the Requirements:

1) NachosProcess.java:

I have made the change in fork() method, earlier it is used to create a default java thread but now I am creating a thread here using NachosThreading.java class. I have commented the functions which are not required for now(sleep, yield, finish,
switchprocess etc.) since these will be created in new thread class.

2) JNachos.java:
I have created the following methods:

a) getCurrentThread() :
it is the static function in order to get the current thread.
b) setCurrentThread():
It is used to update the currently running thread.
c) getThreadToBeDestroyed():
It Returns the thread to be destroyed on the next context switch.
d) setThreadToBeDestroyed():
It will Set the thread to be destroyed on the next context switch.

3) Scheduler.java:
I have moved the scheduler concept from process to threads. I have linked the scheduler file to thread methods so that it schedules threads. Basically, I have change the references to threads.

4)SystemCallHandler.java:

I have added the following two new System calls:

a) SC_CreateThread:
Here, the first thing is to disable the interrupt because, at the time of system call, if an interrupt occurs, it will create a problem, so storing its old state, so that we can return to its initial state in a variable and then turning it to false Then, allocate stack space for it from space of process’s address space. Then, we are creating a new thread by calling Create_Thread function of new thread class. Then, at last enable the interrupt by restoring its old state.

b) SC_WaitThread:

It first disable the interrupt.Now, I have to get the thread_id of thread created by above system call.
Since I have given the thread name to them as “S_Thread”, so I am retrieving the id using that. Then, I am putting it to sleep
Then I have created a new thread by name “W_Thread”.Then, once it gets finished, I have resumed the previous thread. For this I am using a static variable “is_finish”(declared in new thread class).At last, interrupt is enabled to its old state.

5)AddrSpace.java:

I have increase the size of UserStackSize variable to 16*1024 since threads are using the same stack.
