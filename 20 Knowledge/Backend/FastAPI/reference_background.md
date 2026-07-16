---
title: "Background"
source: "https://fastapi.tiangolo.com/reference/background/"
---

# Background Tasks - `BackgroundTasks`¶

You can declare a parameter in a _path operation function_ or dependency function with the type `BackgroundTasks`, and then you can use it to schedule the execution of background tasks after the response is sent.

You can import it directly from `fastapi`:
[code] 
    from fastapi import BackgroundTasks
    
[/code]

##  `` fastapi.BackgroundTasks ¶
[code] 
    BackgroundTasks(tasks=None)
    
[/code]

Bases: `BackgroundTasks`

A collection of background tasks that will be called after a response has been sent to the client.

Read more about it in the [FastAPI docs for Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/).

#### Example¶
[code] 
    from fastapi import BackgroundTasks, FastAPI
    
    app = FastAPI()
    
    
    def write_notification(email: str, message=""):
        with open("log.txt", mode="w") as email_file:
            content = f"notification for {email}: {message}"
            email_file.write(content)
    
    
    @app.post("/send-notification/{email}")
    async def send_notification(email: str, background_tasks: BackgroundTasks):
        background_tasks.add_task(write_notification, email, message="some notification")
        return {"message": "Notification sent in the background"}
    
[/code]

Source code in `starlette/background.py`
[code] 
    def __init__(self, tasks: Sequence[BackgroundTask] | None = None):
        self.tasks = list(tasks) if tasks else []
    
[/code]

###  `` func `instance-attribute` ¶
[code] 
    func = func
    
[/code]

###  `` args `instance-attribute` ¶
[code] 
    args = args
    
[/code]

###  `` kwargs `instance-attribute` ¶
[code] 
    kwargs = kwargs
    
[/code]

###  `` is_async `instance-attribute` ¶
[code] 
    is_async = is_async_callable(func)
    
[/code]

###  `` tasks `instance-attribute` ¶
[code] 
    tasks = [list](python-types_#list "List".md)(tasks) if tasks else []
    
[/code]

###  `` add_task ¶
[code] 
    add_task(func, *args, **kwargs)
    
[/code]

Add a function to be called in the background after the response is sent.

Read more about it in the [FastAPI docs for Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/).

PARAMETER | DESCRIPTION  
---|---  
`func` |  The function to call after the response is sent. It can be a regular `def` function or an `async def` function. **TYPE:** `Callable[P, Any]`  
Source code in `fastapi/background.py`
[code] 
    def add_task(
        self,
        func: Annotated[
            Callable[P, Any],
            Doc(
                """
                The function to call after the response is sent.
    
                It can be a regular `def` function or an `async def` function.
                """
            ),
        ],
        *args: P.args,
        **kwargs: P.kwargs,
    ) -> None:
        """
        Add a function to be called in the background after the response is sent.
    
        Read more about it in the
        [FastAPI docs for Background Tasks](https://fastapi.tiangolo.com/tutorial/background-tasks/).
        """
        return super().add_task(func, *args, **kwargs)
    
[/code]
