# Node
<!-- 5/22 lecture (backend mvc) (node express) -->

npm i -g bcw
bcw create

(in package.json)
- express is a library
- mongoose is how you talk to the database

(in terminal)
- in directory where you have package.json
    - run npm i 
    - will create a node modules folder for you

to hide node modules
- in settings
    - ignore / add item node_modules

every controller must extend to BaseController

gets/deletes dont have body
post and puts have body



<!-- 5/23 lecture ((GregslistNode) building gregslist server) -->

update bcw tool 
express-mvc

before opening workspace, initialize/publish code

in server folder 
- locate env and copy paste info from notepad into env
- make port = 3000
copy all info from env and paste into notepad(for future use)
in client folder
- paste same info into envs file
when running bcw command you must be in client file

what is my data going to look like?
- create a model (Car.js)
    - import mongoose 
    - const schema = mongoose.schema 
    - export const CarSchema = new Schema 
    - model:{type: string, ... (reference)}
    - include timestamps: true (reference)
after creating model you need to register the models to the database
- in db, go to dbContext
    - (reference) add Cars = mongoose.model('Car', CarSchema)
create CarsController
- export class CarsController extends BaseController
    - constructor
    - super('api/cars')
    - this.router
        - .get('', this.getCars)
    - add async getCars(req, res, next)
        - try catch / return res.send() /next(error) (reference)
        - const cars = await carsService.getCars()
create CarsService
- class CarsService..
    - export const.. 
- export const carsService = ..
- create async getCars
    - const cars = await dbContext.Cars.find()
    - return cars
in postman
- create new collection (Gregslist)
    - create (Cars) folder inside of gregslist collection
- (test get request)
- ask about creating a baseURL
CarsController
(middleware) (getting a post to only work if they're authenticated)
- in constructor
    - add use.(Auth0Provider.getAuthorizedUserInfo) (.use grabs bearer token for authenticating user)
    - .post('', this.createCar) (because it is below the .use, user must first be authenticated)
- add async createCar(req,res,next)
    -(reference)
    - const carData = req.body
    -const newCar = await carsService.createCar(carData)
    - res.send(newCar)
CarsService
- async createCar(carData)
    - (reference)
CarsController
- add .get('/:carId', this.getCarById)
- async getCarById(req,res,next)
    - (reference)
CarsService
- create getCarById(carId)
CarsController
- .put (reference)
- add async editCar(req,res,next)
    -(reference)
CarsService
- create async editCar
CarsController
- create async deleteCar(req,res,next)
- also add .delete...


<!-- 5/24 Lecture ((Schoolyard) data relationships) -->

express-mvc
after opening up workspace
in terminal make user you are in your service (schoolyard/schoolyard)
bcw serve in client folder
copy stuff from notepad into .env file

create School model
- import mongoose...
- const Schema = mongoose...
- export const SchoolSchema = new Schema
    - add data( name{ type:...})
register model in database
DbContext
- Schools = mongoose.model(...)
Create SchoolsController
- export class SchoolsController extends BaseController
    - constructor 
    - super('api/schools')
    - this.router
    - .post('', this.createSchool)
- createSchool(req,res,next)
    - try catch
    - const schoolData = req.body
    - const new school = await...
    - return res.send(newSchool)
Create SchoolsService
- class SchoolsService
- export const schoolsService = new...
- async createSchool(schoolData)
    - const newSchool= await...
    - return newSchool
create new collection in postman
- set up base url
- new folder for schools
    - add create request and copy schema into it
SchoolsController
- add .get('', this.getSchool)
- getSchools
    - get requests support query
    - try catch
    - const query = req.query
    (reference)
STOPPED TAKING NOTES HERE

create new folder courses in postman
- add requests
create Course.js
- import mongoose
- const Schema - mongoose.Schema
- export const CourseSchema = new Schema
    - add data (reference schoolId in export const)
- CourseSchema.virtual(...reference)
    - local field
    - ref
    - foreignField
    - justOne
create CoursesController
- (initial info same as before)
create CoursesService
- (initial info same as before)
- in async getCourses
    - const courses = await dbContext.Courses.find(query).populate('schools')


go and get all courses by school id
- in postman
    - add get by school id request in courses folder
SchoolsController
- add .get ('/:schoolId/courses', this.get)
- add async getCoursesBySchoolId(req,res,next)
    - try catch
    - const schoolId = req.params.schoolId
(reference)

building out students(many to many relationship)
create CourseStudent.js
- (similar initial info / reference)
- reference data
- CourseStudentSchema.virtual(reference)
register model
- in dbCOntext
    - CourseStudents = ...
create CourseStudentsController
create CourseStudentsService

<!-- 5/25 Lecture - Full Stack example(Bird brain) -->
express mvc
go into workspace
push to github
create new repository
adding collaborator
- settings
- collabs and teams
- app people

~Front end~
