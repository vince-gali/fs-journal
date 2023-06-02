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