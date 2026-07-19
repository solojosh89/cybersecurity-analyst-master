# Reflection: Module 00

## What surprised me?

The biggest surprise was learning that one program can create many processes. Before this lesson, I thought opening Chrome would create only one process, but I learned that Chrome intentionally creates many processes to isolate different tasks. I was also surprised that Windows can have many instances of `svchost.exe` running at the same time, and that this is completely normal.

Another surprise was realizing that a process name alone cannot be trusted. Before this lesson, I assumed that if I saw `chrome.exe` or `svchost.exe`, then it must be legitimate. I now understand that analysts must investigate beyond the name.

## What mistake did I make?

My biggest mistake was thinking that a process was simply a "running activity." Although that wasn't completely wrong, it was incomplete. I now understand that a process is a running instance of a program after Windows loads it into memory and begins executing it.

I also tended to make conclusions too quickly. Through this lesson, I learned to separate observations from conclusions and to ask for more evidence before deciding whether something is legitimate or suspicious.

## What would I investigate next?

- Why every process has a unique PID.
- How Windows decides which processes to start automatically.
- How parent processes work.
- Why `svchost.exe` has so many instances.
- How to verify whether a process is legitimate using its path, parent process, digital signature, and behavior.

## How would I explain this to someone new?

A program is a set of instructions stored on your computer. It does nothing until Windows starts it. Once Windows loads the program into memory and begins running it, it becomes a process.

A process is like a worker carrying out the instructions in the program. One program can create many processes, which is why applications like Chrome appear many times in Task Manager.

I would also tell them not to trust a process simply because of its name. In cybersecurity, we investigate using evidence such as the file path, parent process, and behavior rather than making assumptions based on filenames.

## Personal takeaway

This lesson taught me more than what a process is. It taught me how a cybersecurity analyst thinks. Instead of guessing or assuming, I should collect evidence, ask questions, and only make conclusions that the evidence supports. I believe this mindset will be useful throughout the rest of my cybersecurity journey.
