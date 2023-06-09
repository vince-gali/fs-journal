# Vue


5/30 - Gregslist using different framework

bcw create
vue starter

"controllers" are now used in the pages vue
in html use script tag to import 'controller' (vue) and also now include css in html

if you want anything hard coded to the dom, regardless of what page you're on, do it in the app.vue file
v-if directive (if something is true show it to me)
- reference HomePage.vue 
    - disable button if no one is logged in
    - (reference) :disabled= "account.id"
    - reference account: computed(() => AppState.account)
    - also reference increment
        - @ increment button => @click="incrementNumber"

~starting gregslist here~

set up env
- no local service in baseURL so change it to sandbox

(if i want a page to navigate to)
in router.js
- add another "path" in const routes

making the cars page work
in pages folder create CarsPage.vue
- (reference snippet from sam)
- 'hello from the cars page in template' (to make sure it works)

in Navbar.vue
- add li and router-link :to... (reference)

in CarsPage.vue
- add async function getCars.. (reference)
bring in 'lifecycle hook'
- add onMounted (similar to calling function/method in constructor of a controller)

create CarsService.js
- initial setup for service is same as before

in CarsPage
- add await carsService.getCars()

in CarsService
- add async getCars()
    -reference

create Car model
- (reference) info is same as before
- reference createdAt & creator

in AppState
- cars : []
- add @type import

(stretch goal)
in Account.js
- export class Account extends Profile
    - reference

in CarsService 
- add AppState.cars = res.data.map(...)

raw data dump to make sure cars show on page
in CarsPage.vue
- within template, add {{cars}}
    - cars info should show on page without any styling
- revise template to how you want it to look when it loads(reference)
Create CarCard.vue in components folder
- copy template here (reference)

in CarsPage.vue
- add <CarCard/>

CarCard.vue
- add props: ...(reference)

CarsPage
- in template
    - <CarCard : car="c"/> (binding)

in router.js
- add path / carDetails

create CarDetailsPage.vue
- test to see if you can get the car details page to render

CarCard.vue
- add router-link :to... (car details) (reference), params: {carId: CarProp.id}

