# assignment-6
# Assignment 6 – Process Management Utilities

**Submitted by:** Jeetendra Singh

---

# Part A: otProcessManager

`otProcessManager` is a shell utility used to inspect and manage running processes.

## 1. Create otProcessManager

Create the script:

```bash
nano otProcessManager
```

Give execute permission:

```bash
chmod +x otProcessManager
```

### Screenshot 1

<img width="1440" height="900" alt="Screenshot 6 1" src="https://github.com/user-attachments/assets/fd44e0b3-ffdb-48f8-a90d-15314b3f4629" />


---

## 2. Top N Processes by Memory

The `topProcess` option displays the top N processes based on memory usage.

```bash
./otProcessManager topProcess 5 memory
```

For CPU:

```bash
./otProcessManager topProcess 10 cpu
```

### Screenshot 2

![Top Processes by Memory and CPU](images/screenshot2.png)

---

## 3. Kill Process Having Least Priority

This command identifies and terminates a process with the lowest priority.

```bash
sudo ./otProcessManager killLeastPriorityProcess
```

Verify the running processes:

```bash
ps
```

### Screenshot 3

![Kill Least Priority Process](images/screenshot3.png)

---

## 4. Running Duration of a Process

The utility can show the running duration of a process using either PID or process name.

Using PID:

```bash
./otProcessManager RunningDurationProcess 3957
```

Using process name:

```bash
./otProcessManager RunningDurationProcess bash
```

### Screenshot 4

![Running Duration](images/screenshot4.png)

---

## 5. List Orphan and Zombie Processes

### List Orphan Processes

```bash
./otProcessManager listOrphanProcess
```

### List Zombie Processes

```bash
./otProcessManager listZoombieProcess
```

### Screenshot 5

![Orphan and Zombie Processes](images/screenshot5.png)

---

## 6. Kill Process and List Waiting Processes

Kill a process using its name or PID:

```bash
./otProcessManager killProcess sleep
```

List waiting processes:

```bash
./otProcessManager ListWaitingProcess
```

### Screenshot 6

![Kill and Waiting Processes](images/screenshot6.png)

---

# Part B: ProcessManager.sh

`ProcessManager.sh` is a utility used to register, start, stop, monitor and manage services.

## 1. Create ProcessManager.sh

```bash
nano ProcessManager.sh
```

Give execute permission:

```bash
chmod +x ProcessManager.sh
```

---

## 2. Create a Dummy Service

Remove the previous process manager directory:

```bash
rm -rf ~/.processmanager
```

Create a dummy service:

```bash
cat > dummy.sh << 'EOF'
#!/bin/bash
while true; do
    sleep 5
done
EOF
```

Give execute permission:

```bash
chmod +x dummy.sh
```

---

## 3. Register the Service

Register `dummy.sh` as `myservice`:

```bash
./ProcessManager.sh -o register -s "$(pwd)/dummy.sh" -a myservice
```

---

## 4. Start and Check Service Status

Start the service:

```bash
./ProcessManager.sh -o start -a myservice
```

Check its status:

```bash
./ProcessManager.sh -o status -a myservice
```

---

## 5. Change Service Priority

Set high priority:

```bash
./ProcessManager.sh -o priority -p high -a myservice
```

---

## 6. List Services and Display Top Processes

List registered services:

```bash
./ProcessManager.sh -o list
```

Display top processes:

```bash
./ProcessManager.sh -o top
```

Display information for a specific service:

```bash
./ProcessManager.sh -o top -a myservice
```

---

## 7. Kill the Service

```bash
./ProcessManager.sh -o kill -a myservice
```

Verify that the service has stopped:

```bash
./ProcessManager.sh -o status -a myservice
```

```bash
./ProcessManager.sh -o top -a myservice
```

### Screenshot 7

![ProcessManager Service Management](images/screenshot7.png)

---

# Part C: Playing Around with Process and Log File

Start a background process that continuously writes to a log file:

```bash
exec 3>> /tmp/test.log

while true; do
    echo "$(date): running" >&3
    sleep 1
done &

echo "PID: $!"
```

View the log:

```bash
cat /tmp/test.log
```

Clear the log file:

```bash
> /tmp/test.log
```

Check the process:

```bash
ps -p $!
```

The process continues running even after the log file is cleared because the process still has the file descriptor open.

Delete the log file:

```bash
rm /tmp/test.log
```

Check the process:

```bash
ps -p $!
```

Check the deleted file descriptor:

```bash
ls -l /proc/$!/fd/ | grep deleted
```

Check the process priority:

```bash
ps -o pid,ni,comm -p $!
```

Elevate the priority:

```bash
sudo renice -n -10 -p $!
```

Verify the new priority:

```bash
ps -o pid,ni,comm -p $!
```

Cleanup:

```bash
kill $!
exec 3>&-
rm -f /tmp/test.log
```

---

# Conclusion

In this assignment, the following process management concepts were implemented and tested:

* Top processes by CPU and memory
* Process priority management
* Killing processes
* Process running duration
* Orphan processes
* Zombie processes
* Waiting processes
* Service registration
* Starting and stopping services
* Service status monitoring
* Service priority management
* Process and log file handling
* Deleted file descriptors
* `renice` and process priority

The assignment demonstrates practical Linux process management using Bash scripting and standard Linux process utilities.
