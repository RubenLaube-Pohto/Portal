# Portal service

<!-- MDTOC maxdepth:6 firsth1:2 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [Setup](#setup)   
- [How to install our custom setup to Angular2 app](#how-to-install-our-custom-setup-to-angular2-app)   
- [angular/material2](#angularmaterial2)   
- [How to modify ng2-table to allow html to table cells](#how-to-modify-ng2-table-to-allow-html-to-table-cells)   
- [Zeppelin Graphs](#zeppelin-graphs)   

<!-- /MDTOC -->

## Setup

Install Node.js v6:

```shell
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Clone this repo.

Run `npm install` inside the project folder.

Start with `npm start`




## How to install our custom setup to Angular2 app

Our Angular2 app uses routes to navigate to different pages so you need to know basics of routing in Angular2 to use this manual

**If you clone this repository you have everything needed**,  
but if you are adding ng2-tables to your own app here are all setups to get it running.

Checkout packages.json from this wiki for our setup and dependencies
To get this working you need to have installed

* Angular2-material ```npm install --save @angular2-material/core @angular2-material/button @angular2-material/card```
* ng2-bootstrap ```npm install --save ng2-bootstrap```  
* ng2-table ```npm i ng2-table --save```  
*  ```npm install --save ng2-material angular-ui-grid angular ```

* moment ```npm install moment --save``` one that is supplied with angular 2 is not working as it should be)  



You need to add some mapping and packages to systemjs.config.js so Angular2 will find required files

Add ng2-table, ng2-bootstrap and moment to map and packages


```
//systemjs.config.js
var map = {
    'app':                        'app', // 'dist',

    '@angular':                   'node_modules/@angular',
    'angular2-in-memory-web-api': 'node_modules/angular2-in-memory-web-api',
    'rxjs':                       'node_modules/rxjs',
    'ng2-table':                  'node_modules/ng2-table',       //add this
    'ng2-bootstrap':              'node_modules/ng2-bootstrap',   //add this
    'moment':                     'node_modules/moment',          //and this
  };

var packages = {
  'app':                        { main: 'main.js',  defaultExtension: 'js' },
  'rxjs':                       { defaultExtension: 'js' },
  'ng2-table': { main: 'ng2-table.js', defaultExtension: 'js' },          //add this
  'ng2-bootstrap': { main: 'ng2-bootstrap.js', defaultExtension: 'js' },  //add this
  'moment': { main: 'moment.js', defaultExtension: 'js' },                //and this
};
```


first go to your grid app component (in our case ./app/app.kamulist.component.ts)
Add following imports
```
//app.kamulist.component.ts
import {CORE_DIRECTIVES, FORM_DIRECTIVES, NgClass, NgIf} from '@angular/common';
import {PAGINATION_DIRECTIVES} from 'ng2-bootstrap';
import {NG_TABLE_DIRECTIVES}    from 'ng2-table';
import {TableData} from './data/table-data'; //this is data storage for demo use
```

also add these to @Component directives

  ```
  //app.kamulist.component.ts
  directives: [
  NgClass,
  NG_TABLE_DIRECTIVES,
  PAGINATION_DIRECTIVES,
  NgIf,
  CORE_DIRECTIVES,
  FORM_DIRECTIVES
  ]
  ```

We have laid our template to seperate html file, which can be found in ./app/templates/kamulist.html

## angular/material2

Angular/material2 provided a set of elements that follow Google's matrial design principles. In this project version 2.0.0-alpha.6 elements were used.

Adding the needed elements to the project went as follows:

- installed the elements via `npm install --save @angular2-material/core @angular2-material/button @angular2-material/card`
- modified systemjs.config.js:

```javascript
// map to installed packages
const map: any = {
  ...
  '@angular2-material': 'node_modules/@angular2-material'
};

...

// Define material packages to add
var materialPkgs = [
  'button',
  'card',
  'core'
];

// Add package entries for material packages
materialPkgs.forEach((pkg) => {
  packages[`@angular2-material/${pkg}`] = {main: `${pkg}.js`};
});
```

Sources: [Getting started](https://github.com/angular/material2/blob/2.0.0-alpha.6/GETTING_STARTED.md)


## How to modify ng2-table to allow html to table cells

Navigate to node_modules/ng2-table/components/table/ng-table.component.js

And edit row 84  

from this:

```html
<td *ngFor=\"let column of columns\">{{getData(row, column.name)}}</td>\n
```  

to this:

```html
<td *ngFor=\"let column of columns\" [innerHtml]=\"row[column.name]\"></td>\n
```

Now table can shows html correctly.


## Zeppelin Graphs

It was planned that the measurement data from a KaMU could be displayed in the KaMU's detail view. Unfortunately Zeppelin didn't have support for our use case. It was possible to create different graphs for different KaMUs but then changing the default graph style from table to something more graphlike was impossible in Zeppelin v0.6.0 through the REST API.
