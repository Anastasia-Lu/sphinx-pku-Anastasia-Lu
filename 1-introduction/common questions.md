

# Common Questions and Trouble Shooting

this document lists a commonly asked question when using TICDC and shows an operation solution. It also summerizes some common operation troubles and their solutions.
## How to choose start-ts when starting a task
First, you should know that the `start-ts` of a replication task corresponds to a TSO of the upstream TiDB cluster. The replication task will start its data request from this TSO. Therefore, the `start-ts` should meet two conditions below:
- the value of `start-ts` should be larger than current `tikv_gc_safe_point`, otherwise, an error would occor when creating the task.


- While starting the task, you should ensure that the downstream has all the previous data before start-ts. You can loose this requirement accordingly, when the replication task is for message queue or other senarios, in which strict data consistensy between upstream and downstream is not required.

If `start-ts` is not specified or specified 0 `start-ts=0`, when the task gets started, it will get the current TSO from PD and start to replicate data from this TSO.

## When a task gets started, it prompts that some tables cannot be synchronized

When  using `cdc cli changefeed create` to create a replication task, it will check if the upstream tables comply with the [restrictions](/ticdc/ticdc-overview.md#restrictions). If not, it will prompt that `some tables are not eligible to repicate` and list out the ineligible tables. If you choose `Y` or `y`, you will continue creating the sychronization task and automatically ignore all the updates of these ineligible tables. If you choose other inputs, the replication task will not be created.
