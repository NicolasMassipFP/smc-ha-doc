## server closed the connection unexpectedly

Example
```
2026-02-18 09:38:30.189 FILE [1878] ERROR:  could not receive data from WAL stream: server closed the connection unexpectedly"
```

This error may appear during an Active Server restart or a server switch, depending on how quickly the system transitions between states. In such cases, the error is expected and temporary.

However, this error should **not** occur without a server restart. If it does, it may indicate that the Active Server restarted unexpectedly (for example, due to insufficient memory) or that a network issue is affecting communication between servers.