CarDetailsPage
- const route = useRoute (reference sam's notes)
- const router = useRouter (reference sam's notes)
- async function getCarById()
- onMounted(reference)
    - getCarById

CarsService
- async getCarById()
    -reference

AppState
- activeCar = null and add @type

CarsService
- in getCarById
    - AppState.activeCar = ...

CarDetailsPage
- reference template

reference for creates and deletes

showing a create car button
CarsPage
- add button

create Modal.vue in component folder
- insert modal
- reference

in App.vue
- reference modal below the footer
    - <Modal id="create-car" />
CarsPage
- add data-bs-toggle / target etc etc

create CarForm.vue

App.vue
- insert <CarForm/> inside modal

CarForm
- copy modal guts into template here
- async submit carForm
- setup
    - const editable = ref( )
CarsService
- async createCar

deleting car

in CarCard (for UI if you are going to use an icon as or on a button you must have a title tag that denotes its purpose)
- reference delete button n CarCard
- add account computed (reference)
- in setup
    - add async deleteCar



5/31
bcw create
vue-starter

Axios Service
(when using an api)
- export const movieApi = Axios.create(reference)
    - baseURL
    - timeout
    - params
    - api_key

(sam uses HomePage as movies page)
HomePage.vue
- within export default setup
    - async function getMovies (reference)
        -try catch
    - onMounted
        - getMovies()
create MoviesService
- async getMovies
- const res = await movieApi.get('discover/movie')
- (reference)

create Movie.js
- export class Movie
    - constructor(data)
    (reference)
MoviesService
- getMovies
    - add AppState.movies = res.data.results.map(m => new Movie(m))
//NOTE - cannot map over an object / use map for arrays ('res.data.map is not a function' - make sure you're using .map on arrays)

AppState
- movies: [] & add @type

HomePage
- add movies: computed...(reference)
(to bring in data from appState, use computed)
- add {{movies}} into template
    - then hard code your template to look how you want it

create MovieCard.vue
- move template from HomePage to template in MovieCard
(instead of abstracting our template to our models, we are now abstracting it to its own component)

HomePage
- insert <MovieCard / > within template

MovieCard
- add props: {
    movieProp: {type:Movie, required:true}
} (reference for rest of prop)

HomePage
- (bind) add :movieProp = "m" to MovieCard in template

MovieCard
- reference src in template

(clicking on a movie card and going to a movie details page) vvvvv
MovieCard
- add router-link :to="{name: 'MovieDetails', params:{movieId: movieProp.id}}" to template

create MovieDetailsPage

in router
- (reference path: movie/:movieId /name: MovieDetails)
( if you want to navigate to another page, add another path in the router)

MovieDetailsPage
- const route = useRoute(), 
- async function getMovieById(reference)
- try catch
- const movieId = route.params.movieId (grab the movie id from the URL)
- await moviesService.getMovieById(movieId)

MoviesService
- getMovieById(movieId)
    - const res = await movieApi.get('movie/${movieId}')
    - AppState.activeMovie= new Movie(res.data)

MovieDetailsPage
- add movie: computed (()=> AppState.activeMovie)
- reference src in template
reference background img stuff

create SearchBar.vue (component)
- insert input field into template (reference) form @submit searchMovie()
- <SearchBar />
- async searchMovie()(reference)
- (sim to window preventDefault )use @submit.prevent = searchMovie()
- const searchTerm = ref('')
- within the input in template, add v-model="searchTerm"
- const searchTerm = search.value
- await moviesService.searchMovie(searchTerm)

MoviesService
- async searchMovies()
    - const res = await movieApi.get('search/movie', {params: query})
    - (reference params)
(getting movies to draw based on the search) vvvvv
- in async searchMovies
    - AppState.movies = res.data.results.map(m => new Movie(m)) 

AppState
- currentPage: null,
- totalPages: null

MoviesService
- getMovies
    - AppState.currentPage = res.data.page
    - AppState.totalPages = res.data.total_pages

HomePage
- currentPage: computed(reference)
- totalPages: computed(reference)

(when clicking on previous or next button, bring to next page and get results)
HomePage
- async changePage
    - if(pageChangeSwitch == 'next') (reference for else if)
    - AppState.currentPage++
    - await moviesService.changePage()
(reference)
- (on buttons) add @click="changePage('previous / next (for each reference)')"

MoviesService
- changePage()
    - const res = await movieApi.get('discover/movie?page=${AppState.currentPage}')
    - AppState.movies = res.data.results.map(m=>new Movie(m))
    -AppState.currentPage...
    - AppState.totalPages... (reference)
AppState
- query: null
MoviesService
- searchMovies
    - AppState.query = searchTerm
- changePage
    - const savedQuery = AppState.query
    - if(!savedQuery ){reference}
    - AppState.movies = res.data.results.map(m=>new Movie(m))


when will i be editing or changing App.vue???

6/1 Lecture - Artsy
vue-starter 

start server/client
make sure env is setup correctly so that you can log in

(editing accounts)
- AccountPage.vue
    - (reference scoped lang in this page) (scoped means the styling will only affect this page)
    - reference coverImg: computed....
    - styles template
(building a form) - don't build forms inside pages/ instead build forms in another component
create AccountForm.vue
- add form into template
    - @submit.prevent
- account: computed(()=> AppState.account)
- test raw data dump

in AccountPage
- add <AccountForm /> within template

AccountForm (anytime you submit a form, the notes below will always be the same)
- (editing cover img)
- const editable = ref(AppState.account)
- watchEffect(()=> {editable.value = {...AppState.account}})  (spread operator)//NOTE - what does watchEffect do?
    - return{ editable, handleSubmit{reference}}
- in template
    - v-model="editable.name"
- await accountService.editAccount(editable.value)
create AccountService
-  class
    - (reference getAccount & editAccount)
- export const accountService = new AccountService()
- async editAccount
    - const res = api.put('/account', formData)
    - AppState.account = ...

getting data to page
create Project.js
- export class
    - constructor(w data)

Create ProjectsService
- class 
    - async getProjects
        - const res = await api.get('api/projects')
        - logger
- export const...

HomePage
- (invoke get projects)
- await projectsService.getProjects
- async function getProjects
    - (reference)
- onMounted(()=> getProjects())
- projects: computed(()=> AppState.projects)

AppState
- projects: [] & @type import

ProjectsService
- AppState.projects = res.data.map(p => new Project(p))

do raw data dump then build template

create ProjectCar in components
- vt
- props: projectProp {reference}

HomePage
- add v-for="p in projects"
- <ProjectCard :project="p" />
- key="p.id"

ProjectCard
- project.coverImg
- {{project.title}}
- reference styling
- {{project.creator.name}}
- reference img src within em tags
- reference .length

App.vue
- inserts modal (projectModal)

ProjectCard
@click = "setActiveProject"
- add data-bs-target="#projectModal"
- data-bs-toggle ="projectModal"
- setActiveProject
    - AppState.activeProject = props.project

App.vue
- {{project.title}}
- project: computed(()=> AppState.activeProject)
- v-for="img in project?.projectImgs"
- put a v-if project in modal-content you wont need to use an elvis operator
- add img :src for displaying img

making profile clickable
ProjectCard
- wrap project.creator.picture in a router-link :to="(reference)"

router
- add path: '/profile/:id'
(reference)

create page (ProfilePage.vue)
- vt
- const route = useRoute()
- async function getProfile
    - try catch
    - 
- onMounted
    - getProfile()

create ProfileService
- class
- export const
- async getProfile(id)
    - const res = await api.get('api/profiles/' + id)
    - logger

ProfilePage
- await profileService.getProfileById(route.params.id)

AppState
- activeProfile: null & @type

ProfileService
- async getProfileById(id)
    - (reference)

create Profile.js
- reference

ProfilePage
- profile: computed(()=> AppState.activeProfile)
- data dump
- <ProfileCar />

create ProfileCard
- prop
    - profile(reference)

ProfilePage
- <ProfileCard :profile= "profile"/>

- async function getProjectsByProfile
    - (reference)
- onMounted
    - getProjectsByProfile

ProjectsService
- async getProjectsByProfile
    - reference

ProfilePage
- reference v-for p in projects



<!-- 6/5 Building full stack app -->

starting off building the backend

bcw create - express-vue

open the workspace
setup env and env.js
start up client and server

start with a model (in server)
create Album.js
- export const AlbumSchema = new Schema(){}
    - title:{...reference}
    - category..
    - coverImg
    - creatorId:{type: Schema.Types.ObjectId...}
    - archive{type: boolean}
    - {timestamps:true, toJSON....}
- AlbumSchema.virtual('creator',)
    - local field
    - foreign field
    - ref 
create AlbumsController
create AlbumsService

in dbContext
- register Albums
    - Albums = mongoose.model('Albums', AlbumSchema)

AlbumsService
- export const AlbumsService = new

AlbumsController
- export class AlbumsController extends BaseController
    - constructor
    - super('api/albums')
    - this.router

in postman go to tests
copy bearer token from dev tools(preview)
- add into auth variable and save

AlbumsController
- this.router
    - .use(Auth0Provider.getAuthorizedUserInfo)
    - .post('', this.create)
- async create(req,res,next)
    - try catch
    - req.body.creator = req.userInfo.id
    - const album = await albumsService.create(req.body)
    - next(error)
    - return res.send(album)

AlbumsService
- create(albumData)
    - const album = await dbContext.Albums.create(albumData)
    - await album.populate('creator')
    - return album

AlbumsController
- super
    - .get ('', this.findAllAlbums)
    - .post
    - .get('/:albumId', this.findAlbumById)
- async findAllAlbums
    - reference
- async findAlbumById
    - reference

AlbumsService
- async findAllAlbums
    - reference
- async findAlbumById 
    - reference

Controller
- super
    - .delete('/:albumId', this. archiveAlbum)
- async archiveAlbum(req,res,next)
    - reference

AlbumsService 
- async archiveAlbum (albumId, userId)
    - const album = await this.findByAlbumId(albumId)
    - if(album.creatorId == userId) throw new Forbidden('')
    - album.archived = !album.archived (or true)
    - await album.save()
    - return album

create Picture.js 
- export const PictureSchema = new Schema{} (reference) 
    - imgUrl: ... reference
    - creatorId
    - albumId
    - timestamps
- PictureSchema.virtual('creator')
    - localField
    - foreignField
    - justOne
    - ref

DbContext
- register Pictures = mongoose.model('Picture', PictureSchema)

create PicturesService
- class PicturesService{}
- export const picturesService = new PicturesService

create PicturesController
- export class PicturesController extends BaseController
    - constructor
    - super
    - this.router
        - .use(Auth0Provider...)
        - .post('', this.create)
- async createPicture
    - reference
    - try catch
    - req.body.creatorId = req.userInfo.id
    - const picture = await picturesService.createPicture(req.body)

PicturesService
- async createPicture(pictureData)
    - const picture = await dbContext.Pictures.create(pictureData)
    - await picture.populate(creator)
    - return picture

(getting pictures from an album)
AlbumsController
- super
    - .get(/:albumId/pictures, this.findAlbumPictures)
- async findAlbumPictures
    - reference

PicturesService
- async findAlbumPictures(albumId)
    - reference
    - const pictures = await dbContext.Pictures.find({ albumId: albumId }).populate('creator')

<!-- 6/6 Full stack app continued (Post-it front end) -->

in server create .env file
- paste info

start client

go in client folder
create Album.js (in client folder reference Album.js in server(backend) folder// model icon will be blue)
- export class Album
    - constructor
        - reference data from backend
- Account.js
    - reference
create Picture.js
- export class Picture
    - constructor
        - reference data

create AlbumsService
- class AlbumsService
- export const albumsService = new ...

HomePage
- within setup
    - async function getAlbums()
        - reference
        - await albumsService.getAlbums()

    - onMounted(()=> getAlbums())

AlbumsService
- async getAlbums()
    -  reference
    - const res = await api.get('api/albums')
    - logger.log([GETTING ALBUMS], res.data)

AppState
- albums: [] (& add type import)

AlbumsService
- getAlbums
    - AppState.albums = res.data.map(a => new Album(a))

Homepage
- return
    - albums: computed(()=> AppState.albums)
        (use computed to access data in the AppState)
- raw data dump
- after dump build out rough template in HomePage

create AlbumCard (component)
- past template AlbumCard here

HomePage
- within albumCard area
    - add v-for and <AlbumCard :album="a" />

AlbumCard 
- export default
    - album (props)
        - reference
- inject data within template

sam saves background img in assets and puts it in the app.vue
- reference background-img css class

router.js
- create album path (reference)

create AlbumDetailsPage
- vt
- throw in random text to make sure it shows up when you navigate to the details page

AlbumCard
- wrap card in router-link :to="{name: 'AlbumDetails', params:{id:album.id}}"

AlbumDetailsPage
- const route = useRoute()

- async function getAlbumById
    - const albumId = route.params.id 
        - (use id at the end because we used id in the router path)
    - reference

- async function getPicturesByAlbumId
    - reference

- onMounted
    - getAlbumById
    - getPicturesByAlbumId

AlbumsService
- async getAlbumById(albumId)
    - const res = await api.get(`api/albums/${albumId}`)

AppState
- activeAlbum: null

AlbumsService
- getAlbumById
    - AppState.activeAlbum = new Album (res.data)

create PicturesService
- class PicturesService
- export const picturesService = new PicturesService()

AlbumDetailsPage
- getPicturesByAlbumId
    - const albumId = route.params.id
    - await picturesService.getPicturesByAlbumId(albumId)

PicturesService 
- async getPicturesByAlbumId(albumId)
    - AppState.pictures = [] (gets rid of 'ghost' data)
    - const res = api.get('api/albums/${albumId}/pictures') (reference AlbumsController in backend)
    - AppState.pictures = res.data.map(p=> new Picture(p))

AppState
- pictures: []

AlbumDetailsPage
- builds out details page in template
- return
    - album: computed(()=> AppState.activeAlbum)
    - pictures: computed(()=> AppState.pictures)
 
 modal and form for creating albums

 create Modal.vue component
 - vt
 - use slot to make modal reusable?

 App.vue
 - under footer add <Modal id="createAlbum"/>
 add create album button in navbar
 - data-bs-toggle
 - data-bs-target (reference)

 create CreateAlbumForm
 - paste base template of modal

 App.vue
 - <Modal id=..
    - <CreateAlbumForm/>

CreateAlbumForm
- setup
    - const editable = ref{{}}
- v-model = "editable.title"
- v-model = "editable.coverImg"
- return
    - editable
    - async createAlbum
        - const newAlbum = await albumsService.createAlbum(formData)
        -try catch 
        - window.event.preventDefault()
- within form
    - @submit.prevent="createAlbum"
- async createAlbum
    - const formData = editable.value
    - await albumsService.createAlbum(formData)
    - editable.value={}

AlbumsService
- async createAlbum(formData)
    - const res = await api.post('api/albums', formData)
    - log res.data

Navbar
- create album button
    - v-if="user.id"
    (renders create album button only if someone is logged in)
- user: computed(()=> AppState.user)

CreateAlbumForm
- createAlbum
    - Modal.getOrCreateInstance...
    - editable.value = {}
    - router.push({name: 'AlbumDetails', params{...reference}})
- const router = useRouter()

AlbumsService
- createAlbum
    - return res.data

App.vue
-create new modal that opens when you click add picture
    - <Modal id="createPicture">

AlbumDetailsPage
- add button to add picture

create CreatePictureForm.vue (component)
- vt
- copy template
- add @submit.prevent="createPicture()"
- setup
    - const editable = ref{{}}
    - const route = useRoute()
    - async createPicture
        - try catch
        - const formData = editable.value
        - formData.albumId = route.params.id
        - await picturesService.createPicture(formData)

PicturesService
- async createPicture
    - const res = await api.post('api/albumId/pictures', formData)
    - Modal.getOrCreateInstance(...)
    editable.value{}
    
    - AppState.pictures.push(new Picture(res.data))?

<!-- 6/6 continued (backend) -->

working on adding collaborators
create Collaborator.js model
- export const CollaboratorSchema = new Schema()
    - albumId 
    - accountId
    (reference)
    - timestamps
- CollaboratorSchema.virtual('album',)
    - localField
    - foreignField
    - ref
    - justOne
- CollaboratorSchema.virtual('profile')
    - localField
    - foreignField
    - ref
    - justOne

dbContext
- Collaborator = mongoose.model...

Create CollaboratorController
- export class CollaboratorController extends BaseController
    - constructor
        - super ('api/collaborators')
        - this.router
        -.use(Auth0Provider...)
            - .post(''. this.createCollaboration)
- async createCollaboration(req,res,next)
    - try catch
    req.body.accountId = req.userInfo.id
    - reference

create COllaboratorsService
- class CollaboratorsService
- export const collaboratorsService = ...
- async createCollaboration(collabData)
    - reference
    - const collab = await dbContext.Collaborators.create(collabData)
    - await collab.populate('profile album')
    - return collab

AlbumsController
- .get('/:albumId/collaborators', this.findAlbumCollaborators)
- async findAlbumCollaborators(req,res,next)
    - try catch
    - reference
    - const collabs = await collaboratorsService.findAlbumCollaborators(req.params.albumId)
    - reference

CollaboratorsService
- findAlbumCollaborators(albumId)
    - const collabs = await dbContext.Collaborators.find({albumId: albumId}).populate('profile')
    - reference

AccountController
- .get('/collaborators', this.getAccountCollaborations)
- async getAccountCollaborations(req,res,next)
    - reference

CollaboratorsService
- getAccountCollaborations(accountId)
    - reference
- async getCollabs()
    -reference
refactored here and changed to getCollabs in service and controller
AccountController
- change to await collaboratorService.getCollabs...

CollaboratorsController
- .delete('/:collabId', this.removeCollaborator)
- async removeCollaborator(req,res,next)

CollaboratorsService
- removeCollaborator
    - reference

Album
- AlbumSchema.virtual('memberCount')
    - localField
    - foreignField
    - justOne
    - ref
    (reference)

AlbumsService
- change


getting collaborator count
CollaboratorsService
- async getAccountCollaborations
    - add another .path({'album', populate: })
    - ^^ reference

<!-- 6/6 continued (front end) -->

HomePage
- setup
    - const filterBy = ref('')
- return
    - filterBy
- in category buttons
    - add @click ="filterBy='animals, etc... reference'"
- albums: computed(()=>) (changing existing computed)
    - if(filterBy.value == '')
    return AppState.albums
    - else return AppState.albums.filter(a => a.category == filterBy.value)

AccountCollaborators
- .get('/collaborators', this.getAlbumCollaborators)

CollaboratorsService
- class CollaboratorsService
    - async getMyCollabs
        - const res = await api.get('account/collaborators')

- export const collaboratorsService = ..

AuthService
- AuthService.on...
    - await collaboratorService.getMyCollabs()

AppState
- myCollabs:[]

create Collab.js
- export class Collab
    - constructor
    - data
    - reference
    - this.profile comes from the Collaborator.js on the backend, 'profile in the CollaboratorSchema.virtual'

CollaboratorService 
- getMyCollabs
    - AppState.myCollabs...

creating a collab

AlbumDetailsPage
- async function getCollabsByAlbumId()
    - reference

CollaboratorsService
- async getCollabsByAlbumId(albumId)
    - reference

AppState
- collabs:[]

AlbumDetailsPage
- collabs: computed...

Collab
- export class AlbumMember extends Collab
    - reference
- export class CollabedAlbum extends Collab
    - reference

AlbumDetailsPage
- add v-for = c in collabs
- missed some stuff here
- async createCollab
- also add @click create collab to heart

CollaboratorsService
- async createCollab(albumId)
    - reference

AlbumDetailsPage
- return
    - isCollaborator: computed(()=> AppState.collabs.find(c=> c.accountId == AppState.user.id))
- add v-if = !isCollaborator
- v-else
    - @click removeCollab
- async removeCollab(albumId)
    - reference

CollaboratorsService
- async removeCollab(collab.id)

AlbumDetailsPage
- add v-if="album.creatorId == user.id" to archive album button
- user: computed...
archiving the album..
- @click archiveAlbum
- async archiveAlbum
    - reference

AlbumsService
- async archiveAlbum
    - reference

HomePage
- myCollabs: computed...
- add <AlbumCard :album="a"/>
