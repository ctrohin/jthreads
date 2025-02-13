# jthreads

jthreads is a Python library that extends on the Python Thread class by adding extra functionality such as LoopingThread, Timer, Interval, CountdownTimer and CallbackCountdownTimer.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install jthreads.

```bash
pip install jthreads
```

## Usage

### LoopingThread

```python
from jstreams import LoopingThread
from time import sleep

class MyThread(LoopingThread):
    def __init__(self) -> None:
        LoopingThread.__init__(self)
        self.count = 0
    
    def loop(self) -> None:
        # Write here the code that you want executed in a loop
        self.count += 1
        print(f"Executed {self.count} times")
        # This thread calls the loop implementation with no delay. Any sleeps need to be handled in the loop method
        sleep(1)
thread = MyThread()
thread.start()
sleep(5)
# Stop the thread from loopiong
thread.cancel()
```

### CallbackLoopingThread
This looping thread doesn't require overriding the loop method. Instead, you provide a callback
```python
from jthreads import CallbackLoopingThread
from time import sleep

def threadCallback() -> None:
    print("Callback executed")
    sleep(1)

thread = CallbackLoopingThread(threadCallback)
# will print "Callback executed" until the thread is cancelled
thread.start()

sleep(5)
# Stops the thread from looping
thread.cancel()
```
### Timer
The Timer thread will start counting down to the given time period, and execute the provided callback once the time period has ellapsed. The timer can be cancelled before the period expires.
```python
from jthreads import Timer
from time import sleep

timer = Timer(10, 1, lambda: print("Executed"))
timer.start()
# After 10 seconds "Executed" will be printed
```

```python
from jthreads import Timer
from time import sleep

# The first parameter is the time period, the second is the cancelPollingInterval.
# The cancel polling interval is used by the timer to check if cancel was called on the timer.
timer = Timer(10, 1, lambda: print("Executed"))
timer.start()
sleep(5)
timer.cancel()
# Nothing will be printed, as this timer has been canceled before the period could ellapse
```

### Interval
The interval executes a given callback at fixed intervals of time.
```python
from jthreads import Interval
from time import sleep
interval = Interval(2, lambda: print("Interval executed"))
interval.start()
# Will print "Interval executed" every 2 seconds
sleep(10)
# Stops the interval from executing
interval.cancel()
```

### CountdownTimer
The countdown timer is similar in functionality with the Timer class, with the exception that this timer cannot be canceled. Once started, the callback will always execute after the period has ellapsed.
```python
from jthreads import CountdownTimer

CountdownTimer(5, lambda: print("Countdown executed")).start()
# Will always print "Countdown executed" after 5 seconds
```

## License

[MIT](https://choosealicense.com/licenses/mit/)