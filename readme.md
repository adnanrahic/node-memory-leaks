# Showcasing Node.js Memory Leaks

In order to expose the inspector, let's run Node.js with the `--inspect` flag.

```bash
node --inspect index.js

Debugger listening on ws://127.0.0.1:9229/7fc22153-836d-4ed2-8090-a84a842a199e
For help, see: https://nodejs.org/en/docs/inspector
Sample app listening on port 3000.
```

Open up Chrome and go to `chrome://inspect`. Here you can take snapshots of the memory usage.

```bash
open -a "Google Chrome" chrome://inspect/#devices
```

![Screen Shot 2022-01-26 at 00.10.50.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ed704efb-7bcc-426b-a152-a9e6d729c611/Screen_Shot_2022-01-26_at_00.10.50.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220208%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220208T203708Z&X-Amz-Expires=86400&X-Amz-Signature=524b50cc860a8275698969ef19f5b517a12d8f897188551a07f6d6b08388e871&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screen%2520Shot%25202022-01-26%2520at%252000.10.50.png%22&x-id=GetObject)

## **Watching Memory Allocation In Real-Time**

You use the `Allocation instrumentation on timeline` option in this case. Select that radio button and check the `Record stack traces of allocations` checkbox. This will start a live recording of the memory usage.

For this use case, I used `[loadtest](https://www.npmjs.com/package/loadtest)` to run 1000 requests against the sample Express app with a concurrency of 10.

![Screen Shot 2022-01-26 at 00.27.32.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c6112871-a107-42ad-ba47-fdd1d4acb22a/Screen_Shot_2022-01-26_at_00.27.32.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220208%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220208T203706Z&X-Amz-Expires=86400&X-Amz-Signature=bf677e4c4676cd0329d06667fdde425eec11c915e8ee3120f6e9946487f73516&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Screen%2520Shot%25202022-01-26%2520at%252000.27.32.png%22&x-id=GetObject)

For the first few requests, you can see a spike in memory allocation. But it’s obvious that most memory is allocated to the arrays, closure, and objects.
