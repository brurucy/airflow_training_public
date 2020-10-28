Upstream tasks = pointing to the task dependencies
Downstream = marking the task a dependency of other tasks

Task = manages the execution of an operator

part 1 - Tutorial on how to create your first DAG

cron.

\* \* \* \* \*

https://crontab.guru/

part 2 - templating

airflow context?

backfilling/catchup

atomicity

Idempotency ensures tasks can be rerun while producing the same output results. / use partitioning.

a key to idempotency is using the airflow context.

templating is very important

in order to template the python operator you have to set provide_context = True

apparently in Python, if you have kwargs, and you put a specific argument within the kwags in the function signature it will recognise it, putting all other arguments in the kwargs.

op_args for the Python Operator..or op_kwargs where you can actually name it

First part of the lecture, taking a pg_dump

Keep data out of airflow!! but if you really really need it use XCOM.

template_searchpath

adding pgsql

airflow connections --add \
--conn_id my_postgres \
--conn_type postgres \
--conn_host localhost \
--conn_login postgres \
--conn_password mysecretpassword

one to multiple

fan out
start >> \[task_one, task_two\]

fan in
\[task_one, task_two\] >> task_three

branch_operator. check the book for an example

```python

def _pick_erp_system(**context):
	if context["execution_date"] < ERP_SWITCH_DATE:
	return "fetch_sales_old" else:
	return "fetch_sales_new"
	
	
sales_branch = BranchPythonOperator( task_id='sales_branch',
	provide_context=True,
	python_callable=_pick_erp_system, )
	
sales_branch >> [fetch_sales_old, fetch_sales_new]
```

trigger rules.

trigger_rule="none_failed" for the branch operator

Sensors. File and Python sensor

sensor deadlock fix = reschedule

https://airflow.readthedocs.io/en/stable/_api/airflow/operators/bash_operator/index.html

triggered dags run without black border, while scheduled ones have a black border.

