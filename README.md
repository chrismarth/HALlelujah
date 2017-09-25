# Hallelujah

Hallelujah! This Angular module offers a [HAL/JSON](http://stateless.co/hal_specification.html) http-client to easily interact with a [Spring Data Rest](https://projects.spring.io/spring-data-rest) API (and by extend any API that conforms the Spring Data Rest resource model)

!! This module needs Angular version 4.3+ since it uses the new HttpClientModule introduced in 4.3

Happy coding! Feedback much appreciated!

## Installation

```
npm install ng2-hallelujah
```
## Configuration

1. Import HallelujahModule in your app root module
2. ResourceService is the entry-point for interacting with Spring Data Rest resources. Their should be only one application-wide ResourceService. So we add it to the providers of our app root module.
3. Set the base URL of our Spring Data Rest API with the API_URI InjectionToken. Either put a string value or use an environment variable (best practise in multi-environment deployments)

```typescript
import {NgModule} from '@angular/core';
import {BrowserModule} from '@angular/platform-browser';
import {HallelujahModule} from './hallelujah/hallelujah.module';

import {AppComponent} from './app.component';
import {environment} from '../environments/environment';
import {API_URI, ResourceService} from './hallelujah/resource.service';

@NgModule({
  declarations: [
    AppComponent,
  ],
  imports: [
    BrowserModule,
    HallelujahModule
  ],
  providers: [
    ResourceService,
    { provide: API_URI, useValue: environment.api_url }
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
``` 

## Usage
First of all let's model our resource entities.  
To illustrate we use a simple model of a team and players  
Notice that our entity class extends the Resource class.  
By inheriting the Resource class we give HAL specific features to our entity 

**Attention**: The name and type of the members of your resource class must exactly match the name and type of the members of the resource entity exposed by your API  

```typescript
export class Player extends Resource{
    firstName: string;
    lastName: string;   
}
```
Since a Team consists of multiple players, we model the one-to-many relationship between the Team resource and the Player resources
```typescript
export class Team extends Resource {
    name: string;
    players: Player[];
}
```
So far so good, time to make our application interact with the API.  
To illustrate we create a TeamManagerComponent that will implement some basic CRUD on our resources.

```typescript
@Component({...})
export class TeamManagerComponent implements OnInit {

  teams: Team[];
    
  constructor( private rs: ResourceService ) { }

  ngOnInit() {
    this.getAllTeams();
  }

  getAllTeams() {
    this.teams = this.rs.getAll(Team, 'teams');
  }
 }
```
Our component constructor has an argument of type ResourceService. Upon creation of the component the ResourceService instance we defined earlier as a provider in our root module will be injected and be available for further use in the component.  
We create a function getAllTeams() which will fetch all the teams from our backend.
To fetch these teams we use the getAll method of the ResourceService. This method requires 2 parameters:  
+ The type of the resource
+ The relative URI path of the resource


##API
###ResourceService

###Resource

###Page
 
## Demo Application

## Roadmap

+ Write unit tests (I know)
+ Error handling
+ Security Interceptors (Basic, JWT)
