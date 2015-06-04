This example demonstrates how to use Storm and a persistent data store (HBase in this case,) to correlate pieces of data that arrive at different times.

This example is based on tracking session state. When a session begins, a START event is sent through the topology. This event, the time it was sent, and the Session ID are stored in HBase.

Later, an END event is received. The topology uses the Session ID to look up the previously received START event and calculate the duration (time elapsed between the two,) and then stores the END event and duration to HBase.

The SessionInfo project is a basic HBase client application that can be used to create, delete, or do some basic time range queries against the table in HBase.

##To Use

###Create a table in HBase

1. Open the **SessionInfo** project in Visual Studio.

2. Right click on the project and select **Build** (to restore packages and such,) then right click again and select **Properties**.

3. In Properties, set the URL to your HBase cluster (https://clustername.azurehdinsight.net/,) and the admin user name and password for the cluster. You can leave the table name and column family entries as they are.

4. Run the SessionInfo project. When prompted, select 'c' to create a table.

###Configure and deploy the topology

1. Open the **CorrelationTopology** project in Visual Studio.

2. Right click on the project and select **Build** (to restore packages.) Then right click again and select **Properties**.

3. In Properties, set the URL to your HBase cluster (https://clustername.azurehdinsight.net/,) and the admin user name and password for the cluster. You can leave the table name and column family entries as they are.

3. Right click the project and select **Submit to Storm on HDInsight**. You may have to authenticate to your Azure subscription here. When the dialog appears, and lists your clusters, select the Storm cluster to deploy to.

	After deployment, the Topology viewer will appear. At this point the topology should be running. Give it a few seconds to start generating events.
	
###Query data

1. Go back to the running SessionInfo project. From here, select 's' to see START events that have been logged.

2. When prompted, enter a start time of around the time the topology started on the Storm cluster. Use a time format of HH:MMam or pm. 
	
3. When prompted, enter an end time of the current time, or some time in the future to make sure you have a time range of a couple minutes.
	
	The utility will search HBase for START events in the given time span and return a list of events.
	
Searching for END events works the same how, however END events happen between 1 and 5 minutes after the START event. Once END events are fired, more START events will occur. The topology will keep generating these events as long as you leave it running

##Kill the topology

After you have finished running this, return to the topology viewer in Visual Studio, select the topology, and then use the **Kill** button to stop the topology.