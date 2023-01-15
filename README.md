# Medusa
Golang anti-vm framework



<p align="center">
  <a href="https://github.com/whiterabb17/medusa">
    <img src="https://github.com/whiterabb17/medusa/blob/main/medusa.jpg" alt="Logo" width="400" height="400">
  </a>

  
  <p align="center">
    Let Medusa stone wall analysis
    <br />
    <br />
    <br />
  </p>
</p>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#About-the-project">About The Project</a>
    </li>
    <li>
      <a href="#Getting-started">Getting Started</a>
      <ul>
        <li><a href="#Dependencies">Dependencies</a></li>
        <li><a href="#Installation">Installation</a></li>
      </ul>
    </li>
    <li>
      <a href="#Usage">Usage</a>
      <ul>
        <li><a href="#Anti-Debugging">Anti-Debugging </a></li>
        <li><a href="#Anti-Memory">Anti-Memory</a></li>
        <li><a href="#Anti-VM">Anti-VM</a></li>
      </ul>
    </li>
  </ol>
</details>




<!-- ABOUT THE PROJECT -->
## About The Project
Medusa is an anti-vm framework.<br>
Written in Golang in order to support Red Team operations and Pentesters during engagements.<br>
Medusa is designed for Windows environment!<br/>
!!I'm not responsible for your acts!!



<!-- GETTING STARTED -->
## Getting Started
Firstly, make sure that your dependencies are satisfied.

### Dependencies
Medusa has 3 dependencies:
* wmi
  ```
  go get github.com/StackExchange/wmi@v0.0.0-20210224194228-fe8f1750fd46
  ```
* go-ole
  ```
  go get github.com/go-ole/go-ole@v1.2.5 
  ```
* go-ps
  ```
  go get github.com/mitchellh/go-ps@v1.0.0 
  ```
  
### Installation
In your prompt type
  ```
  go get github.com/whiterabb17/medusa
  ```
 
### Note
Additional processes and configs can be set in `util\process_list.go`<br>
Such as AV processes to kill/search for.<br>
Additional strings to find or MAC addresses to add to the blacklist
  
## Usage
Into your program, import the packages used by Medusa
```
import (
      "github.com/whiterabb17/medusa/antidebug"
      "github.com/whiterabb17/medusa/antimem"
      "github.com/whiterabb17/medusa/antivm"
    )
```
### Anti-Debugging
` "github.com/whiterabb17/medusa/antidebug"` <br/>
Antidebug package implement strategies to avoid common programs that are used for debugging.

#### Process
`antidebug.ByProcessWatcher()` return boolean <br/>
This function look for common programs used for inspect process, like processhacker.exe, procmon.exe, xdbg.exe, etc. <br/>
Example:
```
if antidebug.ByProcessWatcher() { // Whether some debugger program founded, enter here.
  // exit or wait
}
```
#### Timming
`antidebug.ByTimmingDiff(time, int)` return boolean<br/>
Compare whether the difference between initial and end time is bigger than difference allowed (in seconds).
When debugging, some analisys use to take some time into a function.
Grab the time just in the begging of the function and later in the end, before go out, and ask Medusa to compare.<br/>
Example:
```
func myFuncHere(){
  initTime := time.Now() // grab the time here
  // do your actions here
  if antidebug.ByTimmingDiff(timeInit, 2){ // if your function takes 2 seconds or more, your malware must be debugged. You chose your time.
    // exit or wait
  }
}
  ```

### Anti-Memory
` "github.com/whiterabb17/medusa/antimem"` <br/>
Antimem package implement strategies to avoid common programs that are used for inspect memory process.

#### Memory
`antimem.ByMemWatcher()` return boolean <br/>
This function look for common programs used for inspect memory, like rammap.exe, dumpit.exe, etc. <br/>
Example:
```
if antimem.ByMemWatcher() { // Whether some program used for inspect memory founded, enter here.
  // exit or wait
}
```

### Anti-VM
` "github.com/whiterabb17/medusa/antivm"` <br/>
Antivm package implement strategies to avoid virtualized environment.

#### Disk size
`antivm.BySizeDisk( int )` return boolean <br/>
Check total size disk, in GB. <br/>
Example:
```
if antivm.BySizeDisk(100) { // whether total disk size is less than 100 GB, enter here. You chose the size, always in GB.
  // exit or wait
}
```
#### Virtual disk
`antivm.IsVirtualDisk()` boolean <br/>
Check whether may be on virtual disk. <br/>
Example:
```
if antivm.IsVirtualDisk() { // If Medusa guess you are on virtual disk, enter here.
  // exit or wait
}
 ```

#### Known virtual MAC Address
`antivm.ByMacAddress()` boolean <br/>
Look for known virtualized MAC Address. <br/>
Example:
```
if antivm.ByMacAddress() { If Medusa guess you are on virtual MAC Address, enter here.
  // exit or wait
}
```
