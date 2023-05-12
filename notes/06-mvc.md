# MVC

May 8 (week 3)

model is almost like the empty template (also seen as a definition of a class)

do npm i -g bcw
then do bcw create
then select mvc

<!-- ------------------------------------------------------------------------------------------------------------------------ -->
May 9 (tuesday) Lecture - Encapsulation / Monday lecture recap
Gachamon lecture
MVS 


step by step

-very first thing is create js in models (for example Gachamon.js)
-(if we want to be able to reference this class elsewhere make sure to add an export before defining it)(this class serves as a blueprint, similar to an array)
    put export class House(){
        -add constructor and invoke
        -(reference construction analogy/ gachamon.js(models) from lectures)
    }
- next throw in > new House with all objects (reference house.js from lecture)
(4 pillars of object orientation)

next store that class in appState
// class/model is what i want my data to look like... the appState is where i want the data stored
- create gachamon arrary
    gachamon = {
        new Gachamon({ properties and values from Gachamon.js here})
    }
    (reference lecture)

NEXT
- create GachamonService.js
- create GachamonService class
- create/export const gachamonService (different from GachamonService class) = new GachamonService()
// NOTE  services are for 'business logic' .... anytime i want to manipulate or alter my data (AppState), it will happen in the service layer
-create GachamonController.js (takes inputs/ anything the user will interact with... will come through the controller first) 

//NOTE app.js file is the window between your html (the user) and the rest of your javascript
-(in app.js) gachamonController = new GachamonController()
- (in gachamonController) export class GachamonController
    export GachamonController {
        constructor {
            console.log('hello from ')
        }
    }   (reference lecture)

