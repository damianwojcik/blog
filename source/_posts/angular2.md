---
title: Angular2 Intro Part 1
date: 2018-07-07 16:18:39
tags: #angular #javascript #components
---
After long delay I decided to migrate from AngularJS into Angular2+ (actually 6).
Summary of my notes after completing few tutorials about Introduction/Quick Start.

## WHAT IS ANGULAR2 (6)
Angular2 is a Framework for creating Single Page Applications (SPA).
SPA's are mostly client-side applications, so there is much less request to the server.
Angular2 is co-developed by 2 gigants: **Google** and **Microsoft**.

### WHAT'S NEW FROM ANGULARJS?
- No more Controllers and Scope.
- Components based - reusable, modular code.
- Better mobile support.
- Angular CLI]
- Uses **TypeScript**.
- Uses **RxJS** and Observables.

### WHY TYPESCRIPT?
- Strong typings.
- Next. gen JS features: **classes, imports/exports**.
- Missing JS features: **interfaces, generics**.

### QUICK START
- Install node and npm
- <code>npm i -g @angular/cli</code>
- <code>ng new first-app</code>
- <code>cd first-app</code>
- <code>ng serve -o</code>

### ARCHITECTURE
#### 1. MODULES
- First building block.
- Angular app is collection of many invidual modules.
- Every module represents a feature area e.g. users module.
- Root module: **AppModule**
- May contain one or more Components and/or Services. 

#### 2. COMPONENTS
- Building blocks of app view.
- Responsible for rendering view and it's logic.
- Consists of template: **.html**, styles: **.css**, class: **.ts**, testing file: **.spec.ts**.

#### 3. SERVICES
- Business logic.
- Provides **Data** for Components.

### 1. COMPONENTS
- **GOOD PRACTISE:** When starting building Angular2 application draw Components structure on a piece of paper.
- Components have **@Component** decorator which contains metadata.
- **Decorator** is a feature in TS. Function that provides information (metadata) about class attached to it (the one below it).
- Split up large components into smaller sub-components. Each focused on specific task or workflow.

#### CREATING COMPONENT USING CLI:
<code>
    ng generator component dashboard
    ng g c dashboard
</code>

Possible **flags**:
* --flat - creates Component in current directory
* --inline-template (or **-it**)
* --inline-styles (or **-is**)

Metadata's component **selector** may be: HTML tag(default), css class ('.dashboard') or attribute('[dashboard]')

**GOOD PRACTISE:** if your template is NOT longer than **3 lines** use inline template.

#### PROPERTY BINDING

##### ATTRIBUTE VS PROPERTY
- Are **not** the same.
- Attributes => **HTML**.
- Properties => **DOM**.
- **Attribue** initialize DOM properties and then they are done. Thier values **cannot change** once they are initialized.
- **Property** values **can change**.

**REMEMBER:** Angular Property Binding is to **property** not to attribute.

Example:
``` html
    <input [id]="myId" type="text"><!-- Property Binding -->
    <input bind-id="myId" type="text"><!-- Property Binding Other Syntax -->
    <input id="{{{myId}}" type="text"><!-- Interpolation -->
```
Why property binding over interpolation?
- Interpolation can work only with string values.

#### CLASS BINDING
``` html
    <h2 [class.text-danger]="some-condition">Hello</h2><!-- For Single Class Binding -->
    <h2 [ngClass]="titleClasses">Hello</h2><!-- For Multiple Classes Binding -->
```
Don't use regular html class declaration together with Angular Class Binding (beacuse it will be skipped).

#### STYLE BINDING
``` html
    <h2 [style.color]="orange">Hello</h2>
    <h2 [style.color]="hasError ? 'red' : 'green'">Hello</h2>
    <h2 [ngStyle]="titleStyles">Hello</h2><!-- For Multiple Styles Binding -->
```

#### EVENT BINDING
``` html
    <button (click)="onclick()">Greet</button>
    <button (click)="onclick($event)">Greet</button><!-- Angular specific $event object -->
```

#### TEMPLATE REFERENCE VARIABLES
``` html
    <input #myinput type="text">
    <button (click)="logMessage(myInput.value")>Log</button>
```

