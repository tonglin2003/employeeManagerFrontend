## Setup of Angular
1. Enter the following to install Angular:
```
npm install -g @angular/cli
```

2. Enter the following to create a new project, replace `my-app` to your project name:
```
ng new my-app
```

3. cd into your project and enter the following to run the angular app:
```
ng serve
```

4. In the Terminal pane of your IDE:
```
npm install
ng serve
```

### Note
**Open the `/src` directory, and you will see these files**
1. `index.html` the app top level HTML template
2. `style.css`
3. `main.ts` the app starts running

<br><br>

**Open the `/app` component file:**
1. `app.component.ts` the srouce file that describes the `app-root` component. The top level Angular components in the app.
    * The component description includes the component's code, HTML template, and styles, which can be described in this file, or in separate files.
2. `app.component.css`
3. **NEW COMPONENT** are added to this directory


<br><br>

**Open the `/assets` component file:**
1. The folder where all the images goes

<br><br>


## Component Features
* `selector`: describe how Angular refers to the component in template
* `standalone`: describe whether the component requires a `NgModule`
* `imports`: describe the components' dependencies
* `templates`: to describe the components's HTML markup and layout, here is where all the design and HTML goes, you can also create a new HTML file and add it in the component as `templateUrl`
* `styleUrl`: list the URLs of the CSS files that the componenet use in an array

Enter the following to generate a new component, you can also do it manually:
```
ng generate component home --inline-template --skip-tests
```
1. in `app.component.ts` add:
```
import { HomeComponent } from './home/home.component';
```

Then update the `app.component.ts`, in `@Component`, update the `imports` array property and add `HomeComponent`.
```
imports: [
  HomeComponent,
],
```

<br><br>

## Using another component in the current component
1. Create a new component using:
```
ng generate component housingLocation --inline-template --skip-tests
```
2. Import into the current component, usign `home.component.ts` as example:
```
import { HousingLocationComponent } from '../housing-location/housing-location.component';
```
3. Add into the import of the `home.component.ts`, `@Component`, `imports`
```
imports: [
  CommonModule,
  HousingLocationComponent
],
```
4. Use in the `template`:
```
template: `
  <section>
    <form>
      <input type="text" placeholder="Filter by city">
      <button class="primary" type="button">Search</button>
    </form>
  </section>
  <section class="results">
    <app-housing-location></app-housing-location>
  </section>
  `,
```
**NOTE:** The format of the component in the template must be the `selector` of the component



<br><br>

### Creating an interface
* a new interface that it can use as a data type
* and it can create an instance of the new interface with some sample data

1. In the terminal enter:
```
ng generate interface housinglocation
```

2. Enter the interface definition and example:
```
export interface HousingLocation {
  id: number;
  name: string;
  city: string;
  state: string;
  photo: string;
  availableUnits: number;
  wifi: boolean;
  laundry: boolean;
}
```

3. in `home.component.ts` or any file you want to import the interface, then import the componenet. Inside the `export class HomeComponent` create the instance of the interface:
```
export class HomeComponent {
  readonly baseUrl = 'https://angular.io/assets/images/tutorials/faa';

  housingLocation: HousingLocation = {
    id: 9999,
    name: 'Test Home',
    city: 'Test city',
    state: 'ST',
    photo: `${this.baseUrl}/example-house.jpg`,
    availableUnits: 99,
    wifi: true,
    laundry: false,
  };
}
```


<br><br>

### Input Parameter in Component
You will need to use `@Input`

And example of the input is:
```
export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
```

**NOTE:** The `!` and also called *non-null* assertion operation. It is needed because the input is expecting the value to be passed. In this case, there is no default value. 
    * Therefore, the `!` is saying that the input won't be null


<br><br>

### Add property bingding to a component's template
The **property binding** allow to connect a variable to an `Input` in Angular template.
Example:
```
<app-housing-location [housingLocation]="housingLocation"></app-housing-location>
```
**Note:** we uses `[attribute] = "value"` as the format

<br><br>

### Add an interpolation to a component's template
* we will display values (properties and input value) in a template using interpolation.
* So it is more like the smaller building block of the website, example you can use a for loop and create multiple display of info in the website.

