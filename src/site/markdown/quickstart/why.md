# Why Should I Use Imixs-Workflow?

So the first question so far is - why should you use a workflow engine? 
Surely you can also implement your business states within a database. But the reason why you should use a workflow engine comes from a different direction. The workflow engine manages your business data behind the scene and generates a lot of useful business information. 

### Status Information

The process instance returned by the method call '_processWorkItem()_' contains several useful new item values. For example you can get the new status based on your workflow model:

	String status=workitem.getItemValueString(WorkflowKernel.WORKFLOWSTATUS);

or you can ask for the process name: 

	String group=workitem.getItemValueString(WorkflowKernel.WORKFLOWGROUP);
	
Both information are part of your model and need no longer be managed by your code. 
	
### Processing Information
	
Beside this general status information Imixs-Workflow tells you who has created the process instance and who was the last editor: 

	String creator=workitem.getItemValueString(WorkflowKernel.CREATOR);
	String editor=workitem.getItemValueString(WorkflowKernel.EDITOR);

and of course you can also check the time points: 

	Date created==workitem.getItemValueDate(WorkflowKernel.CREATED);
	Date modified==workitem.getItemValueDate(WorkflowKernel.MODIFIED);

A complete list of all processing information generated by the workflow engine can be found [here](./workitem.html). 

### Responsibilities and Access 
	
Now lets come to the more interesting part - who is responsible within the process? You can ask this by checking the owner list of the process instance:

	List<String> owners=workitem.getItemValue("$owner");

The Imixs-Workflow engine in addition adds a custom [Access Control List (ACL)](https://www.imixs.org/doc/engine/acl.html) to each process instance. This ensures that only authorized persons can change the process instance. 	


	List<String> authors=workitem.getItemValue(WorkflowKernel.WRITEACCESS);
	List<String> readers=workitem.getItemValue(WorkflowKernel.READACCESS);	

The owners and ACL settings are part of the modeling process and can be set individually for each task or event:

<img src="../images/bpmn-example02.png" width="500px" />

A shortcut to ask if the current workitem is editable by the current user is:

	boolean editable=workitem.getItemValueBoolean(WorkflowKernel.ISAUTHOR);



## Model Driven Business Logic

All information you got so far is based on your workflow model. But you can also ask for information from your model. 
For a process instance already started, you can ask the workflow engine which events are currently available:

	List<ItemCollection> events=workflowService.getEvents(workitem);

The result depends on the current Task the process instance is assigned to.
The list of events holds information about the model (e.g. the name or the description of an event). 
If you have an event, you can process the workitem and transfer the order into a new state:

	// process the workitem with a new event
	workitem = workflowService.processWorkItem(workitem, event);


Models can also contain complex business rules to describe how a process should behave under certain circumstances.

<img src="../images/modelling/example_06.png"/>

You can find more information about in the section ['How to Model'](./modelling/howto.html). 

## The Processing History 

During the processing the Imixs-Workflow engine not only add business logic. The Engine also logs all processing steps so that a contiguous log is created. This log can be used to display a processing history:

<img src="../images/modelling/order-02.png" width="500px" />

This Processing History is generated by the [HistoryPlugin(./engine/plugins/historyplugin.html). Plugins are an excellent way to adapt the behavior of the workflow engine to your own needs. Learn more about this interface in section [Plugin API](./engine/plugins/index.html). 

## What's Next...

Continue reading more about:

 * [What Means Human Centric Workflow?](../quickstart/human.html)
 * [Imixs-BPMN - The Modeler User Guide](../modelling/index.html)
 * [The Imixs-Workflow Plugin API](../engine/plugins/index.html)
 * [The Imixs-Workflow Rest API](../restapi/index.html)
