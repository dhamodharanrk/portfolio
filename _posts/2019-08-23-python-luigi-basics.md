---
title: Getting Started with Luigi
tags: [Python,ETL,Luigi]
style: fill
color: primary
description: "Getting Started with Luigi"
---

# Getting Started with Luigi

Luigi is a Python (2.7, 3.6, 3.7 tested) package that **helps you build complex pipelines of batch jobs (automated)**. It handles dependency resolution, workflow management, visualization, handling failures, command line integration, and much more.

An **automatic process** is usually a sequence of tasks (batch jobs). Something like “get some data”, “transform data” and “use data”. This also has a fancier name: **pipeline (of batch jobs).**

For example, a **pipeline** that consists into 3 separate batch jobs and each job has its own **dependencies**

[![Mock pipeline](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/1_mDxybyI5YY76mYBn5O7GqQ.png "Mock pipeline")](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/1_mDxybyI5YY76mYBn5O7GqQ.png "Mock pipeline")

The arrows represent dependency, that means that “transform data” must only be executed after “get some data” is complete. There is a fancy name for this kind of structure: **Directed Acyclic Graph (or DAG)**, which in short, is just a “one way” graph.


**Some of the useful features of Luigi include:**

- Dependency management
- Checkpoints / Failure recovery
- CLI integration / parameterisation
- Dependency Graph visualisation

## **Implementation**

There are **two core concepts** to understand how we can apply Luigi to our own data pipeline: 

- **Tasks**
- **Targets**

A **task is a unit of work**, designed by extending the class luigi.Task and overriding some basic methods. **The output of a task is a target**, which can be a file on the local filesystem, a file on Amazon’s S3, some piece of data in a database etc.

Example code look like,

    import luigi
     
    class PrintNumbers(luigi.Task):
     
        def requires(self):
            return []
     
        def output(self):
            return luigi.LocalTarget("numbers_up_to_10.txt")
     
        def run(self):
            with self.output().open('w') as f:
                for i in range(1, 11):
                    f.write("{}\n".format(i))
     
    class SquaredNumbers(luigi.Task):
     
        def requires(self):
            return [PrintNumbers()]
     
        def output(self):
            return luigi.LocalTarget("squares.txt")
     
        def run(self):
            with self.input()[0].open() as fin, self.output().open('w') as fout:
                for line in fin:
                    n = int(line.strip())
                    out = n * n
                    fout.write("{}:{}\n".format(n, out))
                     
    if __name__ == '__main__':
        luigi.run()

The programe can run via two modes, One is local-scheduler and other is for production environment.

To run in local or test environment,,

`python luigi_example.py SquaredNumbers --local-scheduler`

The first argument we’re passing to Luigi is the name of the last task in the pipeline we want to run

### Running on server

You can run the Luigi scheduler daemon in the foreground with

`$ luigid`

or in the background with:

`$ luigid --background`

It will default to port 8082, so you can point your browser to http://localhost:8082 to access the visualisation.

[![Luigi UI](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/luigi_UI.JPG "Luigi UI")](https://raw.githubusercontent.com/dhamodharanrk/examples/master/blog_files/luigi_UI.JPG "Luigi UI")

With the global Luigi scheduler running, we can re-run the code without the option for the local scheduler:

`$ python run_luigi.py SquaredNumbers --n [BIG_NUMBER]`

Programe also, takes the input from the command line as below,

    class PrintNumbers(luigi.Task):
        n = luigi.IntParameter()
     
        def requires(self):
            return []
     
        def output(self):
            return luigi.LocalTarget("numbers_up_to_{}.txt".format(self.n))
     
        def run(self):
            with self.output().open('w') as f:
                for i in range(1, self.n+1):
                    f.write("{}\n".format(i))
     
    class SquaredNumbers(luigi.Task):
        n = luigi.IntParameter()
     
        def requires(self):
            return [PrintNumbers(n=self.n)]
     
        def output(self):
            return luigi.LocalTarget("squares_up_to_{}.txt".format(self.n))
     
        def run(self):
            with self.input()[0].open() as fin, self.output().open('w') as fout:
                for line in fin:
                    n = int(line.strip())
                    out = n * n
                    fout.write("{}:{}\n".format(n, out))

for n (user-input), we can also set a default value as 

`n = luigi.IntParameter(default=10)`


Its also important to consider its **limitations**:

- It was built for batch jobs, it’s probably not useful for near real-time processing
- It doesn’t trigger the execution for you, you still need to run the data pipeline (e.g. via a cronjob)