In the `HousingLocationComponent` template, that will display different houses in a small card, it would look like this:
```
template: `
  <section class="listing">
    <img class="listing-photo" [src]="housingLocation.photo" alt="Exterior photo of {{housingLocation.name}}">
    <h2 class="listing-heading">{{ housingLocation.name }}</h2>
    <p class="listing-location">{{ housingLocation.city}}, {{housingLocation.state }}</p>
  </section>
  `,
```
**Format:** `{{ expression }}`

Explanation: 
`HousingLocationComponent` takes in `housingLocation` of the type **HouseLocation** which the interface we created. 

So Angular works like: HomeComponent fetch for house information -> HousingLocationComponent work as small blocks in HomeComponent, and display all the hourse information, and HouseLocation will be the inpit of HousingLocationComponent

in other words:
HomeComponent.contain(HousingLocationComponent(houseLocation : HouseLocation))


<br><br>

### Use *ngFor to list object in component
* Add a data set to the app
* display a list of element from the new data set using `ngFor`
* it can also be utilize to iterate over array and even asynchronus value.

1. In the `HomeComponent` you will need to prepare a list of usable HouseLocation instances, it could be fetched from an API or created manually with sample data, so example code:
```typescript
export class HomeComponent {
  readonly baseUrl = 'https://angular.io/assets/images/tutorials/faa';

  housingLocationList: HousingLocation[] = [
    {
      id: 0,
      name: 'Acme Fresh Start Housing',
      city: 'Chicago',
      state: 'IL',
      photo: `${this.baseUrl}/bernard-hermant-CLKGGwIBTaY-unsplash.jpg`,
      availableUnits: 4,
      wifi: true,
      laundry: true
    },
    {
      id: 1,
      name: 'A113 Transitional Housing',
      city: 'Santa Monica',
      state: 'CA',
      photo: `${this.baseUrl}/brandon-griggs-wR11KBaB86U-unsplash.jpg`,
      availableUnits: 0,
      wifi: false,
      laundry: true
    },
    {
      id: 2,
      name: 'Warm Beds Housing Support',
      city: 'Juneau',
      state: 'AK',
      photo: `${this.baseUrl}/i-do-nothing-but-love-lAyXdl1-Wmc-unsplash.jpg`,
      availableUnits: 1,
      wifi: false,
      laundry: false
    }
  ];
}
```

***In the Template!! Use it like***
```typescript
<app-housing-location
  *ngFor="let housingLocation of housingLocationList"
  [housingLocation]="housingLocation">
</app-housing-location>
```

Explanation: `housingLocation` on the left is the inputName, `housingLocation` on the right is the element in variable/list (housringLocationList) we have in the export HouseComponent class


<br><br>

### Angular Services
* A service to serve the data to the app.
1. Create service by using the following 
```
ng generate service housing --skip-tests
```
2. Add static data to the new service, in the service file, you can write code like:
```typescript
getAllHousingLocations(): HousingLocation[] {
  return this.housingLocationList;
}

getHousingLocationById(id: number): HousingLocation | undefined {
  return this.housingLocationList.find(housingLocation => housingLocation.id === id);
}
```

and also add in the line to ensure we have all the import we need:
```typescript
import { HousingLocation } from './housinglocation';
```
3. Inject the new service into HomeComponent, and the code would look like:
```typescript
housingLocationList: HousingLocation[] = [];
housingService: HousingService = inject(HousingService);

constructor() {
  this.housingLocationList = this.housingService.getAllHousingLocations();
}
```
**Note:** The service is where we add code for the fetching, we don't want to put in the HomeComponent because it would look messy.


<br><br>

### Add Routes to the Application
1. Create a default details componenet
```
ng generate component details --inline-template --skip-tests
```

2. Add routing to the application
    * in `src/app` create a new file called `routes.ts` 
    * in `main.ts` import `provideRouter` from `@angular/router`
    * update the `bootstrapApplication` to include the routing config:
```typescript
bootstrapApplication(AppComponent,
  {
    providers: [
      provideProtractorTestingSupport(),
      provideRouter(routeConfig)
    ]
  }
).catch(err => console.error(err));
```

3. In the `src/app/app.componenet.ts` update the component to use the routing:
```typescript
import { RouterModule } from '@angular/router';
```

4. Add `RouterModule` to `@Component` metadata import:
```
imports: [
  HomeComponent,
  RouterModule,
],
```

