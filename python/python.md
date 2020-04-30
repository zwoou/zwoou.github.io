
# python使用

## 安装第三方模块

使用 pip3 管理
数据科学库 Anaconda [https://www.anaconda.com/distribution/#download-section](https://www.anaconda.com/distribution/#download-section)

## 在centos上安装python3和python2共存

- [https://www.cnblogs.com/JahanGu/p/7452527.html](https://www.cnblogs.com/JahanGu/p/7452527.html)

## APScheduler 定时任务

运行调度任务只需要以下三部曲：

1. 新建一个 schedulers (调度器)
2. 添加一个调度任务(job stores)。
3. 运行调度任务。

下面执行每两秒示例：

```python
import datetime
import time
from apscheduler.schedulers.background import BackgroundScheduler

def timedTask():
    print(datetime.datetime.utcnow().strftime("%Y-%m-%d %H:%M:%S.%f")[:-3])

if __name__ == '__main__':
    # 创建后台执行的 schedulers
    scheduler = BackgroundScheduler()  
    # 添加调度任务
    # 调度方法为 timedTask，触发器选择 interval(间隔性)，间隔时长为 2 秒
    scheduler.add_job(timedTask, 'interval', seconds=2)
    # 启动调度任务
    scheduler.start()

    while True:
        print(time.time())
        time.sleep(5)
```

## requests模块的response.text与response.content有什么区别

- resp.text返回的是Unicode型的数据。
- resp.content返回的是bytes型也就是二进制的数据。