# The twelve-factor methodology

methodology for building software-as-a-service apps. The twelve-factor methodology can be applied to apps written in any programming language, and which use any combination of backing services (database, queue, memory cache, etc).

## The Twelve Factors

### Codebase

> One codebase tracked in revision control, many deploys.

**Multiple codebases** - it’s not an app, it’s a distributed system. Each component in a distributed system is an app, and each can individually comply with twelve-factor.

**Multiple apps** sharing the same code is a violation of twelve-factor. The solution here is to factor shared code into libraries which can be included through the dependency manager.

**Deploy** - a running instance of the app. This can be a production, stagin or local development copy.

The codebase is the same across all deploys, although different versions may be active in each deploy.

### Dependencies

> Explicitly declare and isolate dependencies

**A twelve-factor app never relies on implicit existence of system-wide packages.** It declares all dependencies, completely and exactly, via a *dependency declaration* manifest. Furthermore, it uses a *dependency isolation* tool during execution to ensure that no implicit dependencies “leak in” from the surrounding system. No matter what the toolchain, dependency declaration and isolation must always be used together – only one or the other is not sufficient to satisfy twelve-factor.

One benefit of explicit dependency declaration is that it simplifies setup for developers new to the app. The new developer can check out the app’s codebase onto their development machine, requiring only the language runtime and dependency manager installed as prerequisites.

Twelve-factor apps also do not rely on the implicit existence of any system tools. Examples include shelling out to ImageMagick or `curl`. While these tools may exist on many or even most systems, there is no guarantee that they will exist on all systems where the app may run in the future, or whether the version found on a future system will be compatible with the app. If the app needs to shell out to a system tool, that tool should be vendored into the app.

### Config

> Store config in the environment

