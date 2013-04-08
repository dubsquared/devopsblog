---
layout: post
title: "Are you unknowingly replicating your failure as a DBA?"
date: 2013-04-06 05:27
comments: true
author: Steve Katen 
published: true 
categories: 
- Cloud Servers
- Cloud Monitoring
- Managed Cloud 
---
MySQL replication offers an opportunity to distribute your database to multiple nodes, for additional performance or to off-load specific functionality from your production stack.  When uptime is a requirement table-locking can create unavailability and a long-running nightly backup can cause an unexpected outage.  One benefit of replication is that it allows the backup to happen on the slave without interrupting your production environment.  So what happens when when you rely on this configuration to safeguard your data without constantly ensuring that the replication process is working?  I'll tell you what happens – you're the guy without a recent backup.<!--More--> 

Rackspace Managed Cloud customers rely on this configuration, and it is important that we have an accurate assessment of slave status and the ability to provide notification if there are errors.  We are huge fans of Rackspace Cloud Monitoring so we decided to leverage the plugin functionality of the Cloud Monitoring Agent to provide notification in the event of an issue.  This allows us to provide notification if the slave is in an error status or if the slave lags behind the master.

Utilizing a plugin with the Cloud Monitoring Agent is simple, but first you must [install the agent](http://www.rackspace.com/knowledge_center/article/install-the-cloud-monitoring-agent).  For our scenario we wrote a Python script that will check the slave status and provide metrics back to the Cloud Monitoring environment.  With the plugin written and functional on the Cloud Server we simply add a new check for the plugin and an alarm with our desired criteria.  Here is a curl command to create the check:

```
curl -i -X POST \
-H 'X-Auth-Token: <AUTH_TOKEN>' \
-H 'Content-Type: application/json; charset=UTF-8' \
-H 'Accept: application/json' \
--data-binary \
'{"label": "<CHECK_TITLE>", "type": "agent.plugin", "details": {"file": "<FILENAME>"}}' \
'https://monitoring.api.rackspacecloud.com/v1.0/<ACCOUNT_ID>/entities/<ENTITY_ID>/checks'
```

The portions that need values corresponding to your environment should be obvious, and the elements that are specific to your plugin are the check_title and the filename.  In this example the script is named mysql_replication.py and is located in the /usr/lib/rackspace-monitoring-agent/plugins directory of the Cloud Server.  If the directory doesn't exist, create it.  You can also confirm that the plugin is running by checking the log, by default it should be located at /var/log/rackspace-monitoring-agent.log and you should expect to see an entry similar to the following:

```
(plugin=mysql_replication.py) -> check completed (id=chdVhpS6Ya, type=agent.plugin, state=available)
(plugin=mysql_replication.py) -> agent.plugin (id=chdVhpS6Ya, period=150s) scheduled for 150s
```

Once you run the command above and created the check you will also need to create an alarm based on your criteria.  Within the new [Cloud Control Panel](https://mycloud.rackspace.com) you can add your Alarm Criteria and choose your notification options, or you can use [raxmon](https://github.com/racker/rackspace-monitoring-cli) or curl.

Below is example Alarm Criteria which can be modified based on your requirements:

```
if (metric['SLAVE_STATUS'] != 'ONLINE') {
    return new AlarmStatus(CRITICAL, 'MySQL Replication is OFFLINE.');
}

if (metric['SLAVE_STATUS'] == 'ONLINE' && metric['SECONDS_BEHIND_MASTER'] >= 300) {
    return new AlarmStatus(CRITICAL, 'MySQL Replication ONLINE - slave behind master.');
}
```

The above criteria will change the AlarmStatus if replication is offline or if the slave lags too far behind the master.  If something goes wrong you can expect a friendly email from our Cloud Monitoring system and hopefully you can avoid that awkward moment where you explain that your backup is 6-months old because you didn't bother to check your slave status. 

For additional information about Rackspace Cloud Monitoring check out the [API Developer Guide](http://docs.rackspace.com/cm/api/v1.0/cm-devguide/content/overview.html).

The above plugin, along with additional contributed plugins can be found on [github](https://github.com/racker/rackspace-monitoring-agent-plugins-contrib) and Fanatical Support can be found [@Rackspace](https://twitter.com/Rackspace).
