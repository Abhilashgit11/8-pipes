# 8Pipes

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 15.2.4.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The application will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

-------------Angular tutorial from Udemy with Maximilian-----------------
-------------Angular 8 The Complete Guide-----------------

# Pipes

1. Created a new project with ng new 8-pipes and copy pasted src folder from exercise files.
2. Added 2 pipes (uppercase, date) in app.component.html
    {{ server.instanceType | uppercase }} | 
    {{ server.started | date:'fullDate' | uppercase}}
    Here date pipe takes 1 parameter
3. Learn more about pipes: https://angular.io/api?query=pipe
4. Make sure the order of the pipes is correct.
    1. Correct way
        {{ server.started | date:'fullDate' | uppercase}}
    2. Wrong way: Application fails
        {{ server.started | uppercase | date:'fullDate' }}
5. Creating custom pipes
    1. Generated custom pipe with "ng g p shorten"
    2. Modified the transform method the following way"
        transform(value: any) {
            if(value.length > 10) {
            return value.substr(0,10) + ' ...';
            }
            return value;
        }
    3. This custom pipe will shorten the string if its length is greater than 10 i.e. it will show only first 10 characters of the string and add 3 dots.
6. Adding parameters to custom types
    1. Adding 15 here with a colon (shorten:15) 
        <strong>{{ server.name | shorten:15}}</strong>
    2. Need to add a parameter in transform method as well
        transform(value: any, limit: number) {
            if(value.length > limit) {
            return value.substr(0, limit) + ' ...';
            }
            return value;
        }
7. Created a custom pipe called filter which is tied to an input and performs a search operation on status.
    1. Take a look at filter.pipe.ts
    2. And the corresponding app.component.html
        <input type="text" [(ngModel)]="filteredStatus">
        *ngFor="let server of servers | filter: filteredStatus: 'status'" 
    3. 
8. This is important to understand as it might cause performance issues. We are talking about filter pipe.
    1. filter pipe is attached to an input and it performs search operation.
    2. Lets say you typed in "offline" in the input field and the servers with status "offline" are shown as the result of the search operation. In our example, only one server is offline, so only one server is shown.
    3. Now while you have still typed in "offline" and you try to add a new server, the new server gets added but it is not automatically shown while you have typed in "offline" in the input.
    4. What we can do is update the input by
        1. Adding space and removing space or
        2. Adding a character and removing a character
     This way it will update the search and the search result will show the added server as well.
    5. However, if we want to update the search result as we add more servers while we are in the search state, we can achieve that by adding "pure: false" to the pipe decorator
        @Pipe({
            name: 'filter',
            pure: false
        })
        Note: Adding "pure: false" will cause performance issues.
9. Pipe Assignment
    1. Reverse a string
        1. Created a new pipe with ng g p reverse
        2. Update the transform method to reverse a string
            transform(value: any): any {
                return value.split('').reverse().join('');
            }
        3. Added the reverse pipe in app.component.html
            {{ server.instanceType | uppercase | reverse}} |
    2. Sort the servers
        1. Created a new pipe with ng g p sort.
        2. Updated the transform method to sort based on the names of the servers.
            transform(value: any, propName: string): any {
                return value.sort((a: any, b: any) => {
                if(a[propName] > b[propName]) {
                    return 1;
                } else {
                    return -1;
                }
                });
            }
        3. Added the sort in app.component.html to 
            *ngFor="let server of servers | filter: filteredStatus: 'status' | sort:'name'" 
        4. Now when you add servers they are added at the bottom. To add them according to the sorting, you can add "pure: false" to the decorator
            @Pipe({
                name: 'sort',
                pure: false
            })
            