An app’s *config* is everything that is likely to vary between [deploys](https://12factor.net/codebase) (staging, production, developer environments, etc). 

**storing config as constants in the code** - a violation of twelve-factor, which requires **strict separation of config from code**. Config varies substantially across deploys, code does not.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.

Types of configs

- **config files which are not checked into revision control** - This is a huge improvement over using constants which are checked into the code repo, but still has weaknesses: it’s easy to mistakenly check in a config file to the repo; there is a tendency for config files to be scattered about in different places and different formats

- **environment variables** (often shortened to *env vars* or *env*). Env vars are easy to change between deploys without changing any code; unlike config files, there is little chance of them being checked into the code repo accidentally; and unlike custom config files, or other config mechanisms such as Java System Properties, they are a language- and OS-agnostic standard.

- **grouping** - batch config into named groups (often called “environments”) named after specific deploys, such as the `development`, `test`, and `production` environments. This method does not scale cleanly: as more deploys of the app are created, new environment names are necessary, such as `staging` or `qa`. As the project grows further, developers may add their own special environments like `joes-staging`, resulting in a combinatorial explosion of config which makes managing deploys of the app very brittle.

### Backing services

> Treat backing services as attached resources

A *backing service* is any service the app consumes over the network as part of its normal operation.

Examples include datastores (such as [MySQL](http://dev.mysql.com/) or [CouchDB](http://couchdb.apache.org/)), messaging/queueing systems (such as [RabbitMQ](http://www.rabbitmq.com/) or [Beanstalkd](https://beanstalkd.github.io)), SMTP services for outbound email (such as [Postfix](http://www.postfix.org/)), and caching systems (such as [Memcached](http://memcached.org/)).

The app may also have services provided and managed by third parties. Examples include SMTP services (such as [Postmark](http://postmarkapp.com/)), metrics-gathering services (such as [New Relic](http://newrelic.com/) or [Loggly](http://www.loggly.com/)), binary asset services (such as [Amazon S3](http://aws.amazon.com/s3/)), and even API-accessible consumer services (such as [Twitter](http://dev.twitter.com/), [Google Maps](https://developers.google.com/maps/), or [Last.fm](http://www.last.fm/api)).

**The code for a twelve-factor app makes no distinction between local and third party services.** To the app, both are attached resources, accessed via a URL or other locator/credentials stored in the [config](https://12factor.net/config). A [deploy](https://12factor.net/codebase) of the twelve-factor app should be able to swap out a local MySQL database with one managed by a third party (such as [Amazon RDS](http://aws.amazon.com/rds/)) without any changes to the app’s code. Likewise, a local SMTP server could be swapped with a third-party SMTP service (such as Postmark) without code changes. In both cases, only the resource handle in the config needs to change.

**Each distinct backing service is a *resource***. For example, a MySQL database is a resource; two MySQL databases qualify as two distinct resources.

### Build, release, run

> Strictly separate build and run stages

A [codebase](https://12factor.net/codebase) is transformed into a (non-development) deploy through three stages:

- *build stage* - transform which converts a code repo into an executable bundle known as a *build*. Using a version of the code at a commit specified by the deployment process, the build stage fetches vendors [dependencies](https://12factor.net/dependencies) and compiles binaries and assets.
- *release stage* - takes the build produced by the build stage and combines it with the deploy’s current [config](https://12factor.net/config). The resulting *release* contains both the build and the config and is ready for immediate execution in the execution environment.
- *run stage* - runs the app in the execution environment, by launching some set of the app’s [processes](https://12factor.net/processes) against a selected release.

**strict separation between the build, release, and run stages.** For example, it is impossible to make changes to the code at runtime, since there is no way to propagate those changes back to the build stage.

Every release should always have a unique release ID, such as a timestamp of the release (such as `2011-04-06-20:32:17`) or an incrementing number (such as `v100`). Releases are an append-only ledger and a release cannot be mutated once it is created. Any change must create a new release.

The run stage should be kept to as few moving parts as possible, since problems that prevent an app from running can cause it to break in the middle of the night when no developers are on hand. The build stage can be more complex, since errors are always in the foreground for a developer who is driving the deploy.

### Processes

> Execute the app as one or more stateless processes

The app is executed in the execution environment as one or more *processes*.

**Twelve-factor processes are stateless and [share-nothing](http://en.wikipedia.org/wiki/Shared_nothing_architecture).** Any data that needs to persist must be stored in a stateful [backing service](https://12factor.net/backing-services), typically a database.

The memory space or filesystem of the process can be used as a brief, single-transaction cache.

The twelve-factor app never assumes that anything cached in  memory or on disk will be available on a future request or job – (served by a different process, a restart triggered by code deploy, config change,... will clean cache)

A twelve-factor app prefers to do asset compiling during the [build stage](https://12factor.net/build-release-run).

[“sticky sessions”](http://en.wikipedia.org/wiki/Load_balancing_(computing)#Persistence) – that is, caching user session data in memory of the app’s process and expecting future requests from the same visitor to be routed to the  same process. Sticky sessions are a violation of twelve-factor and  should never be used or relied upon.

### Port binding

> Export services via port binding

The twelve-factor app is completely self-contained**  and does not rely on runtime injection of a webserver into the execution environment to create a web-facing service. The web app **exports HTTP as a service by binding to a port**, and listening to requests coming in on that port.

HTTP is not the only service that can be exported by port binding.  Nearly any kind of server software can be run via a process binding to a port and awaiting incoming requests.

Note also that the port-binding approach means that one app can become the [backing service](https://12factor.net/backing-services) for another app, by providing the URL to the backing app as a resource handle in the [config](https://12factor.net/config) for the consuming app.

### Concurrency

> Scale out via the process model

**In the twelve-factor app, processes are a first class citizen.** Processes in the twelve-factor app take strong cues from [the unix process model for running service daemons](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/). Using this model, the developer can architect their app to handle diverse workloads by assigning each type of work to a *process type*. For example, HTTP requests may be handled by a web process, and long-running background tasks handled by a worker process.

This does not exclude individual processes from handling their own  internal multiplexing, via threads inside the runtime VM.

The process model truly shines when it comes time to scale out. The [share-nothing, horizontally partitionable nature of twelve-factor app processes](https://12factor.net/processes) means that adding more concurrency is a simple and reliable operation.  The array of process types and number of processes of each type is known as the *process formation*.

Twelve-factor app processes [should never daemonize](http://dustin.github.com/2010/02/28/running-processes.html) or write PID files. Instead, rely on the operating system’s process manager to manage [output streams](https://12factor.net/logs), respond to crashed processes, and handle user-initiated restarts and shutdowns.

### Disposability

> Maximize robustness with fast startup and graceful shutdown

**The twelve-factor app’s [processes](https://12factor.net/processes) are \*disposable\*, meaning they can be started or stopped at a moment’s notice.** This facilitates fast elastic scaling, rapid deployment of [code](https://12factor.net/codebase) or [config](https://12factor.net/config) changes, and robustness of production deploys.

Processes should strive to **minimize startup time**. Short startup time provides more agility for the [release](https://12factor.net/build-release-run) process and scaling up; and it aids robustness, because the process  manager can more easily move processes to new physical machines when  warranted.

Processes **shut down gracefully when they receive a [SIGTERM](http://en.wikipedia.org/wiki/SIGTERM)** signal from the process manager.

For a worker process, graceful shutdown is achieved by returning the current job to the work queue.

Processes should also be **robust against sudden death**, in the case of a failure in the underlying hardware. While this is a  much less common occurrence than a graceful shutdown with `SIGTERM`, it can still happen. Either way, a twelve-factor app is  architected to handle unexpected, non-graceful terminations. [Crash-only design](http://lwn.net/Articles/191059/) takes this concept to its [logical conclusion](http://docs.couchdb.org/en/latest/intro/overview.html).

### Dev/prod parity

> Keep development, staging, and production as similar as possible

Historically, there have been substantial gaps between development and production. These gaps manifest in three areas:

- **The time gap**: A developer may work on code that takes days, weeks, or even months to go into production.
- **The personnel gap**: Developers write code, ops engineers deploy it.
- **The tools gap**: Developers may be using a stack like Nginx, SQLite, and OS X, while the production deploy uses Apache, MySQL, and Linux.

**The twelve-factor app is designed for [continuous deployment](http://avc.com/2011/02/continuous-deployment/) by keeping the gap between development and production small.** Looking at the three gaps described above:

- Make the time gap small: a developer may write code and have it deployed hours or even just minutes later.
- Make the personnel gap small: developers who wrote code are closely  involved in deploying it and watching its behavior in production.
- Make the tools gap small: keep development and production as similar as possible.

Developers sometimes find great appeal in using a lightweight backing  service in their local environments, while a more serious and robust  backing service will be used in production. For example, using SQLite  locally and PostgreSQL in production; or local process memory for  caching in development and Memcached in production.

**The twelve-factor developer resists the urge to use different backing services between development and production**, even when adapters theoretically abstract away any differences in  backing services. Differences between backing services mean that tiny  incompatibilities crop up, causing code that worked and passed tests in  development or staging to fail in production. These types of errors  create friction that disincentivizes continuous deployment.

### Logs

> Treat logs as event streams

Logs are the [stream](https://adam.herokuapp.com/past/2011/4/1/logs_are_streams_not_files/) of aggregated, time-ordered events collected from the output streams of all running processes and backing services. Logs in their raw form are  typically a text format with one event per line (though backtraces from  exceptions may span multiple lines). Logs have no fixed beginning or  end, but flow continuously as long as the app is operating.

**A twelve-factor app never concerns itself with routing or storage of its output stream.** It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to `stdout`.

In staging or production deploys, each process’ stream will be  captured by the execution environment, collated together with all other  streams from the app, and routed to one or more final destinations for  viewing and long-term archival. These archival destinations are not  visible to or configurable by the app, and instead are completely  managed by the execution environment. Open-source log routers (such as [Logplex](https://github.com/heroku/logplex) and [Fluentd](https://github.com/fluent/fluentd)) are available for this purpose.

The event stream for an app can be routed to a file, or watched via  realtime tail in a terminal. Most significantly, the stream can be sent  to a log indexing and analysis system such as [Splunk](http://www.splunk.com/), or a general-purpose data warehousing system such as [Hadoop/Hive](http://hive.apache.org/). These systems allow for great power and flexibility for introspecting an app’s behavior over time, including:

- Finding specific events in the past.
- Large-scale graphing of trends (such as requests per minute).
- Active alerting according to user-defined heuristics (such as an  alert when the quantity of errors per minute exceeds a certain  threshold).

# Resources

-  [The Twelve Factors.md](../sources/The Twelve Factors/The Twelve Factors.md) 

