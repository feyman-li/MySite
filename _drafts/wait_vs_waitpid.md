# Difference between wait and waitpid

This example from the <<Unix Network Programming Volume 1, Third Edition: The Sockets Networking API>> p185.

The key issue is *Establishing a signal handler and calling wait from that handler are insufficient for preventing zombies.* The details as follow:

> Establishing a signal handler and calling wait from that handler are insufficient for preventing zombies. The problem is that all five signals are generated before the signal handler is executed, and the signal handler is executed only one time because Unix signals are normally not queued. Furthermore, this problem is nondeterministic. In the example we just ran, with the client and server on the same host, the signal handler is executed once, leaving four zombies. But if we run the client and server on different hosts, the signal handler is normally executed two times: once as a result of the first signal being generated, and since the other four signals occur while the signal handler is executing, the handler is called only one more time. This leaves three zombies. But sometimes, probably dependent on the timing of the FINs arriving at the server host, the signal handler is executed three or even four times.

The solution is bellow:
>The correct solution is to call waitpid instead of wait. Figure 5.11 shows the version of our sig_chld function that handles SIGCHLD correctly. This version works because we call waitpid within a loop, fetching the status of any of our children that have terminated. We must specify the WNOHANG option: This tells waitpid not to block if there are running children that have not yet terminated. In Figure 5.7, we cannot call wait in a loop, because there is no way to prevent wait from blocking if there are running children that have not yet terminated.

The key is **we cannot call wait in a loop, because there is no way to prevent wait from blocking if there are running children that have not yet terminated.**

We will explain those words for more details. The key code is 
	
	void
	sig_chld(int signo)
	{
		pid_t	pid;
		int		stat;

	    while ( (pid = waitpid(-1, &stat, WNOHANG)) > 0)
	    printf("child %d terminated\n", pid);

	    /* while((pid = wait(&stat)) >0) */
	    /*   printf("child %d terminated\n", pid); */

	   return;
	}
	
when just call *tcpcli04 127.0.0.1* one time. and "Ctl+c" to terminate the tcpcli4, both wait() and waitpid print in tcpserv04 is same 5 prints. When use kill to terminate one of processes in tcpserv04(it have 5 processes generate after conneted). The print also same. You can see how wait block the process.

The wait block the process is block for the for(;;), and can't go back to the for(;;) loop to accept another client connet. So we need frist wait the process(kill one of the tcpserv prosses, but not all), and try new call *tcpcli04 127.0.0.1* and the print can't echo Ok. but the waitpid can.
