---
title: Async Queue with Concurrency Limit
shortTitle: Async Queue
language: javascript
tags: [async, queue, concurrency, promise, utility]
cover: false
excerpt: Queue async tasks with a concurrency limit to prevent overload.
listed: true
dateModified: 2025-12-17
---

Queue async tasks with a concurrency limit to prevent running too many tasks at once.

```js
class AsyncQueue {
    constructor(concurrency = 2) {
        this.concurrency = concurrency; // max tasks running at once
        this.running = 0;               // current running tasks
        this.queue = [];                 // pending tasks
    }

    // add a task to the queue
    add(task) {
        return new Promise((resolve, reject) => {
            const runTask = async () => {
                this.running++;
                try {
                    const result = await task();
                    resolve(result);
                } catch (err) {
                    reject(err);
                } finally {
                    this.running--;
                    this.next(); // trigger next task in queue
                }
            };

            this.queue.push(runTask);
            setTimeout(() => this.next(), 0); // ensure async execution
        });
    }

    // run next task if concurrency allows
    next() {
        if (this.running >= this.concurrency) return;
        const nextTask = this.queue.shift();
        if (nextTask) nextTask();
    }
}

// Example usage:
const queue = new AsyncQueue(3); // max 3 tasks concurrently
const sleep = (ms) => new Promise(res => setTimeout(res, ms));

const tasks = Array.from({ length: 10 }, (_, i) => () =>
    sleep(1000).then(() => console.log(`Task ${i+1} done`))
);

tasks.forEach(task => queue.add(task));