5. In the `template` replace app-home with **router-outlet**
```typescript
template: `
  <main>
    <a [routerLink]="['/']">
      <header class="brand-name">
        <img class="brand-logo" src="/assets/logo.svg" alt="logo" aria-hidden="true">
      </header>
    </a>
    <section class="content">
      <router-outlet></router-outlet>
    </section>
  </main>
`,
```

6. Now we can try out a new component into the route. In `routes.ts`, import all the needed components, like what you would do in React, and state your routes.ts like
```typescript
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { DetailsComponent } from './details/details.component';
const routeConfig: Routes = [
  {
    path: '',
    component: HomeComponent,
    title: 'Home page'
  },
  {
    path: 'details/:id',
    component: DetailsComponent,
    title: 'Home details'
  }
];

export default routeConfig;

```


<br><br>

### Detail Page that get input from the frontend

1. Note that in the `routes.ts` page, you have the `details/:id` and you got somewhere to get your house's id!
2. Your houseComponent should look like this to navigate your user to the correct page with correct id:
```typescript
template: `
  <section class="listing">
    <img class="listing-photo" [src]="housingLocation.photo" alt="Exterior photo of {{housingLocation.name}}">
    <h2 class="listing-heading">{{ housingLocation.name }}</h2>
    <p class="listing-location">{{ housingLocation.city}}, {{housingLocation.state }}</p>
    <a [routerLink]="['/details', housingLocation.id]">Learn More</a>
  </section>
`,
```
* [routerLink] is used to directive to bind the property, allow you to generate link based on provided expression.
* "['/details', housingLocation.id]": This is the expression provided to routerLink. It's an array where the first element '/details' is the route path, and the second element housingLocation.id is the dynamic parameter value.

### Get Router Parameter
1. in `src/app/details/details.componenet.ts`, in the `DetailsComponent` class exported, you get the id number like:
```typescript
export class DetailsComponent {
    route: ActivatedRoute = inject(ActivatedRoute);
    housingLocationId = -1;
    constructor() {
        this.housingLocationId = Number(this.route.snapshot.params['id']);
    }
}
```
Then you can just use in the `template` with {{ expression }}
* You can either get the params[id] inside constructor or the **ngOnInit**, constructor it would only be called once, and ngOnInit will be called multiple times.

2. Add the source of the housingLocation information in the export class:
```typescript
export class DetailsComponent {

  route: ActivatedRoute = inject(ActivatedRoute);
  housingService = inject(HousingService);
  housingLocation: HousingLocation | undefined;

  constructor() {
    const housingLocationId = Number(this.route.snapshot.params['id']);
    this.housingLocation = this.housingService.getHousingLocationById(housingLocationId);
  }

}
```
3. update the template:
```typescript
template: `
  <article>
    <img class="listing-photo" [src]="housingLocation?.photo"
      alt="Exterior photo of {{housingLocation?.name}}"/>
    <section class="listing-description">
      <h2 class="listing-heading">{{housingLocation?.name}}</h2>
      <p class="listing-location">{{housingLocation?.city}}, {{housingLocation?.state}}</p>
    </section>
    <section class="listing-features">
      <h2 class="section-heading">About this housing location</h2>
      <ul>
        <li>Units available: {{housingLocation?.availableUnits}}</li>
        <li>Does this location have wifi: {{housingLocation?.wifi}}</li>
        <li>Does this location have laundry: {{housingLocation?.laundry}}</li>
      </ul>
    </section>
  </article>
`,
```
* the `housingLocation?.wifi` the `?` is there to make sure the item is not null or undefined and so it wont crash.


<br><br>

