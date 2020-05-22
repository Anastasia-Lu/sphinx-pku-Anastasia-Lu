

# Common Questions and Trouble Shooting

this document lists a commonly asked question when using TICDC and shows an operation solution. It also summerizes some common operation troubles and their solutions.
## How to choose start-ts when starting a task
First, you should know that the `start-ts` of a sychronizing task corresponds to a TSO of the upstring TiDB cluster. The sychronizing task will start its data request from this TSO. Therefore, the `start-ts` should meet two conditions below:
- the value of `start-ts` should be larger than current `tikv_gc_safe_point`, otherwise, an error would occor when creating the task.


- While starting the task, you should ensure that the downstring has all the previous data before start-ts. You can loose this requirement accordingly, when the sycronizing task is for message queue or other senarios, in which strict data consistensy between upstring and downstring is not required.

If `start-ts` is not assigned or assigned 0 `start-ts=0`, when the task gets started, will get the current TSO from PD and start sycronizing from this TSO.

## When a task gets started, it prompts that some tables cannot be synchronized

When  using `cdc cli changefeed create` to create a sychronization task, it will check if the upstring tables meet the requirement of sychronization limits. If not, it will prompt that `some tables are not eligible to repicate` and list out the unqualified tables. If you choose `Y` or `y`, you will continue creating the sychronization task and automatically ignore all the updates of these unqualified tables. If you choose other inputs, the sychronization task will not be created.
