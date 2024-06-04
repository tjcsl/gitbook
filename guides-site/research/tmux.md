---
description: How and why you should use tmux to run your jobs.
---

# tmux

## What is tmux

"**tmux** is a [terminal multiplexer](https://en.wikipedia.org/wiki/Terminal\_multiplexer) for [Unix-like](https://en.wikipedia.org/wiki/Unix-like) [operating systems](https://en.wikipedia.org/wiki/Operating\_system). It allows multiple [terminal](https://en.wikipedia.org/wiki/Computer\_terminal) sessions to be accessed simultaneously in a single window. It is useful for running more than one [command-line](https://en.wikipedia.org/wiki/Command-line\_interface) program at the same time. **It can also be used to detach** [**processes**](https://en.wikipedia.org/wiki/Process\_\(computing\)) **from their controlling terminals, allowing** [**SSH**](https://en.wikipedia.org/wiki/Secure\_Shell) **sessions to remain active without being visible.**" - Wikipedia

In other words, tmux allows you to start a session to run your job and then exit the terminal window without killing your process.&#x20;

## Creating a new tmux session

```
tmux new -s "name"
```

This will create (and open) a new tmux session with the given name. In the session, you can then run your job the way you normally would over ssh.&#x20;

## Detaching a tmux session

Detaching the session allows you to close your ssh connection (and then close the terminal) **without** killing the process running in the tmux session.&#x20;

The session can be detached by clicking control-b and then typing a d.

## Attaching a tmux session

After sshing back into the remote server, to "re-open" or attach a tmux session, use the following command. This can be used to see if your process finished running and/or the output of the process.&#x20;

```
tmux attach-session -t "name"
```

## Killing a tmux session

After your process finishes running in the tmux session, you can then kill it off with the following work.&#x20;

```
tmux kill-session -t "name"
```

This will close off any process running in the session, so make sure that your job is done.&#x20;

## Listing all active sessions

```
tmux ls
```
