Tasks are the building blocks of Celery applications.

A task is a class that can be created out of any callable. It performs dual roles in that it defines both what happens when a task is called (sends a message), and what happens when a worker receives that message.

Every task class has a unique name, and this name is referenced in messages so the worker can find the right function to execute.

A task message is not removed from the queue until that message has been [acknowledged](https://docs.celeryq.dev/en/stable/glossary.html#term-acknowledged) by a worker. A worker can reserve many messages in advance and even if the worker is killed – by power failure or some other reason – the message will be redelivered to another worker.

Ideally task functions should be [idempotent](https://docs.celeryq.dev/en/stable/glossary.html#term-idempotent): meaning the function won’t cause unintended effects even if called multiple times with the same arguments. Since the worker cannot detect if your tasks are idempotent, the default behavior is to acknowledge the message in advance, just before it’s executed, so that a task invocation that already started is never executed again.

If your task is idempotent you can set the [`acks_late`](https://docs.celeryq.dev/en/stable/userguide/tasks.html#Task.acks_late "Task.acks_late") option to have the worker acknowledge the message _after_ the task returns instead

Note that the worker will acknowledge the message if the child process executing the task is terminated (either by the task calling [`sys.exit()`](https://docs.python.org/dev/library/sys.html#sys.exit "(in Python v3.12)"), or by signal) even when [`acks_late`](https://docs.celeryq.dev/en/stable/userguide/tasks.html#Task.acks_late "Task.acks_late") is enabled. This behavior is intentional as…

1.  We don’t want to rerun tasks that forces the kernel to send a `SIGSEGV` (segmentation fault) or similar signals to the process.  
2.  We assume that a system administrator deliberately killing the task does not want it to automatically restart.
3.  A task that allocates too much memory is in danger of triggering the kernel OOM killer, the same may happen again.
4.  A task that always fails when redelivered may cause a high-frequency message loop taking down the system.

# Basic

You can easily create a task from any callable by using the [`app.task()`](https://docs.celeryq.dev/en/stable/reference/celery.html#celery.Celery.task "celery.Celery.task") decorator:

```python
from .models import User

@app.task
def create_user(username, password):
    User.objects.create(username=username, password=password)

```

There are also many [options](https://docs.celeryq.dev/en/stable/userguide/tasks.html#task-options) that can be set for the task, these can be specified as arguments to the decorator:

```python
@app.task(serializer='json')
def create_user(username, password):
    User.objects.create(username=username, password=password)
```


# Bound Task
A task being bound means the first argument to the task will always be the task instance (`self`), just like Python bound methods:

```python
logger = get_task_logger(__name__)

@app.task(bind=True)
def add(self, x, y):
    logger.info(self.request.id)
```

# Names
Every task must have a unique name.

If no explicit name is provided the task decorator will generate one for you, and this name will be based on 1) the module the task is defined in, and 2) the name of the task function.

Example setting explicit name:

```python
>>> @app.task(name='sum-of-two-numbers')
>>> def add(x, y):
...     return x + y

>>> add.name
'sum-of-two-numbers'
```

# Retrying

[`app.Task.retry()`](https://docs.celeryq.dev/en/stable/reference/celery.app.task.html#celery.app.task.Task.retry "celery.app.task.Task.retry") can be used to re-execute the task, for example in the event of recoverable errors.

When you call `retry` it’ll send a new message, using the same task-id, and it’ll take care to make sure the message is delivered to the same queue as the originating task.

When a task is retried this is also recorded as a task state, so that you can track the progress of the task using the result instance (see [States](https://docs.celeryq.dev/en/stable/userguide/tasks.html#task-states)).

Here’s an example using `retry`:

```python
@app.task(bind=True)
def send_twitter_status(self, oauth, tweet):
    try:
        twitter = Twitter(oauth)
        twitter.update_status(tweet)
    except (Twitter.FailWhaleError, Twitter.LoginError) as exc:
        raise self.retry(exc=exc)
```
