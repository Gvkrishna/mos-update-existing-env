## updating the existing liv env
 
 Log in to the Fuel Master node CLI as root.

Run an environment integrity check using Cudet:
~~~
cudet -e <ENV_ID>
~~~

To find the environment ID, run 
~~~
fuel env.
~~~

  --Note

    If you use non-default Fuel credentials, update the /usr/lib/python2.7/site-packages/cudet/default_config.yaml file with your    details before running the cudet -e <ENV_ID> command.

The Cudet output contains the versions’ verification analysis, the built-in MD5 verification analysis, and the potential updates if available.

##Prepare your environment for the Noop run:

~~~~
update-prepare prepare env <ENV_ID>
~~~~

  -This command updates nailgun-mcagents on each node of your environment.

Run the configuration check on your environment using Noop run.

  Note

  If your environment does not contain customizations, skip to step 6.

~~~~
fuel2 env redeploy --noop <ENV_ID>
~~~~

  -It may take a while for the task to complete. When the task succeeds, its status changes from running to ready.

##To verify the task status,run 

~~~~
fuel2 task show <TASK_ID>. 
~~~~
  -The task ID is specified in the output of the configuration check command.
##Verify the summary of the configuration check task:

~~~~
fuel2 task history show <TASK_ID> --include-summary
~~~~

  -The task ID is specified in the output of the fuel2 task list command. The name of the task is dry_run_deployment.

The Noop run reports are stored on each OpenStack node in the /var/lib/puppet/reports/<NODE-FQDN>/<TIMESTAMP>.yaml directory.

##Warning

  -All environment customizations will be lost during the update process. Therefore, use the customization detection reports made by Noop run and Cudet to make a decision on whether it is worth proceeding with the update.

##Update the environment:

~~~~
fuel2 update --env <ENV_ID> install
~~~~
  -Optionally, add the **--restart-rabbit** and **--restart-mysql** arguments to the command to restart RabbitMQ and MySQL automatically. Otherwise, these services will not be restarted unless their configurations change during the update.

##To verify the update progress, use the Fuel web UI Dashboard tab or run 

~~~~
fuel2 task show <TASK_ID>. 
~~~~
  -The task ID is specified in the output of the fuel2 update –env <ENV_ID> install command.

##Verify that you have the latest packages included in Mirantis OpenStack 9.1:
~~~
cudet -e <ENV_ID>
~~~


The system response should be the following:

Potential updates: ALL NODES UP-TO-DATE

Manually add your old environment customizations to your updated environment using the Cudet and Puppet Noop run reports.


