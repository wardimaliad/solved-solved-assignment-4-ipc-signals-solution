Download Link: https://assignmentchef.com/product/solved-solved-assignment-4-ipc-signals-solution
<br>
FunctionalityIn the previous assignment synchronization of taking turn to play cannot be done but applying frivolous sleep calls to politely “delay” themselves. With IPC method signals, better synchronization can be achieved.This assignment is still about child processes playing the sum-to-goal game, and the overall game play will look the same as the previous assignment. but the parent will use signal SIGINT to notify each child to play its turn at that time, so a round-robin fair-play sequence can be enforced with signals.The parent and children are to be coded separately in parent.c and child.c.Whenever a child reaches the goal and exits, the parent will announce this result (child PID and the number it has). And the parent will signal all processes (including itself) to quit via signal SIGQUIT.All processes (parent and children) should use a handler to handle exiting which should show a “Bye-bye! Parent/Child x. PID xxxxx.
” message.Extra Credit PointsThe parent will not interrupt anyone but continue to send signals to those that are still playing until all children have done playing.Then after all child processes are done, the parent shows final results in the order of “the 1st-exit-1st-recorded/shown.” Show both the PID and sum for each.This means that the parent needs to keep track of children’s PID’s from the gecko in order to skip those who have already finished (no need to signal in the round-robin play loop). And during the play loop the parent will record the sum as well as the child PID as each finishes.Program Code WritingTwo two separate C programs: parent.c and child.c are required. As described in previous assignments. Modularize programs with subroutines illustrated in the example code: MySigIntRoutine, MyExitRoutine, etc.Demo RunableDEMO RUNABLE FILES ARE HERE.Use shell commands cp and chmod to copy and change to executables as described before.New System/Library CallsYou will need these new ones besides those allowed from the previous assignment:signal() to register a handler to be called by a signal (“man 7 signal”)atexit() to register a handler to be called when exit()kill() to send out signalssleep(1) in the single parent code in the play loop, to view children’s number-accumulation messages slower.perror() is optional but useful.Helpful HintsThe example code illustrates well all these mentioned below.All signal names are listed in Unix manual page “man 7 signal.” SIGINT, SIGQUIT, and SIGCHLD are the ones we use.You must demonstrate the use of these signals and with handlers registered (for the bonus version SIGQUIT is not required since all children play to goal, and exit themselves). The atexit() must also be used for both the parent and child code. It resembles a signal-handler registration.In the bonus version, you will find the limitation of signal-handling routines that cannot take arguments or have a return. Therefore, global and static variables are needed in order to update and pass information. Figure out which must be global or at least static to initialize in a handler routine!The number of maximum child processes is 10 (MAX_CHILD) is the size of an array that is to be declared since it is needed for child PID’s.Assignment Turn-inSubmit your source code the same way as required before in previous programming assignments. No E-mail will be accepted, no late turn-ins.Locate a folder “4” in your named folder to place parent.c and child.c into it. No other files. For repeating submissions, create new folders V2, V3, etc., inside folder “4.”Format your code cleanly, put a header/preamble header with your name, and put suitable and important comments in code (the grader may understand better of your code). Messy code may cause point deduction.Child Code Construction (Minimum Version)Preamble: name, program #, course name, etc.some include statementsand 2 global variables to declare

void MySigIntRoutine() { // will it need static vars?…}

void MyExitRoutine() { // print a message of its own process info…}

int main(int argc, char *argv[]) { // argv[1] child #, argv[2] goal #… local vars …if(argc != 3) { // why 4? see 5 lines aboveprintf(“Child didn’t get 3 args!
”);exit(1);}…while(1) {… wait for a signal}}Parent Code ConstructionPreamble: name, program #, course name, etc.some include statementsand constant declaration

void MySigChildRoutine() {…… show winning child result (child PID and final sum)… printf(“
Parent: proceeds to stop everyone!

”);…}

void MyExitRoutine() { // just a msg like basic demo…}

int main (int argc, char *argv[]) {… local vars…

if(argc != 3) {printf(“Usage: %s child_number (1~10) goal_number (10~100)
”, argv[0]);exit(1);}…printf(“***Parent (PID %d): forks…

”, getpid());

for(…) { // fork loop…}// give the children a break before starting to playsleep(1); // 1st sleep…printf(“
***Parent (PID %d): loops to signal children…
”, getpid());

while(1) {for(…) { // loop thru children… show a msg: Parent sending signal to child # (PID #)… send itsleep(1); // 2nd sleep, total 2 in all code}}}