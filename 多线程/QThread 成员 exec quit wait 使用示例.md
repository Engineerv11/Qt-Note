QThread 成员 exec quit wait 使用示例
=======

```c++
#include <QCoreApplication>
#include <QtDebug>
#include <QThread>

class Thread : public QThread
{
public:
    void run() override
    {
        qDebug() << "thread started";

        exec();
        qDebug() << "exec";

        QThread::sleep(7);
        qDebug() << "thread ended";
    }
};

int main(int argc, char *argv[])
{
    QCoreApplicaion a(argc, argv);

    qDebug() << "Main thread started";

    Thread thread;

    thread.start();

    QThread::sleep(7);

    // 退出线程的事件循环
    thread.quit();
    qDebug() << "quit";

    // 等待线程结束
    thread.wait();
    qDebug() << "wait";

    qDebug() << "Main thread ended";

    return a.exec();
}
```

