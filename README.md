添加保活代码：
```JavaScript
function executeCurlCommand(command) {
  return new Promise((resolve, reject) => {
    exec(command, (error, stdout, stderr) => {
      if (error) {
        reject(`命令执行错误: ${error.message}`);
        return;
      }
      resolve(stdout.trim());
    });
  });
}

async function baohuo() {
  const curlCommands = [
    // 下面的your1-workers.workers.dev 应该改成你另外一个项目的反代域名，两个项目互相保活
    'curl -u admin:password https://your1-workers.workers.dev/start',
    'curl -u admin:password https://your2-workers.workers.dev/start',
    // 复制上面的命令，可以添加更多
  ];

  try {
    const results = await Promise.all(curlCommands.map(command => executeCurlCommand(command)));

    results.forEach((result, index) => {
      console.log(`命令${index + 1}执行成功: ${result}`);
    });
  } catch (error) {
    console.error(`至少有一个命令执行错误: ${error}`);
  }
}

// 每隔5分钟执行一次保活函数
setInterval(baohuo, 5 * 60000);


```
