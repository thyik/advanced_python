# Threading

use case
* make non-blocking function
* long blocking process
* UI and worker process

* add `Lock` if multiple thread accessing the same resource at the same time
* semaphore
  + alternative lock with more than one lock to be acquired
  
timer.py

```
from threading import Thread
import time

tLock = threading.Lock()

def timer(name, delay, repeat):
  print("Timer: " + name + " Started")
  tLock.acquire()
  print(name + " has acquired the lock")
  
  while repeat > 0:
    time.sleep(delay)
	print(name + ": " + str(time.ctime(time.time())))
	repeat -= 1
  print(name + " releasing lock")
  tLock.release()  #
  print("Timer: " + name + " Completed")
  
def Main():
  t1 = Thread(target = timer, args = ("Timer1", 1, 5))
  t2 = Thread(target = timer, args = ("Timer2", 2, 5))

  t1.start()
  t2.start()
  
  print("Main Completed")
  
if __name__ == '__main__':
  Main()
  
```

test the script

```
$ python3 timer.py
```

## AsyncWrite

AsyncWrite.py

```
import threading
import time

# inherit from threading

class AsyncWrite(threading.Thread):
  def __init__(self, text, out):
    threading.Thread.__init__(self)  # init base class
	self.tex = text
	self.out = out
	
  def run(self):   # override base method
    f = open(self.out, "a")
	f.write(self.text + '\n')
	f.close()
	time.sleep(2)
	print("Finished Background File Write to " + self.out)
	
def Main():
  message = input("Enter text to store:")
  background = AsyncWrite(message, "out.txt")
  background.start()
  print("The program can continue")
  
  background.join()
  print("Background thread completed")
  
if __name__ == '__main__'
  Main()
  
```

