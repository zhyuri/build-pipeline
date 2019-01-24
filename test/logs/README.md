# How to follow log outputs?

- [How to follow PipelineRun logs?](#pipelinerun)
- [How to follow TaskRun logs?](#taskrun)

`Pipelinerun` creates TaskRuns depending on the definition of underlying
`Pipeline` definition. Each `TaskRun` creates Kubernetes Pod and all the
steps are translated into init containers.

There is a "gotcha" about tailing logs for init container. Logs cannot be
retrieved from pod that has been shut down for a while. In this case tailing
logs will return error `Unable to retrieve container logs`.

```shell
go run test/logs/main.go [-n NAMESPACE] [-pr PIPELINERUN-NAME] / [-tr TASKRUN_NAME] [-f FILE-NAME]
```

## General flags

```
-n string
  	The namespace scope for this CLI request (default "default")
-f string
   	Name of the file to write logs.
```

Command provides option to change namespace with `-n` flag. If user wants to
dump logs to a file then `-f` flag could be used. By default logs are dumped to
`stdout`.

## PipelineRun

The following command will tail logs for all `TaskRuns` created by PipelineRun
`my-pr` in namespace `default`.

```shell
go run test/logs/main.go -pr my-pr
```

## TaskRun

The following command will tail logs for all specified `TaskRun` in namespace
`default`.

```shell
go run test/logs/main.go -tr my-taskrun
```
