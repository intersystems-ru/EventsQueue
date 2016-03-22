# EventsQueue
Sample of making queue for processing tasks based on InsterSystems Caché %SYSTEM.Event   

Example:

```
/// Starts the queue for processing reports
/// pAgents - number of agents to process the queue, default value = 2
/// pProcess - process tasks that were made before queue has started. 1 - process, 0 - ignore.
ClassMethod StartQueue(pAgents As %Integer = 2, pProcess As %Integer = 0) As %Status
{
	set st = $$$OK
	try {
		$$$TOE(st, ##class(Manager).Start(..#EVENTRESOURCE, 2, $classname(), "ProcessTask"))
		if (pProcess = 1) {
			set id = 0
			while (id < 10) {
				set id = id + 1
				set sent = $system.Event.Signal(..#EVENTRESOURCE, id)
    				if ('sent) $$$ThrowStatus($$$ERROR("Unable to start task processing for id = "_id))
			}
		}
	} catch ex {
		set st = ex.AsStatus()
	}
	quit st
}

/// Stops the queue
ClassMethod StopQueue() As %Status
{
	set st = $$$OK
	try {
		$$$TOE(st, ##class(Manager).Stop(..#EVENTRESOURCE))
	} catch ex {
		set st = ex.AsStatus()
	}
	quit st
}
```
