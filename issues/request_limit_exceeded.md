### Title
Job ID 587505430640001 error: REQUEST_LIMIT_EXCEEDED

### Description
Error Encountered:

```
Py4JJavaError: An error occurred while calling o35.run.
: com.databricks.WorkflowException: com.databricks.common.client.DatabricksServiceHttpClientException: REQUEST_LIMIT_EXCEEDED: There are already 1 active runs (limit: 1).
	at com.databricks.workflow.WorkflowDriver.run(WorkflowDriver.scala:98)
```

Job ID: 587505430640001

### Suggested Solution:
- Revert to a single notebook that orchestrates silver and gold layer.
- Increase the number of multiple tasks to 7.

### Next Steps:
- Investigate current run setup and task concurrency limits.
- Refactor workflow as described above if feasible.