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



<!------------------------------------------ 5/17 Lecture (Spellbook) ------------------------------------------>

AuthController => for login
SpellsController / SpellsService / spellDex: [] (array)
ActiveSpell: {} (object)
userSpells / UserSpellsService / UserSpellsController (these are for the users spellbook)

use same env.js settings from gregslist

- AxiosService
    - in export const api..
        - rename api to SandboxApi (may need to rename it in authService.js/ accountService / )
    - (duplicate and rename) import dndApi and baseUrl (reference)
    - duplicate and rename sandbox.Api.interceptors.request.... to dndApi

create SpellsController
- export class SpellsController
    - add constructor()
in router 
- replace HomeController with SpellsController
in SpellsController
- below constructor
    - add async getSpellsFromApi(){}
        - add await
        - add try catch and pop.error
in SpellsService
- add class SpellsService{}
- add at bottom export const spellsService = ..
- (in class)    
    - add async getSpells(){}
        - add const res = await dndApi.get('/api/spells')
        - console.log('what is the res', res.data.results) (using .results because the results were a 'dot' deeper)
in SpellsController
- in async getSpellsFromApi
    - add await spellsService.getSpells()
- in constructor
    - add this.getSpellsFromApi()
in AppState
- add spellDex = []
in SpellsService
- add AppState.spellDex = res.data.results
in SpellsController
- (setup listener within constructor) AppState.on('spellDex', _drawSpellDex)
- create function _drawSpellDex()
    - console.log('spellDex', AppState.spellDex)
    - add let template = ''
    - add AppState.spellDex.forEach...(reference)
in SpellsService
- add async setActiveSpell()
    - add try catch and await spellsService.setActiveSpell(index)
- move async setActiveSpell into class SpellsService
    - revise const res = await dnd.Api...
in SpellsController
- in function _drawSpellDex
create ActiveSpell.js
- add export class ActiveSpell
    - add constructor(data) reference
    - add data and probably ask for help
- create getActiveSpellCard()
    - add return (reference)
in SpellsService
- in setActiveSpell
    - add AppState.activeSpell = new ActiveSpell(res.data)
In appstate
- add activeSpell = null
    - add @type without square brackets off at the end
in SpellsController
- in constructor
    - add AppState.on('activeSpell', activeSpell)
- create function _drawActiveSpell
    - add setHTML('activeSpell', AppState.activeSpell.ActiveSpellCardTemplate)
after adding button to active spell card - making the button only show if you are logged in 
in ActiveSpells
- add getSpellButtonBook(){}
    - if (!AppState.account)
        return '' (reference)
create UserSpellsController
- export class UserSpellsController
    - constructor
    - add async addSpell()
        - add try catch (reference)
in App.js
- add UserSpellsController = new UserSpellsController()
create UserSpellsService.js
- create class UserSpellsService
    - addSpell() reference
        - const res = sandboxApi.post('api/spells', appState.activeSpell)
- add export const...
in UserSpellController
- in async addSpell
    - add await userSpellsService.addSpell()
    - in constructor async getUserSpells()
        - try catch 
        - await userSpellsService.getUsersSpells()
in UserSpellsService
- add getUsersSpells()
    - const res = await sandboxApi.get('api/spells')
in UserSpellsController
- in constructor
    - AppState.on('account', this.getUserSpells)
in AppState
- create userSpells = [] and add @type
in UserSpellsService
- in async getUsersSpells
    - AppState.userSpells = res.data.map(s => new Spell(s) )
    (rename ActiveSpell to Spell everywhere)
in UserSpellsController
- in constructor
    - add AppState.on('userSpells', _drawUserSpells)
- create function _drawUserSpells(){}
    -console.log('my spells', AppState.userSpells)
in Spell.js
- create get UserSpellTemplate()
    - add return with the template (reference)
in UserSpellsController
- in _drawUserSpells function   
    - add let template = ''
    - add AppState.userSpells.forEach
    - add template += s.UserSpellTemplate
    - add setHTML ('userSpells', template)
in Spell.js 
- create get preparedCheckbox()
    - add if(this.prepared) reference
in UserSpellsController
- add async togglePrepared(id)
    - try catch / pop.error / await (reference)
in UserSpellsService
- add toggleSpell(id)
    - const spell = AppState.userSpells.find(s => s.id == id)
    - spell.prepared = !spell.prepared (flipping a boolean vvv)
    - const res = await sandboxApi.put('api/spells' + id, spell)