### Adding form to the angular app
1. In the `housing.service.ts` 
```typescript
submitApplication(firstName: string, lastName: string, email: string) {
  console.log(`Homes application received: firstName: ${firstName}, lastName: ${lastName}, email: ${email}.`);
}
```
2. import statement
```typescript
import { FormControl, FormGroup, ReactiveFormsModule } from '@angular/forms';
```
3. Add `ReactiveFormsModule` into the `imports`:
```typescript
imports: [
  CommonModule,
  ReactiveFormsModule
],
```
4. `DetailsComponents` class, before the `constructor()` method
```typescript
applyForm = new FormGroup({
  firstName: new FormControl(''),
  lastName: new FormControl(''),
  email: new FormControl('')
});
```
5. and after the constructor:
```typescript
submitApplication() {
  this.housingService.submitApplication(
    this.applyForm.value.firstName ?? '',
    this.applyForm.value.lastName ?? '',
    this.applyForm.value.email ?? ''
  );
}
```
6. Change the template:
```typescript
template: `
  <article>
    <img class="listing-photo" [src]="housingLocation?.photo"
      alt="Exterior photo of {{housingLocation?.name}}"/>
    <section class="listing-description">
      <h2 class="listing-heading">{{housingLocation?.name}}</h2>
      <p class="listing-location">{{housingLocation?.city}}, {{housingLocation?.state}}</p>
    </section>
    <section class="listing-features">
      <h2 class="section-heading">About this housing location</h2>
      <ul>
        <li>Units available: {{housingLocation?.availableUnits}}</li>
        <li>Does this location have wifi: {{housingLocation?.wifi}}</li>
        <li>Does this location have laundry: {{housingLocation?.laundry}}</li>
      </ul>
    </section>
    <section class="listing-apply">
      <h2 class="section-heading">Apply now to live here</h2>
      <form [formGroup]="applyForm" (submit)="submitApplication()">
        <label for="first-name">First Name</label>
        <input id="first-name" type="text" formControlName="firstName">

        <label for="last-name">Last Name</label>
        <input id="last-name" type="text" formControlName="lastName">

        <label for="email">Email</label>
        <input id="email" type="email" formControlName="email">
        <button type="submit" class="primary">Apply now</button>
      </form>
    </section>
  </article>
`,
```

**NOTE:** `(submit)="submitApplication()"` is used to make sure the form will call a function after submit.


<br><br>

### Add the search feature to your app
1. in the `house.component.ts` or the main page
```
filteredLocationList: HousingLocation[] = [];


constructor() {
    this.housingLocationList = this.housingService.getAllHousingLocations();
    this.filteredLocationList = this.housingLocationList;
  }
```

2. Update the home component template
```
<input type="text" placeholder="Filter by city" #filter>


<button class="primary" type="button" (click)="filterResults(filter.value)">Search</button>
```

3. Implement the event handler function, `filterResults(filter.value)`
```
filterResults(text: string) {
  if (!text) {
    this.filteredLocationList = this.housingLocationList;
    return;
  }

  this.filteredLocationList = this.housingLocationList.filter(
    housingLocation => housingLocation?.city.toLowerCase().includes(text.toLowerCase())
  );
}
```

<br><br>

### Add HTTP communication to your app
1. create mock REST APIs
```
npm install -g json-server
```
2. add sample data to your db.json file
3. Run the json server
```
json-server --watch db.json
```
4. In `src/app/housing.service.ts`, remove the housingLocationList, update the getAllHousingLocation in the service.ts:
```typescript
async getAllHousingLocations(): Promise<HousingLocation[]> {
  const data = await fetch(this.url);
  return await data.json() ?? [];
}
```

and for the getHouseLocationById
```typescript
async getHousingLocationById(id: number): Promise<HousingLocation | undefined> {
  const data = await fetch(`${this.url}/${id}`);
  return await data.json() ?? {};
}
```

5. The code should look like:
```typescript
import { Injectable } from '@angular/core';
import { HousingLocation } from './housinglocation';

@Injectable({
  providedIn: 'root'
})
export class HousingService {

  url = 'http://localhost:3000/locations';

  async getAllHousingLocations(): Promise<HousingLocation[]> {
    const data = await fetch(this.url);
    return await data.json() ?? [];
  }

  async getHousingLocationById(id: number): Promise<HousingLocation | undefined> {
    const data = await fetch(`${this.url}/${id}`);
    return await data.json() ?? {};
  }

  submitApplication(firstName: string, lastName: string, email: string) {
    console.log(firstName, lastName, email);
  }
}
```

6. Since we are fetching from API, then we will need use `.then()` to wait for our response. Inside `home.component.ts`
```typescript
constructor() {
  this.housingService.getAllHousingLocations().then((housingLocationList: HousingLocation[]) => {
    this.housingLocationList = housingLocationList;
    this.filteredLocationList = housingLocationList;
  });
}
```