#### TWO WAY DATA BINDING
- Allows us to update a value of a property and at the same time display the value of that property.
- View **<=>** Class (**sync**)
- Uses **ngModel** directive.
- View => Class - Event Binding. <code>()</code> === **OUT**
- Class => View - Data Binding. <code>[]</code> === **IN**
- Banana in a box :) <code>[()]</code> === **OUT <=> IN**

NgModel is not default in Angular2. We have to **import it**.
In <code>app.module.ts</code>:
``` javascript
    import { FormsModule } from '@angular/forms';

    @NgModule {
        imports: [
            FormsModule
        ]
    }
```
Then use in template: 
``` html
    <input [(ngModel)]="name" type="text">
    <span>{{name}}</span>
```

### STRUCTURAL DIRECTIVES
Allow us to add/remove HTML elements.

#### *ngIf - conditionaly render element
``` html
    <!-- Basic If Condition -->
    <h2 *ngIf="false">Hello</h2>
    <!-- If/Else Syntax -->
    <h2 *ngIf="displayName; else elseBlock">Hello</h2>
    <ng-template #elseBlock>
        <h2>World</h2>
    </ng-template>
    <!-- Other Syntax -->
    <div *ngIf="displayname; then thenBlock; else elseBlock">Content</div>
    <ng-template #thenBlock>True content</ng-template>
    <ng-template #elseBlock>False content</ng-template>
```

#### *ngSwitch - conditionaly render element
``` html
    <div [ngSwitch]="color">
        <div *ngSwitchCase="red">Red</div>
        <div *ngSwitchCase="blue">Blue</div>
        <div *ngSwitchDefault>Default</div>
    </div>
```

#### *ngFor - render list of elements
``` html
    <div *ngFor="let color of colors; index as i">
        <h2>{{i}} {{color}}</h2>
    </div>
```

### COMPONENT INTERACTION
Communication between parent (**AppComponent**) and a child (**TestComponent**).

1. Parent => Child (AppComponent => TestComponent).
- Inside <code>app.component.html</code>:
``` html
    <app-test [parentData]="name"></app-test>
```

- Inside <code>testcomponent.component.ts</code>:
``` javascript
    import { Input } from '@angular/core';
    template: `
        <h2>{{"Hello" + parentData}}</h2>
    `
    Class:
    @Input public parentData;
```

1. Child => Parent (TestComponent => AppComponent).
- Inside <code>testcomponent.component.ts</code>:
``` javascript
    import { Output, EventEmitter } from '@angular/core';

    Class:
    @Output() public childEvent = new EventEmitter();

    fireEvent() {
        this.childEvent.emit("Hello");
    }
```
- Inside <code>testcomponent.component.html</code>:
``` html
    <button (click)="fireEvent()">Send Event</button>
```

- Inside <code>appcomponent.component.ts</code>:
``` html
    <h1>{{message}}</h1>
    <app-test (childEvent)="message=$event"></app-test>
```

### PIPES
Transforms data before displaying it into view.
``` html
    <!-- String Pipes -->
    {{ name | lowercase }}
    {{ name | uppercase }}
    {{ name | titlecase }}
    {{ name | slice:3 }}
    {{ name | slice:3:5 }}
    {{ obj | json }}

    <!-- Number Pipes -->
    {{ 5.678 | number:'1.2-3' }} // 5.678
    // 1- minimum number of integer digits, 2/3 - min/max number of decimal digits
    {{ 5.678 | number:'3.4-5' }} // 005.6780
    {{ 5.678 | number:'3.1-2' }} // 005.68
    {{ 0.25 | percent }} // 25%
    {{ 0.25 | currency }} // $0.25
    {{ 0.25 | currency:'GBP' }} // £0.25
    {{ 0.25 | currency:'GBP':code }} // GBP0.25

    <!-- Date Pipes -->
    {{ dateObj | date:'short' }}
    {{ dateObj | date:'shortDate' }}
    {{ dateObj | date:'shortTime' }}
    {{ dateObj | date:'long' }}
```
More pipes in docs.