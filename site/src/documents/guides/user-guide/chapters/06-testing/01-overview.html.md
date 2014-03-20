---

title: 'Overview'
category: 'Testing'

---

When testing Process Applications you first have to be clear on what scope you want to test. Often Process Applications orchestrate various existing services meaning a Process Application tests quickly becomes an integration test. The following picture show the scopes we differentiate when testing Process Applications:

<center><img class="img-responsive" src="ref:asset:/guides/user-guide/assets/img/testing-scopes.png" /></center>

* Testing process definitions only, as isolated as possible.
* Testing your process application including e.g. CDI or EJB beans.
* Integration testing of your applications with other deployments or services (maybe deployed as mock services) on your application server.
* End-to-end integration test including all external systems.
