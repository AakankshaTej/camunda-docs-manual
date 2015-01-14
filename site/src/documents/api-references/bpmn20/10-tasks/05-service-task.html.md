---

title: 'Service Task'
category: 'Tasks'

keywords: 'service task implementation field injections results'

---


A service task is used to invoke services. In camunda this is done by calling Java code.

<div data-bpmn-symbol="servicetask" data-bpmn-symbol-name="Service Task"></div>

There are 4 ways of declaring how to invoke Java logic:

* Specifying a class that implements JavaDelegate or ActivityBehavior
* Evaluating an expression that resolves to a delegation object
* Invoking a method expression
* Evaluating a value expression

To specify a class that is called during process execution, the fully qualified classname needs to be provided by the `camunda:class` attribute.

```xml
<serviceTask id="javaService" 
             name="My Java Service Task" 
             camunda:class="org.camunda.bpm.MyJavaDelegate" />
```

Please refer to [Java Delegate](ref:/guides/user-guide/#process-engine-delegation-code-java-delegate) for details on how to implement a Java Delegate.

It is also possible to use an expression that resolves to an object. This object must follow the same rules as objects that are created when the `camunda:class` attribute is used.

```xml
<serviceTask id="serviceTask" camunda:delegateExpression="${delegateExpressionBean}" />
```
Here, the	`delegateExpression` is an expression that resolves to a bean implementing the `JavaDelegate` interface. Beans can be resolved for example via CDI or Spring (see below).

To specify an expression that should be evaluated, use attribute <class>camunda:expression</class>.

```xml
<serviceTask id="javaService" 
             name="My Java Service Task" 
             camunda:expression="#{printer.printMessage()}" />
```

Method `printMessage` (without parameters) will be called on the named bean called `printer`.

It's also possible to pass parameters with an method used in the expression.

```xml
<serviceTask id="javaService" 
             name="My Java Service Task" 
             camunda:expression="#{printer.printMessage(execution, myVar)}" />
```

Method `printMessage` will be called on the object named `printer`. The first parameter passed is the	`DelegateExecution`, which is available in the expression context by default available as `execution`. The second parameter passed, is the value of the variable with name `myVar` in the current execution.

To specify a UEL value expression that should be evaluated, use attribute `camunda:expression`.

```xml
<serviceTask id="javaService" 
             name="My Java Service Task" 
             camunda:expression="#{split.ready}"
             camunda:resultVariable="myVar" />
```
The getter method of property `ready`,	`getReady()`	(without parameters), will be called on the named bean called `split`. The named objects are resolved in the execution's process variables	and (if applicable) in the CDI or Spring context.

Note that you can store the return value of the method in a process variable by specifying the `camunda:resultVariable`.

## Using Java Delegate Class

Please refer to [Java Delegate](ref:/guides/user-guide/#process-engine-delegation-code-java-delegate) for details.

## Generic Java Delegates & Field Injection

You can easily write generic Java Delegate classes with can later on be configured via the BPMN 2.0 XML in the Service Task. Please refer to [Field Injection](ref:/guides/user-guide/#process-engine-delegation-code-field-injection) for details.

## Service task results

The return value of a service execution (for service task using expression only) can be assigned to an already existing or to a new process variable by specifying the process variable name as a literal value for the `camunda:resultVariable` attribute of a service task definition. Any existing value for a specific process variable will be overwritten by the result value of the service execution. When not specifying a result variable name, the service execution result value gets ignored.

```xml
<serviceTask id="aMethodExpressionServiceTask"
           camunda:expression="#{myService.doSomething()}"
           camunda:resultVariable="myVar" />
```

In the example above, the result of the service execution (the return value of the `doSomething()` method invocation on object `myService`) is set to the process variable named `myVar` after the service execution completes.

<div class="alert alert-warning">
  <strong>Result variables and multi-instance</strong>
  <p>
    Note that when you use <code>camunda:resultVariable</code> in a multi-instance construct, for example in a multi-instance subprocess, the result variable is overwritten every time the task completes, which may appear as random behavior.
  </p>
  <p>
    This is a known issue. As a workaround, a local variable can be declared by adding an <a href="ref:/guides/user-guide/#process-engine-delegation-code-execution-listener">execution listener</a> to the subprocess' start event that initializes the variable as null.
  </p>
</div>

## Additional Resources

* [Tasks in the BPMN Tutorial](http://camunda.org/design/reference.html#!/activities/tasks)
* [Error Handling](https://app.camunda.com/confluence/display/foxUserGuide/Error+Handling)
* [How to call Web Services from BPMN](http://www.bpm-guide.de/2010/12/09/how-to-call-a-webservice-from-bpmn/)
