# 12 Factor

1. **Codebase** - There should always be one-to-one co-relation between your code and your app.


2. **Dependencies** - A twelve-factor app never relies on implicit existence of system-wide packages. It declares all dependencies, completely and exactly, via a dependency declaration manifest. (pip in python)


3. **Config** -Strict separation of config from code. Config varies substantially across deploys, code does not.The twelve-factor app stores config in environment variables


4. **Backing Services** - A backing service is any service the app consumes over the network as part of its normal operation. Eg- MySQL, Redis.


5. **Processes** - Twelve-factor processes are stateless and share-nothing. Any data that needs to persist must be stored in a stateful backing service, typically a database.


6. **Port Binding** - The twelve-factor app is completely self-contained and does not rely on runtime injection of a webserver into the execution environment to create a web-facing service. The web app exports HTTP as a service by binding to a port, and listening to requests coming in on that port.


7. **Build, release, run** - The twelve-factor app uses strict separation between the build, release, and run stages. For example, it is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.


8. **Concurrency** - In the twelve-factor app, processes are a first class citizen. Processes in the twelve-factor app take strong cues from the unix process model for running service daemons. **Using this model, the developer can architect their app to handle diverse workloads by assigning each type of work to a process type. For example, HTTP requests may be handled by a web process, and long-running background tasks handled by a worker process.**
**An individual VM can only grow so large (vertical scale), so the application must also be able to span multiple processes running on multiple physical machines.**


9. **Disposibility** - The twelve-factor app’s processes are disposable, meaning they can be started or stopped at a moment’s notice. This facilitates fast elastic scaling, rapid deployment of code or config changes, and robustness of production deploys.
**Processes should strive to minimize startup time.**  


10. **Dev/Prod Parity** - The twelve-factor app is designed for continuous deployment by keeping the gap between development and production small. Looking at the three gaps described above:

**Make the time gap small**: a developer may write code and have it deployed hours or even just minutes later.
**Make the personnel gap small**: developers who wrote code are closely involved in deploying it and watching its behavior in production.
**Make the tools gap small**: keep development and production as similar as possible.


11. **Logs** - A twelve-factor app never concerns itself with routing or storage of its output stream. It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to **stdout**. During local development, the developer will view this stream in the foreground of their terminal to observe the app’s behavior.


12. **Admin Processes** - Run admin/management tasks as one-off processes
**python manage.py migrate** should be a part of the code.
