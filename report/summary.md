# DBTune thoughts

DBTune is still a product that is in development, as such, there are several
things to improve on the usage of the tool.

## Usage

### Configuration Schema

The configuration file of the service is provided as a yaml file. This is a
standard file format which is good since most people are familiar with it.
However, the `connection_string` field looks unintuitive compared to other
types of database connection strings. The key value pair nature of the custom
format can simply be defined as key value pairs in yaml.

Another thing that will improve the usage of the tool is to provide a schema.
This can be easily provided by using existing libraries such as `pydantic`.
From the source code, the current configuration parsing is done manually.

### Super User

The software requires you to provide it with super user access. Without looking
as the source code too deeply, it has probably has something to do with the 
tools used to access the server configuration. However. as as administrator,
it can be intimidating to see a tool that requires this much permission, 
especially since it is a database tuning tool rather than a server tooling.

### Non-local Support

While the DBTune manual provides a way to limit the resources used by the tool,
the tool still takes away resources from the database. This means that the
result of the tuning may not translate into real world usage. To ensure that
the performance uplift from the tuning is not impacted by the tool, it would
be better if the tool can run on remote database instances.

For the metrics require the server specifications, some of them can be
provided by the RDBMS or provided by the user.

### Redownloading

The current website dashboard tells you to redownload the entire package when
you have to tune a second database. However, looking at the source code, the
only difference is the `db_id` field provided in the `default_config.yml`
inside the file.

This should be transparent to the user, as this tool will probably be
integrated to different deployment toolchains and as such, providing what
really needs to be changed will allows further integration to these toolchains.

### Installable Library

Currently, the DBTune is a downloadable source code file. While that works,
being available on Python's main library distribution tool will definitely
help. Not only you get a free page containing the description of the program,
but you also allow people to easily discover and try the tooling.

### Overriding Hardware Specifications

The example setup recommends to use cloud virtual machines order to make the
tool work, however, different teams will have different setup depending on
their infrastructure. For example, on a container based approach, the tool 
always reports the configuration based on the host machine rather than the
container limits. The way to determine the type of disk also fails on
container volumes.

## Tuning

### Explainable Tuning

Without much knowledge of the DBTune optimization algorithm, the assumption to
have is that its based on a more advance version of the HyperMapper. 
Specifically, the assumption is that it uses a modeling approach as the
optimization is being performed.

Given that assumption, providing some reasoning from the model on why the 
iteration arrived on the best metrics is going to be really helpful. Database
administrators have also built intuition on how databases work over the years
and being able to show a model that matches those intuition will help them
accept the suggestions and trust the tuning model more.

### Oneshot Tuning

To further develop the optimization strategy, it looks like the next step for
the tool from an algorithm perspective is to develop a oneshot tuning model.
After being able to see different workloads and different types of database
sizes, the model should be able to incorporate these existing knowledge into 
new database tuning needs. This would allow the tool to get the best 
performance with the fraction of the current runtime.

### Replay Support

Replays allow replication of real world work load. It also allows replication
of real world cases where the target database configuration is having an issue.
Currently, DBTune works by exposing the database into continuous workloads and
if the workload stops during the tuning, the tuning result also changes.

By allowing replay support, the tool can save the state of the database at the
start of each run. From then, the tool can run the replays while it's 
performing the tuning. This means that each optimization step will have the
same exact workload.

While this may not be an issue, but for example, iteration 1 contains less
data than iteration 30, which depending on the database structure can have
implications of the performance difference between those iteration.

## Extensions

### Out of Tuning Monitoring

This is more of a vertical integration type of extension, but being able to
provide more statistics makes DBTune an all-in-one solution for looking at
database performance. Possible statistics are the queries that are taking a
long time to run or the peak hours of the database.

This means that for example, the dashboard can also suggest that workloads at
certain times should be tried for optimization. Or that some queries might
be better optimized further as they are taking a lot of resources.

### Dynamic Tuning

Being able to tune a database in real time base on its current performance
would be the a further iteration of the product. While quite different in
approach, some big data tooling like spark also performs some predictions on
the fly to further improve the performance of the queries.

In this case, DBTune might be able to provide a new configuration if the system
is shown to be under load or if the system is in idle status. However, this is
more of a hypothesis and the difference with dynamic tuning might not be that
large if the tool arrives at an `optimal` configuration.

### Database Design Support

Unfortunately, there are a lot of database in production that are not designed
optimally. Further in the future, if the tool needs to improve its database
performance capability, it also has to look at the existing database design and
the queries being run through it.

Though from a research perspective, there is probably some work to be done to
be able to encode database structures and queries into a dense numeric values 
that a model can utilize.
