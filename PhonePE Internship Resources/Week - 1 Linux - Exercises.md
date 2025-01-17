# Week - 1

## Task Set -1

Download and install Ubuntu 20.04 (Focal) server edition (Not desktop edition)

Once installed, create users for our team as follows:

- Add srivatsa, lekhana and rohit to interns group
- Add mandar, saurabh (and others) to sre group
- Attach two disks of 10G each to this VM. Create a physical volume (PV), volume group (VG) and logical volume (LV) out of it. Format the LV with xfs and mount it on /data directory. Please ensure that the LV mounted on /data persists across reboots.
- Attach an additional disk of 10G to the VM and add it to the existing VG.
- Bring up an HTTP server on this VM such that it starts automatically whenever the VM is restarted.
- Automate the entire setup using Packer.

### Task Set - 2

Once these tasks are completed, please create a git repo of your own (on github) and push the answers to the following questions to the repo in a README file and then share the repo link with me:

1. Find the processes running on a linux machine
2. Find the users currently logged in
3. Find the uptime of the machine
4. Find the ram usage
5. Find the disk usage
6. Find the inode usage
7. Find the ulimit of a user
8. Find the ulimit of a process
9. Find the file descriptors used by a process
10. Find the top 5 processes by memory usage
11. Find the top 5 processes by cpu usage
12. Find the top 5 processes by network usage
13. Find the top 5 processes by disk iops usage
14. Find the network traffic and bandwidth usage of the machine
15. Given a file as input, find the processes using that file
16. List files opened by a process (ex: sshd, httpd)
17. List processes listening on a specific port (ex: 22)
18. Find the status of a service (ex: httpd)
19. Find zombie processes on a machine
20. Find the environment variables set on a machine
21. Display processes started by a user
22. Kill a process
23. List open ports
24. Find the permissions set for a file
25. List all the directories inside a directory
26. Display the number of files inside a directory
27. Display the number of lines in a file
28. List oldest and newest file in a directory
29. Display the OS and kernel information
30. Display the sizing (cpu cores, memory, and disk) of the machine
