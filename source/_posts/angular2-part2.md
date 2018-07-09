---
title: Angular2 Intro Part 2
date: 2018-07-09 13:14:35
tags: #angular #javascript #services
---
After long delay I decided to migrate from AngularJS into Angular2+ (actually 6).
Summary of my notes after completing few tutorials about Introduction/Quick Start.

## SERVICES
Are classes with specific purpose.
- Share data between Components.
- Consist application logic.
- Server external interaction (e.g. with database).

Why Services?
Because components should not fetch or save any data. They should be immutable - work with any data.

### DEPENDENCY INJECTION
- **DI** as a Design Pattern or a Framework.
- **DI** === flexible controlled, code. E.g.to avoid issues such as when we add parameter to a child class, then the parent class is broken.
- **DI** is a coding pattern in which a class receives its dependencies from external sources rather than creating them itself.
- **@Injectable** decorator in Service allow us to use service-in-service.

Example:
- **Without DI**:
``` javascript
    class car {
        engine;
        tires;
        constructor() {
            this.engine = new Engine();
            this.tires = new Tires();
        }
    }
```
- **With DI**:
``` javascript
    class car {
        engine;
        tires;
        constructor(engine, tires) {
            this.engine = engine;
            this. tires = tires;
        }
    }
```

### IMPLEMENTING SERVICE
Steps:
1. **Define EmployeeService Class.**
Using CLI:
    <code>ng g s employee</code>
2. **Register with injector.**
Angular DI is hierarchical so register service in the highest parent (AppModule) so all children will have access to the data. CLI v. 6+ does it automatically for us.
3. **Declare as dependency in EmpList and EmpDetail Components.**
In both of those files add:
``` javascript
    import { EmployeeService } from '../employee.service'

    public employees = [];
    constructor(private employeeService: EmployeeService) {}

    ngOnInit() {
        this.employees = this.employeeService.getEmployees();
    }
```

## HTTP AND OBSERVABLES
Observables are served in Angular6 by **RxJS 6** library.

**Observables** - sequence of items that arrive asynchronously over time (**stream of data**). To the contrary to standard HTTP response which is single item.
**Observer** - watches Observable and executes an action if it changes.
**Subscription** - tells *Observer* to observe *Observable* (listen).

More about Observables and RxJS in my other post.

To add **HTTP** to your app you must:
- In <code>app.module.ts</code>:
``` javascript
    import { HttpClientModule } from '@angular/common/http';
    imports: [
        HttpClientModule
    ]
```
- In <code>emp.service.ts</code>:
``` javascript
    import { HttpClient } from '@angular/common/http';
    constructor(private http: HttpClient) {}
```

Steps to serve HTTP requests:
1. **HTTP GET request.**
In <code>emp.service.ts</code>:
``` javascript
    uri = 'https://api.com/get'

    getEmpoyees() {
        return this.http.get(this.uri)
    }
```
2. **Receive the Observable in Component and subscribe to it.**
In <code>emplist.component.ts</code>:
``` javascript
    ngOnInit() {
        this.employeeService.getEmployees()
            .subscribe(employees => {
                console.log(employees);
            });
    }
```
3. **Optionally create Interface for data and add Error Handling.**

## ROUTING
1. **Add routing module.**
- To new project:
    <code>ng new routing-app --routing</code>
- To existing project:
    <code>ng generate module app-routing --flat --module-app</code>
2. **Add routes.**
In <code>app-routing.module.ts</code>:

``` javascript
    const routes: Routes = [
        { path: '', redirectTo: '/employees', pathMatch: 'full' }, // default route
        { path: 'employees', component: EmpListComponent },
        { path: 'departments', component: DepartmentsComponent },
        { path: 'departments/:id', component: DepartmentDetailComponent }, // route with param
        { path: 'employees/:id',
          component: EmployeeDetailComponent,
          children: [
              { path: 'overview', component: DepartmentOverviewComponent }, // child routes
              { path: 'contact', component: DepartmentContactComponent }
          ]
         },
        { path: '**', component: PageNotFoundComponent } // 404 page
    ]
```
Always configure routes from the most specific to the least specific below.
3. **Add router-outlet directive to** <code>app.component.html</code>.

4. **Add router links to view (navigation).**
``` html
    <a routerLink="/departments" routerLinkActive="activeClass">Departments</a>
    <a routerLink="/employees">Employees</a>
```

## VERSION 6 FEATURES
- **RxJS v.6**
- <code>ng update package-name</code>
- <code>javascript ng add @angular/material</code>
- CLI + Material Templates:
    * Navigation
        <code>ng generate @angular/material:material-nav --name=my-nav</code>
    * Dashboard
        <code>ng generate @angular/material:material-dashboard --name=my-dashboard</code>
    * Table
        <code>ng generate @angular/material:material-table --name=my-table</code>
- Angular Elements - allow us to use Angular component in other enviroments e.g. **Vue**.
- Ivy
- [Angular Update to 6](https://update.angular.io)