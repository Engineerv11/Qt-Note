QThread 创建派生类
=======

thread.h

```c++
#ifndef THREAD_H
#define THREAD_H

#include <QThread>

class Thread : public QThread
{
    Q_OBJECT
public:
    Thread(QObject* parent = nullptr);

signals:
    void done();
    void update_progress(int percent);

protected:
    void run() override;
};

#endif  // THREAD_H
```

thread.cpp

```c++
#include "thread.h"

Thread::Thread(QObject *parent)
{
}

void Thread::run()
{
    for(int i = 0; i <= 10; ++i)
    {
        QThread::msleep(500);
        emit update_progress(i*10);
    }

    emit done();
}
```

window.cpp

```c++
Window::Window(QWidget *parent) : QWidget(parent) , ui(new Ui::Window)
{
    ui->setupUi(this);

    Thread * t = new Thread(this);

    connect(t, &Thread::update_progress, ui->progressBar, &QProgressBar::setValue);

    connect(t, SIGNAL(done()), this, SLOT(thread_finished()));

    t->start();
}
```

