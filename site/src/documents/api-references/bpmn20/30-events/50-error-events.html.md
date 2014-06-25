---

title: 'Error Events'
category: 'Events'

keywords: 'error start end boundary event definition'

---


Error events are events which are triggered by a defined error.

__Important note:__ a BPMN error is meant for business errors - which is pretty different to technical exceptions. So this is different to Java exceptions - which are by default handled in their own way.

<div class="alert alert-warning">
   <strong>Heads up!</strong>
   You might want to check out the basics of
   <a href="ref:/guides/user-guide/#process-engine-transactions-in-processes-transaction-boundaries">Threading and Transactions</a> first.
</div>

<div data-bpmn-diagram="implement/event-error"></div>

An error event definition references an error element. The following is an example of an error end event, referencing an error declaration:

```xml
<definitions>
  <error id="myError" errorCode="ERROR-OCCURED" name="ERROR-OCCURED"/>
  <!-- ... -->
  <process>
    <!-- ... -->
    <endEvent id="myErrorEndEvent">
      <errorEventDefinition errorRef="myError" />
    </endEvent>
  </process>
</definitions>
```

You can trigger this error event either by a throwing error event within your process definition or from Delegation Code, see
[Throwing BPMN Errors from Delegation Code](ref:/guides/user-guide/#process-engine-delegation-code-throwing-bpmn-errors-from-delegation-code).

Another possibility to define an error is the setting the type (class name) of any Java Exception as error code. Example:

```xml
<definitions>
  <error id="myException" errorCode="com.company.MyBusinessException" name="myBusinessException"/>
  ...
  <process>
    ...
    <endEvent id="myErrorEndEvent">
      <errorEventDefinition errorRef="myException" />
    </endEvent>
  </process>
</definitions>
```

The exception type should only used for business exceptions and not for technical exceptions in the process.

An error event handler references the same error element to declare that it catches the error.


## camunda Extensions

<table class="table table-striped">
  <tr>
    <th>Attributes</th>
    <td>&ndash;</td>
  </tr>
  <tr>
    <th>Extension Elements</th>
    <td>
      <a href="ref:#custom-extensions-camunda-extension-elements-camundainputoutput">
        camunda:inputOutput</a>
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td>&ndash;</td>
  </tr>
</table>


## Error Start Event

An error start event can only be used to trigger an Event Sub-Process - it __cannot__ be used for starting a process instance. The error start event is always interrupting.

<div data-bpmn-diagram="implement/event-subprocess-alternative1"></div>


## Error End Event

When process execution arrives in an error end event, the current path of execution is ended and an error is thrown. This error can caught by a matching intermediate boundary error event. In case no matching boundary error event is found, the execution semantics default to the none end event semantics.


## Error Boundary Event

An intermediate catching error on the boundary of an activity, or boundary error event for short, catches errors that are thrown within the scope of the activity on which it is defined.

Defining a boundary error event makes most sense on an embedded subprocess, or a call activity, as a subprocess creates a scope for all activities inside the subprocess. Errors are thrown by error end events. Such an error will propagate its parent scopes upwards until a scope is found on which a boundary error event is defined that matches the error event definition.

When an error event is caught, the activity on which the boundary event is defined is destroyed, also destroying all current executions within (e.g. concurrent activities, nested subprocesses, etc.). Process execution continues following the outgoing sequence flow of the boundary event.

<div data-bpmn-diagram="implement/event-subprocess-alternative2"></div>

A boundary error event is defined as a typical boundary event. As with the other error events, the errorRef references an error defined outside the process element:

```xml
<definitions>
  <error id="myError" errorCode="ERROR-OCCURED" name="name of error"/>
  <!-- ... -->
  <process>
    <!-- ... -->
    <subProcess id="mySubProcess">
      <!-- ... -->
    </subProcess>
    <boundaryEvent id="catchError" attachedToRef="mySubProcess">
      <errorEventDefinition errorRef="myError"/>
    </boundaryEvent>
  </process>
</definitions>
```

The errorCode is used to match the errors that are caught:

*   If errorRef is omitted, the boundary error event will catch any error event, regardless of the errorCode of the error.
*   In case an errorRef is provided and it references an existing error, the boundary event will only catch errors with the same error code.


## Additional Resources

*   [Error Events in the BPMN Tutorial](http://camunda.org/design/reference.html#!/events/error)
*   [Error Handling](ref:/guides/user-guide/#process-engine-transactions-in-processes )
