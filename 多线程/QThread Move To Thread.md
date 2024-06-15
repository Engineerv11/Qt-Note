QThread Move To Thread
=======

worker.h

```c++
#ifndef WORKER_H
#define WORKER_H

#include <QObject>
#include <QDebug>
#include <QThread>

class Worker : public QObject
{
    Q_OBJECT
public:
    explicit Worker(QObject *parent = nullptr);

signals:

public slots:
    void do_work();
};

#endif  // WORKER_H
```

worker.cpp

```c++
#include "worker.h"

Worker::Worker(QObject *parent) : QObject(parent)
{
}

void Worker::do_work()
{
    qDebug() << "worker thread id -> " << QThread::currentThreadId();

    // 模拟耗时任务
    for(int i = 0; i < 5; ++i)
    {
        qDebug() << "working..." << i;
        QThread::sleep(2);
    }

    qDebug() << "work done";
}
```

main.cpp

```c++
#include <QCoreApplication>
#include <QObject>
#include <QtDebug>
#include <QThread>

#include "worker.h"

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);

    qDebug() << "Main Thread ID -> " << QThread::currentThreadId();

    // 创建新线程
    QThread *thread = new QThread();

    // 创建工作对象并移动到新线程中执行
    Worker * worker = new Worker();
    worker->moveToThread(thread);

    // 链接信号/槽，当新线程启动时执行 do_work 槽函数
    QObject::connect(thread, &QThread::started, worker, &Worker::do_work);

    // 启动新线程
    thread->start();

    return a.exec();
}
```