NEXT
- in html build out template
//NOTE - if element error pops up make sure the id ='app' is in html
(reference html from lecture)
- copy html (commented out) 
- (in Gachamon.js) do get ListTemplate(){
    return '
    paste stuff from html here || or you can use string interpolation ${this.emoji} (reference Gachamon.js 'ListTemplate)
    '
}
//NOTE - anytime you want to manipulate or change the dom (what the user sees) draw something do it in the controller
- (in GachamonController.js) create function drawGachamon(){
    (add _drawGahcamon in constructor of GachamonController // underscore before drawGachamon is professional practice)
        (declare this outside of your class)
        (reference lecture)
}
- (in drawGachamon function) let gachamon = appState.gachamon
- next let template = ''
- next (reference lecture)
- next add id to html
- next setHtml('id from html', template)
//NOTE - line above does the same as document.getElementById
- (in AppState reference) @type tag above gachamon array
In GachamonController 
- (in export class GachamonController) do setActive (method) and add a console log
- in set active add gachamonService.setActive -> reference lecture
    -  hover on setActive and click ctrl period (this will auto declare method into GachamonService)
In GachamonService
- console log in set active method
- add let foundGachamon = (reference)
- console log
- add appState.activeGachamon = foundGachamon
In AppState
- create activeGachamon = null
- add @type import above activeGachamon (reference)
//NOTE ^^ null means it is either there or it is not
In html
- hard code what you want it to look like
(elevation adds box shadow - reference)
In Gachamon.js
- get ActiveTemplate(reference)
In GachamonController
- add function _drawActive()
- (in export class GachamonController) add appState.on(reference)
In controller
- in draw active function add let active = appState.active
- then add setHtml('active', active) add id to html (reference)
Getting buttons to add coins
- make new controller file
- export class(reference)
Create new Service class
- class coinsService
- export const coinsService
In AppState
- coins = 0(reference)
In app.js
- coinsController = new CoinsController()
In html
-add onclick to add coins (reference)
In Coins Controller
- addCoins
-add coinsService -> ctrl period function
In Coins Service
- in class CoinseService addCoind(){
    appState++ (reference)
}
In coins controller
- add function _drawCoins
- (in class Coins Controller) appState.on
- let template = ''
- (in drawCoins function) add for loop 
let coins = appState.coins
(reference)
- add coins id to html
- (in function drawCoins) add set coins
dispense function in GachamonServices file(reference)
create new array in appstate to save
- myGachamon(reference)
add push to appState
In controller
-add drawMyGachaMon function
- add appState myGachaman in GachamanController class
In AppState
- reference load state / 
In GachamonService file
also reference saveState in dispense function 
<!-- ----------------------------------------------------------------------------------------- -->



Wednesday 5/9 lecture (Gregslist)

Start with creating model js (Car.js)
- export class car 
    - with constructor (data) and add array info
    - (reference)
- (in appState) add cars = [new Car]
- create CarsController.js
in CarsController
- create export class CarsController
- add constructor in class
- create carsService.js
in carsService
- add class CarsService 
- add export const carsService = new CarsService
in app.js
- add carsController = new CarsController
//NOTE -  as soon as you want to get something on the page, hard code what you want it to look like in html
in CarsController
- create function _drawCars
- add draw cars function in CarsController.js
- create get cars and calls _draw cars underneath
in html
- add onclick to button (reference app.) and invoke
in Car.js
- add getter class with CardTemplate (reference)
^ this gets the data from the constructor above it
in cars controller
- in draw cars function add let cars = appState.cars
- also add let template = ''
- next add cars.forEach() => (reference)
- next add setHtml beneath and add id (for example ('cars', template))
in html
- add id to where you want the template to come into^
in carsController.js
- add _drawCars in CarsController function
modals
- reference bootstrap
- copied live demo modal into html
- after modal is done
go into car.js
- add get active template with modal template from html and add string interpolation 
- add this.data = data.id || generateId() in data array of constructor
in CarsController
- add setActive(carId)
- in set active add carsService.setActive(carId)
in carsService.js
- let foundCar = appState.cars.find
in appState
- add activeCar = null (within cars array)
- add @type function thing above it
in CarsService
- in class CarsService add appState.activeCar = foundCar
in CarsController
- add new function _drawActive()
- in class carsController add appState.ono(activeCar, _drawActive)
in html
- add id behind "modal-content"
in CarsController
- (in _drawActive function) add let car = appState.ActiveCar then add setHTML
//NOTE - below are steps to add a car posting from site
- (html) add button "list car" 
- add modal tags to button (reference)
- copy modal 'guts' and copy and revise to use as a form template
- (reference form template)
- (when form is complete and ready to add submit action) add onsubmit on form(reference)
in CarsController
- add create car()
- in createCar add windows.event.preventDefault
in html
- add onclick app.carsController.getCarForm()
in carsController
- add getCarForm()
- (copy car form template into car.js add static) Carform() (within constructor class)
- (beneath getCarForm) add setHTML
- (in createCar) const formHTML = event.target
- (^) add const formData = getFormData(formHTML)
in Car.js
- (within createCar function that has the form details) add name = model
in carsController
- add cars.Service.createCar .... (reference)
in CarsService 
- (in createCar) add let newCar = new Car(formData) & add appState.cars.push(newCar)
- also add appState.emit
in carcontroller
- (in createCar) add formHtml.rest() THEN add bootstrap.Modal...
- (in export cars) CarsController add appState ....
vvvvv notes on local storage vvvv
in CarsService.js
- ( in createCar) add saveState('cars', appState.cars) || add a function _saveCars and call it in createCar (reference)
in AppState.js
- (in class AppState extends...) add cars = loadState('cars', [Car])
in html
- (hardcode) add 'login' button
add new controller for login (UserController.js)
in UserController
- add export class UserController with constructor
add UserService.js
- add class user
in app.js
- add userController = new... in class
in UserController
- add async enterUserName in UserController class
in html 
- add onclick to button
in UserController
- then add let input = await pop.prompt (if pop doesn't import you can manually import Pop at top of page. see reference)
- the add input console.log (if you want to )
- then add userService.enterUserName(input)
in UserService
- add enterUsername (input)
- then add appState.userName = input
in AppState
- add userName = ''
in UserController
- add if(!input) return
in Car.js
- (in class Car) add this.creatorName = data.creatorName
in CarsController
- (in createCar) add formData.creatorName = appState.userName (above carService)
in html
- add id to section holding getCarForm button
in car.js
- add static CreateCarButton and paste html (reference)
in carsController.js
- create function _drawCreateCarButton
- add setHtml into function above (reference)
- add appState.on('', _drawCreateCarButton in controller class)
in Car.js
- add ${this.creator} in car template
//NOTE - deleting posting (below)
in html
- hardcode delete button in car.js in get ActiveTemplate
- add onclick to button (reference delete)
in CarsController.js
- add deleteCar(carId)
- then add if(await pop.confirm)....
- then add carsService.deleteCar(carId)
in carsService.js
- add deleteCar(carId)
- then add let carToDelete = appState.cars.find....
- then add appState.cars = appState.cars.filter(...)...
- then add _saveCars()
// IN GITHUB



<!-- ---------------------------------------------------------------------------------------------- -->

Thursday 5/11 lecture (Redacted)

start by creating model
- case.js
- add export class case & add constructor with this.data... 
- import generate id
add data in AppState
- case = [...]
- make sure case.js is imported
create CasesController.js
create CasesService.js
in CasesController.js
- create export class CasesController
in CasesServices
- create export const casesService = newCasesService()
register controller in App.js
- add casesController = newCases... (within class App{}) & comment out values controller
in CasesController
- add console.log to make sure its working
this is where you start adding stuff to the page through html
- reference case list section & case template
- comment out template and move to model( get CasesTemplate)
in CasesController.js
- add function _drawCases()... (reference)
- add _drawCases in export class CaseController
- (in draw cases function) let cases = appState.cases
    - then add let template = ''
    - add cases.forEach
    - add setHTML... 
in HTML
- add id to cases list
in Case.js
- create get ComputeDate
    - add let date = this.date
    - add return(date.getMonth()) + '/' + (date.getDate()) + '/' + (date.getFullYear())
- create get ComputeTitle()
    - add return (this.reportBody.slice(specify what part of the string we want to see here. By position ) + '...')  (reference)
- in CasesTemplate
    - add onclick app.casesController.setActive (reference)
in CasesController.js
- create setActive
    - add console log to make sure it works
- in setActive
    - add casesService.setActive(caseId) and declare the method so it populates class CasesService in CasesService.js
in CasesService.js
- in class CasesService
    - add let foundCase = appState...
    - add console.log
in AppState.js
- add @type
- add activeCase = null
in CasesService.js
- in class CasesService
    - add appState.activeCase = foundCase
    - add console.log to make sure it works
- create function _drawActiveCase
go in html and hardcase your active case template (see active case reference)
- after it looks how you want it - copy template from html into model (Case.js)
in Case.js
- create get ActiveCaseTemplate()... 
    - add return then add template from html
in CasesController.js
- in _drawActiveCase function
    - add let activeCase = appState.activeCase
    - add setHTML
- in export class CasesController
    - add appState.on(...)
in AppState.js
- (reference classified words)
in Case.js
- create get ComputeRedactedReport (this get is used to hide sensitive info)
    - add let originalReportArray = this.reportBody.split(' ')
    - add let redactedReportArray = originalReportArray.map(word....) (reference)
    - if (appState.classifiedWords.includes(word.toLowerCase))
    - add return 'black squares emoji here' (reference)
    - return word
    -return redactedReportArray.join(' ')
    - rename activeCaseTemplate to redactedReportTemplate
clicking on eye button to show everything in report
in Case.js
- add onclick to button
- add onclick to unredactedReport button as well
in CasesController.js
- in _drawActiveCase function
    - add if (activeCase.unlocked)... (reference)
    - add setHTML ('activeCase') reference
- create unlockReport()reference
    - add console
    - add casesService.unlockReport
in CasesService.js
- in class CasesService
    - add unlockReport()
    - add let foundCase = appSate.activeCase
    - add foundCase.unlocked = !foundCase.unlocked
    - add console log
    - add appState.emit('activeCase')
create UserController.js
create UserService.js
in App.js
- add userController = new UserController
reference agencies in AppState.js
in UserController.js
- add async verifyUser function (within export class UserController)
    - add let input = await Pop.prompt('Please put in your agency code)....
    - add this verifyUser()
    - add userService.verifyUser(input) // declare verifyUser
inUserService
- add let agencies = appState.