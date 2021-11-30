## node进程
- 进程（Process）是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础，进程是线程的容器
```js
const http = require('http');

http.createServer().listen(3000, () => {
    process.title = '测试进程 Node.js' // 进程进行命名
    console.log(`process.pid: `, process.pid); // process.pid: 20279
});
```

## node线程
- 线程是操作系统能够进行运算调度的最小单位，首先我们要清楚线程是隶属于进程的，被包含于进程之中。
- 一个线程只能隶属于一个进程，但是一个进程是可以拥有多个线程的。
```js
const http = require('http');
const [url, port] = ['127.0.0.1', 3000];

const computation = () => {
    let sum = 0;
    console.info('计算开始');
    console.time('计算耗时');

    //1e10,1*10^10
    for (let i = 0; i < 1e10; i++) {
        sum += i
    };

    console.info('计算结束');
    console.timeEnd('计算耗时');
    return sum;
};

const server = http.createServer((req, res) => {
    if(req.url == '/compute'){
        const sum = computation();

        res.end(`Sum is ${sum}`);
    }

    res.end(`ok`);
});

server.listen(port, url, () => {
    console.log(`server started at http://${url}:${port}`);
});
```

## child_process 子进程
- child_process.spawn()
- child_process.exec()
- child_process.execFile()
- child_process.fork()
- 
```js
const spawn = require('child_process').spawn;
const child = spawn('ls', ['-l'], { cwd: '/usr' }) // cwd 指定子进程的工作目录，默认当前目录

child.stdout.pipe(process.stdout);
console.log(process.pid, child.pid); // 主进程id3243 子进程3244
```

```js
const exec = require('child_process').exec;

exec(`node -v`, (error, stdout, stderr) => {
    console.log({ error, stdout, stderr })
    // { error: null, stdout: 'v8.5.0\n', stderr: '' }
})
```

```js
const execFile = require('child_process').execFile;

execFile(`node`, ['-v'], (error, stdout, stderr) => {
    console.log({ error, stdout, stderr })
    // { error: null, stdout: 'v8.5.0\n', stderr: '' }
})
```

```js
const fork = require('child_process').fork;
fork('./worker.js'); // fork 一个新的子进程
```