- in addSpell
    - add const newSpell = new Spell(res.data)
    - AppState.userSpells.push(newSpell)
    - AppState.emit('userSpells')


<!------------------------------------------ 5/18 Lecture (NASA APOD) ------------------------------------------>

bcw create / mvc-auth

in env.js
- copy and past content from previous lab project
in AxiosService
- copy export const api and change baseUrl and change api to nasaApi and add params key (reference)
create NasaController
- export class NasaController
    - constructor
    - console log
in router
- change HomeController to an array and add , NasaController(reference)
in NasaController
- add async getPictureOfDay
    - try catch (reference)
    - await nasaService.getPictureOfDay
create NasaService
- class NasaService
    - async getPictureOfDay
    - const res = await nasaApi.get()
    - console log
- (at bottom) export const nasaService = ...
in NasaController
- in export class
    - this.getPictureOfDay
mapping the object vvvv
create NasaPicture.js
- export class NasaPicture
    - constructor
    - add data
in NasaService
- in async getPictureOfDay
    - console log('creating nasa picture', new NasaPicture(res.data)) 
in AppState
- add nasaPicture = null and add the @type
in NasaService
- in async getPictureOfDay
    - add appState.nasaPicture = new NasaPicture(res.data)
in NasaController
- create function _drawPicture
    - const picture = appState.nasaPicture
    - document.body.style.backgroundImage = `url(${picture.image})`
- add AppState.on('nasaPicture', _drawPicture)
- add _drawPicture() in constructor
- (you can style background image in css)
getting title,date,description by adding a template
(build it out in html first then bring it to NasaPicture and create a template)
in NasaPicture
- (reference template) and reference css  
- reference pictureTitle:hover in css
- add get NasaPictureTemplate
    - return (add html here)
in NasaController
- in function _drawPicture
    - add setHTML('pictureInformation', picture.NasaPictureTemplate)
in html
- in header reference calender form thing
- add onchange to input
in NasaController
- create async selectDat()
    -try catch
    - await nasaService.selectDate(date.value)
in NasaService
- add async selectDat()
    -const res = await nasaApi.get('?date=${date}')
in NasaController
- in async selectDate
    - add let dateElem = document.getElementById('date')
    - let dateValue = dateElem.value
in NasaService
- in async selectDate
    - add AppState.nasaPicture = new NasaPicture(res.data)
adding the ability to save the current image
this is where sandbox comes in 
create Sandbox Controller
- export class SandboxController
    - constructor
create SandboxService
- class SandboxService
    - export const sandboxService = new SandboxService()
in router 
- add SandboxController
in SandboxController 
- create async favoritePicture
    - let favoriteData = AppState.nasaPicture
    - try catch
    - await sandboxService.favoritePicture
in SandboxService
- add async favoritePicture(favoriteData)
    - const rest = await api.post()
    - console log
in html
- add button to save to favorites and add an onclick (reference)
in sandboxController/service
- add async quick (reference)
in AppState
- add sandboxPictures = []
in SandboxService
- in async favoritePicture
    - add AppState.favoritePicture...
in SandBoxPicture
- export class...
    - constructor()
        - this.date....(reference)
in SandboxService
- in async favoritePicture
    - add AppState.sandboxPictures.push(new...)
    - (reference)
    - AppState.emit('sandboxPictures')
in sandboxCOntroller
- add async getFavoritePictures
    - try catch
    - await sandboxService.getFavoritePictures()
In sandboxService
- add async getFavoritePictures
    - const res = await api.get('api/apods')
in sandboxController
- AppState.on('account', this.getFavoritePictures)
in sandbox controller
- in async getFavoritePictures
    - in console log add res.data.map...
    - add AppState.sandboxPictures = res.data.map(p => new SandboxPicture(p)) (sandBoxService)
add an off canvas (also add favorite pictures id)
in sandbox controller
- create function _drawFavorite
    - let template = ''
    - appState.sandboxPictures.foreach(p => template += p.FavoriteTemplate)
    - setHTML('favoritePictures', template)
- below appState.on(account) (reference drawing favorites after login confirmed)
add remove button
in sandboxController
- add async removeFavorite
    -try catch
in sandboxService
- add async removeFavorite(favoriteId)
    -const res = await api.delete('api/apods/favoriteId?)
    - AppState.sandboxPictures = AppState.sandboxPictures.filter(p => p.id!=favoriteId)


