# Async

<!-- SECTION 5/16 GREGSLIST ASYNC LECTURE -->

bcw create => mvc auth

in env.js
(where we go to load up the environment for the application)
- go to sandbox and copy the keys and paste them in env.js
- paste in appropriate export const
- take sandbox url and paste into baseUrl
(bearer token is the access token)

building out gregslist
in html
- adding a href 'cars' (for cars page)

create CarsController.js
- add export class CarsController
- add constructor
    -log something
- only want cars controller to be working while on cars page
in router
- replicate paths except change it for the cars

in CarsController
- (when i come into cars page, lets get cars)
    - 
create CarsService.js
- add class CarsService
- at bottom export const carsService = ...
- (in class CarsService)
    - add async getCarsFromApi()
    - const res = await api.get('api/cars')(see reference)
in CarsController
- below constructor   
    - add async getCarsFromApi(){}
    - try (reference)
- in constructor
    - add this.getCarsFromApi()
in CarsService
- in async getCarsFromApi
    - add 
create Car.js
- add export class Car{}
    - add constructor(data)
    - this.id = data.id
    (reference data)
- below constructor
    - add get CarCard()
    - add return /*html*/ (reference)
in CarsService
- in async getCarsFromApi
    - add AppState.cars = res.data.map(reference)
in appstate
- add cars = []
- add @type above cars array

(adding a loading thing when you click on cars and are waiting for the cars to draw)
in CarsController
- (drawing a console log to page)
- add function _drawCars()
    - let template = ''
    - appState.cars.forEach(reference)
    - setHTML('app', template)
- in constructor 
    - add AppState.on('cars', _drawCars)
    - also add setHTML(with the app and vroom vroom)
in car.js
- add static CarForm(){
    return'' (reference form)
}
in CarsController
- add setHTML ('modal-guts', Car.CarForm())
in CarsController
(getting it so that you cant add a car or see listings unless you are logged in)
- in constructor
    - add setHTML('the-place-to-put-the-button', )
in AboutController/HomeController
- see where the place to put the button (setHTML (provide the id but then leave open ticks))
^^^ it is referencing the id but isnt presenting anything to the page
in CarsController
- in constructor
    -add AppState.on('account', _drawButton)
- add function _drawButton(){}
    - add if(appstate.account)
    - setHTML
- in constructor
    - add _drawButton()
- add async createCar(){}
    - add try catch (reference)
    - reference for the rest
    - add carsService.createCar(formData)
in CarsService
(getting a new car to draw to the page when listing is created)
- add async createCar(formdata){}
    - add const res = ...
    - reference
    - const newCar = new Car(res.data)
    - AppState.cars.push(newCar)
    - AppState.emit (triggers the draw)
    (vvvv getting modal to close if submit was successful)
    - (after form.reset) bootstrap.Modal.getOrCreateInstance('#modal).hide()
vvvv adding the delete button
in Car.js
- add delete button to get CarCard
    - add onclick (reference)
in CarsController
- add await async deleteCar(id)
    - try catch (reference)
in CarsService
- add async deleteCar(id)
    - const res = await...(reference for rest)
(getting it so that when you delete a car it automatically removes it from the page without refreshing the page)
in CarsService.
- in class CarsService
    - add appState.cars = AppState.cars.filter(reference)
(getting the delete button to show only if you were the user that posted the listing)
in Car.js
below car card
- create get deleteButtonIfCarIsYours(){}
    - if (this.creatorId != AppState.account?.id)
    - return (insert delete button)
- add ${this.deleteButtonIfCarIsYours} (add this where button was originally placed)
in CarsController
- add AppState.on('account', _drawCars )