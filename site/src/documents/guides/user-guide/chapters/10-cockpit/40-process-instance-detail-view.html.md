---

title: 'Process Instance Detail View'
category: 'Cockpit'

---

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-process-instances-view.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    <p>Open the Process Instance View by selecting a process instance from the <a href="ref:#cockpit-process-definitions-view">Process Definitions View</a> instance list. This view allows you to drill down into a single process instance and explore it's running activities as well as the variables, tasks, jobs, etc.</p>
    <p>Beside the diagram view the process will be displayed as an <a href="ref:#cockpit-process-instance-detail-view-activity-instance-tree">Activity Instance Tree View</a>. Variables that belong to the instance will be listed in a variables table of the [Detailed Information Panel](ref:#tools-cockpit-process-instance-view-detailed-information-panel). Now you can select single or multiple ('ctrl + click') flow nodes in the interactive BPMN 2.0 diagram or you can select an activity instance within the activity tree view. As diagram, tree view and variables table correspond with each other the selected flow node will also be selected in the tree as well as the associated variables will be shown and vice versa.</p>
  </div>
</div>

## Activity Instance Tree

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-activity-instance-tree-view.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    <p>The activity instance tree contains a node for each activity that is currently active in the process instance. It allows you to select activity instances to explore their details. Concurrently the selected instance will be marked in the rendered process diagram and the corresponding variables will be listed in the Detailed Information Panel.</p>
  </div>
</div>

## Detailed Information Panel

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-detailed-information-view.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    <p>Use the Detailed Information Panel to get an overview about variables, incidents, called process instances and user tasks that the process instance contains. Depending on the selected activity instance in the rendered diagram the panel lists the corresponding information. You can also focus on the activity instance via a scope link in the table.<br>
    In addition to the instance information you can <a href="ref:#cockpit-process-instance-detail-view-editing-variables">edit variables</a> or change the asignee of user tasks.</p>
  </div>
</div>

## Adding Variables

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-add-variables.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    <p>Hit the <button class="btn btn-xs dropdown-toggle"><i class="glyphicon glyphicon-plus"></i> </button> button on the right hand side to add variables to a process instance. You can chosse between different data types. Please note that variable will be overwritten if you add a new variable with an existing name.</p>
  </div>
</div>

## Editing Variables

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-edit-variables.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    <p>Hit the <button class="btn btn-xs dropdown-toggle"><i class="glyphicon glyphicon-pencil"></i> </button> symbol in the Detailed Information Panel to edit variables. This feauture allows you to change the value of variables as well as the type. A validation of the date format and for the value of integers happens on client side. If you enter NULL the variable will be converted to string.</p>
  </div>
</div>

## Cancel a Process Instance

<div class="row">
  <div class="col-xs-6 col-sm-6 col-md-3">
    <img data-img-thumb src="ref:asset:/assets/img/implementation-cockpit/cockpit-cancel-process-instance.png" />
  </div>
  <div class="col-xs-6 col-sm-6 col-md-9">
    When you select a single process instance you can cancel it in the Process Instance View. <p>Hit the <button class="btn btn-xs dropdown-toggle"><i class="glyphicon glyphicon-remove-circle"></i> </button> button on the right hand side. After you have confirmed this step the runtime data of the canceld instance will be deleted.
  </div>
</div>
