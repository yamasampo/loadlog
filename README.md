# loadlog

Log system-wide CPU temp and load, fan speed and memory usage. Uses Python 3 to record how running a particular program
affects CPU temperature and load, fan speed, and overall memory usage. Any program that can be launched from the
command line can be profiled by this script.

## Test info

Tested on OS X 10.9.5, Python 3.4 by KK

### Update

Tested on MacPro (Late 2013) by HY

- OS version: maxOS High Sierra (10.13.6)
- Python version: 3.7.5
- Ruby version: 2.3.7p456 (2018-03-28 revision 63024) [universal.x86_64-darwin17]
- iStats version: v1.6.1 installed via Gem (version 2.5.2.3)
- psutil version: 5.6.7 installed via pip (version 19.3.1)

## Requirements

- istats : Ruby gem  
  Command-line program that outputs overall CPU temperature and fan speed  
  Install using command `gem install iStats`  
  URL: <https://github.com/Chris911/iStats>

`gem install iStats` may not work simply and you may get an error message like,

```[bash]
ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.3.0 directory.
```

In such case, use `sudo gem install -n <dir under PATH> iStats`. It will install iStats under the specified directory.

- psutil : Python 3 module  
  Install using `pip3 install psutil`  
  URL: <https://pythonhosted.org/psutil/>

## Usage

`loadlog.py <"command"> [options]`

## How it works

This script records system stats before the target program is launched, while the target is running, and
after a certain period of time after the program has finished.

To use this script, you must pass a command that you want to execute flanked by " ". Enter any command-line based
command as you would type it in Terminal. You can also pass it optional arguments such as the computer name to be
printed on the log file and the amount of time between recordings.

The script first instantiates a stats object and records system constants such as how many cpu cores are present
and how much memory is installed. It also records load, temperature and fan speed before the command is executed
(pre-run). After a certain amount of time (prewait), the script executes the command passed to it
as a child process. While the program is running (child process is active), the script records cpu and memory load stats
on a log file. After a predetermined amount time (postwait) after the command finishes executing,
the script again records cpu and memory load of the post-run state.
