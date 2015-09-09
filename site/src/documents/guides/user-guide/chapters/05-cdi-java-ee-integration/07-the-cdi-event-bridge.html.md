---

title: 'Cdi Event Bridge'
category: 'CDI and Java EE Integration'

---


The Process engine can be hooked-up to the CDI event-bus. We call this the "Cdi Event Bridge" This allows us to be notified of process events using standard CDI event mechanisms. In order to enable CDI event support for an embedded process engine, enable the corresponding parse listener in the configuration:

    <property name="postBpmnParseHandlers">
      <list>
        <bean class="org.camunda.bpm.engine.cdi.impl.event.CdiEventSupportBpmnParseListener" />
      </list>
    </property>

Now the engine is configured for publishing events using the CDI event bus.
<div class="alert alert-info">
  <p>
    <strong>Note:</strong>
    The above configuration can be used in combination with an embedded process engine. If you want to use this feature in combination with the shared process engine in a multi application environment, you need to add the CdiExecutionListener as Process Application event listener. <a href="ref:#cdi-and-java-ee-integration-cdi-event-bridge-the-cdi-event-bridge-in-a-process-application">See next section</a>.
  </p>
</div>

The following gives an overview of how process events can be received in CDI beans. In CDI, we can declaratively specify event observers using the @Observes-annotation. Event notification is type-safe. The type of process events is org.camunda.bpm.engine.cdi.BusinessProcessEvent. The following is an example of a simple event observer method:

```
public void onProcessEvent(@Observes BusinessProcessEvent businessProcessEvent) {
  // handle event
}
```

This observer would be notified of all events. If we want to restrict the set of events the observer receives, we can add qualifier annotations:

* `@BusinessProcessDefinition`: restricts the set of events to a certain process definition. Example:

  ```
  @Observes
  @BusinessProcessDefinition("billingProcess")
  private BusinessProcessEvent evt;
  ```

* `@StartActivity`: restricts the set of events by a certain activity. For example:

  ```
  @Observes
  @StartActivity("shipGoods")
  private BusinessProcessEvent evt;
  ```

  is invoked whenever an activity with the id "shipGoods" is entered.

* `@EndActivity`: restricts the set of events by a certain activity. The following for example is invoked whenever an activity with the id "shipGoods" is left:

  ```java
  @Observes
  @EndActivity("shipGoods")
  private BusinessProcessEvent evt;
  ```

* `@TakeTransition`: restricts the set of events by a certain transition.

The qualifiers named above can be combined freely. For example, in order to receive all events generated when leaving the "shipGoods" activity in the "shipmentProcess", we could write the following observer method:

```
public void beforeShippingGoods(@Observes @BusinessProcessDefinition("shippingProcess") @EndActivity("shipGoods") BusinessProcessEvent evt) {
  // handle event
}
```

In the default configuration, event listeners are invoked synchronously and in the context of the same transaction. CDI transactional observers (only available in combination with JavaEE / EJB), allow to control when the event is handed to the observer method. Using transactional observers, we can for example assure that an observer is only notified if the transaction in which the event is fired succeeds:

```
public void onShipmentSuceeded(
  @Observes(during=TransactionPhase.AFTER_SUCCESS) @BusinessProcessDefinition("shippingProcess") @EndActivity("shipGoods") BusinessProcessEvent evt) {

  // send email to customer
}
```

### The Cdi Event Bridge in a Process Application

In order to use the Cdi Event Bridge in combination with a multi-application deployment and the shared process engine, the [CdiExecutionListener](ref:/reference/javadoc/?org/camunda/bpm/engine/cdi/impl/event/CdiExecutionListener.html) needs to be added as a [Process Application Execution Event Listener](ref:#process-applications-process-application-event-listeners).

Example configuration for [Servlet Process Application](ref:#process-applications-the-process-application-class-the-servletprocessapplication):

    @ProcessApplication
    public class InvoiceProcessApplication extends ServletProcessApplication {

      protected ExecutionListener cdiExecutionListener = new CdiExecutionListener();

      public ExecutionListener getExecutionListener() {
        return cdiExecutionListener;
      }
    }

Example configuration for [Ejb Process Application](ref:#process-applications-the-process-application-class-the-ejbprocessapplication):

    @Singleton
    @Startup
    @ConcurrencyManagement(ConcurrencyManagementType.BEAN)
    @TransactionAttribute(TransactionAttributeType.REQUIRED)
    @ProcessApplication
    @Local(ProcessApplicationInterface.class)
    public class MyEjbProcessApplication extends EjbProcessApplication {

      protected ExecutionListener cdiExecutionListener = new CdiExecutionListener();

      @PostConstruct
      public void start() {
        deploy();
      }

      @PreDestroy
      public void stop() {
        undeploy();
      }

      public ExecutionListener getExecutionListener() {
        return cdiExecutionListener;
      }
    }
