# Node.js


## Common issues

### listen EADDRINUSE: address already in use

**Error message**

```
Error: listen EADDRINUSE: address already in use :::3000
```

**Solution**

Zombie servers are running in background and listening at the targeted port. This very likely happened because previous executions of the server was killed with `SIGKILL` a signal and then did not close properly.

Killing all node running servers should solve the problem:

```
killall node
```
