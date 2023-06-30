# CSharp

6/26/23 intro to c sharp (monday week 10)

data types have to be declared
(examples) 
int x = 6;
float y = 1.2354f;
double dbl = 12.343d;
// decimal is the largest datatype, so it is used for 'm'oney
decimal dcl = 23.434343m;
string cool = "cool stuff" (always use double quotes for strings);
char letter = 'c' (use single quotes for single characters);
string cooler $"now you can say {cool} with a {letter}";
bool tru = true;
bool no = false;

bool? nothing = null;
int? nada = null;
string zero = null

no truthy falsey in c sharp

if(nothing == null){
    Console.WriteLine(nothing)
}

if(x ==6){
    Console.WriteLine("no truthy falsey")
}

Building an object (Dictionary)vvv
Dictionary<string, string> dict = new Dictionary <string, string>{};
dict.Add("one", "another one");
dict.Add("two", "another one");
dict.Remove("two");
(define datatypes inside <> (data teeth))

class Cat{
    int age;
    string name;
    public Cat(int age, string name){  (public Cat is like a constructor)
        this.name= name;
        this.age = age;
    }

    void speak(){
    }
}

Console.WriteLine (x +y);
Console.WriteLine (dbl);
Console.WriteLine (dcl);
Console.WriteLine (cooler);

Arrays...
Array nums = new Array[4]; // arrays have fixed lengths that cannot be increased or decreased so they are hard to work with

Lists...
    List<int> numbers = new List<int>{0, 9, 8, 7, 6};
    numbers.Add(1);
    numbers.Add(2);
    numbers.Add(3);
    numbers.Remove(3);
    List lowNumbers = numbers.FindAll(n => n <=3)
    int five = numbers.Find(n => n == 5)

    numbers.ForEach(n => {
        Console.WriteLine(n)
    })

    for( int i = 0; i < numbers.Count; i++)
        {
            Console.WriteLIne($"i:{i} n:{numbers[i]}")
        }




~~~Building an api... (catRoundUp)~~~
    bcw create
    dotnet-auth
    (don't name a file with a hyphen)
    dotnet restore is sim to npm i (bcw create tool already implements dotnet so no need to use it unless cloning down on a diff pc)

appSettings.Development == .env
probably wont touch program.cs

in Startup.cs comment out 'ConfigureAuth(services)'

close and re open
- prompt should pop up and click yes or in notifications

'public' defines the accessibility of those properties (anyone can get it or set it)

Account.cs
- get allows something to be seen
- set allows something to be changed
- public makes it so that anyone can see or change
- private does the opposite

AccountController.cs
- 
AccountService.cs
-

create Cat.cs
- define classes inside of namespaces in order to import/export
    - nameSpace catRoundUpModels;
    - public class Cat{
        public string Name{get;set;}
        public int Age {get;set;}
        public string Color{get;set;}
        public bool LongHair {get;set;}
        public bool Caught {get;set;}
        public Penned {get;set;}
        // NOTE for today we need a constructor defined
        ctrl . on class name to generate constructor (reference line 7)
        }

create CatsController.cs
- define namespace
    - nameSpace catRoundUp.Controllers;
- [ApiController]
- [Route("api/cats")]
- public class CatsController : ControllerBase (':' is similar to 'extends')
    - [HttpGet("test")]
    - public ActionResult<string> Test(){
        return Ok("Hey");
    - refactored above in a try catch (reference)
    }
    (when running it you may have to click advanced in the browser and continue to local host)
    - [HttpGet]
    - public ActionResult<List<Cat>> GetAllCats(){
        - (reference try catch)
        - try   
            - List<Cat> cats =   (go to my service and get cats)
            - return Ok(cats);
    }

Global.cs handles all imports for you

create CatsService.cs
- public class CatsService
    - public List<Cat> GetAllCats(){}
        - List<Cat> cats = // go to repo and get cats
        - return cats;

CatsController
- public class CatsController : ControllerBase
    - private readonly CatsService catsService;
    - crtl . on CatsController to define constructor
- public ActionResult..
    - List<Cat>> cats = catsService GetAllCats (reference)

CatsService.cs
-public class
    - private readonly CatRepository _repo;
- 

create CatsRepository.cs
- public class
    - internal List<Cat> GetAllCats(){}
    - (fake database) private List<Cat> dbCats = new List<Cat>;
    - public CatRepository(){}
        - this.dbCats = new List<Cat>{};
        - dbCats.Add(new Cat("Murdoc", 31, "black", true, false));

CatRepository
- internal
    - return dbCats (ref)

CatsController
- public CatsController(CatsService catsService){}
    - _catsService = catsService;

Startup.cs
- in public void
    - add services.AddSingleton<CatsRepository>();
    - add services.AddTransient<CatsService>();
    (repository needs to go before service)

CatsController
- public class
    - [HttpPost]
    - public ActionResult<Cat> CreateCat([FromBody]Cat catData){}
        - try 
            - Cat cat = _catsService.CreateCat(catData);
            - return Ok(cat);
        - catch 
            - return BadRequest(e.message)

CatsService
- internal Cat CreateCat( Cat catData)
    - reference

CatsRepository
- internal Cat CreateCat( Cat catData)
    - dbCats.Add(catData)

(deleting cat)
Cat.cs
- add public in Id (get; set;)
- crtl . on Id and add to parameters

CatRepository
- internal Cat CreateCat...
    - int lastId = dbCats[dbCats.Count -1].Id;
    - catData.Id = lastId + 1
    // above probably only used for today

CatsController
- [HttpDelete("{catId}")]
- public ActionResult <string> RemoveCat(int catId){}
    - try
        - string message = _catsService.RemoveCat(catId)
        - return Ok(message)
    - catch

CatsService
- internal string RemoveCat(int catId)
    - Cat cat = _repo.GetById(catId);
    - if (cat == null) throw new Exception($"no cat at id {catId}")
    - repo.RemoveCat(catId);
    - return $"{cat.name} was deleted";

CatRepo
- internal Cat GetById (int catId)
    - reference

editing a cat
CatsController
- [HttpPut("{catId}")]
- public ActionResult <Cat> UpdateCat( int catId, [FromBody]Cat updateData){}
    - try
        - updateData.Id = catId
        -Cat cat = _catsService.UpdateCat(updateData);
        - return Ok(cat);
    -catch
        - return BadRequest(e.message)

CatsService
- internal Cat UpdateCat( Cat updateData)
    - Cat original = _repo.GetById(updateData.id);
    - if( original == null) throw new Exception...;
    - original.Age = updateData.Age != null ? updateData.Age : original.Age;
    - original.Name = updateData.Name != null ? updateData.Name : original.Name;
    - _repo.UpdateCat(updateData)
    - return updateData;

Cat.js
- add elvis operator to age and penned (reference) also add to line 7
    - need this in order to edit integers and booleans

CatsRepo
- internal void UpdateCat( Cat updateData)
    - Cat cat = GetById (updateData.Id);



    <!-- 6/27 c# day 2 lecture SharkList (SQL) -->

dbSetup.sql
- creating tables that store info (schema)
- rows will be entries in those tables
    - create Table penguins(
        - id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        - name TEXT NOT NULL, 
        - age INT DEFAULT 1, 
        - species);
(run this code against database ("execute" button above the table))

in database tab
open penguins table

dbSetup
- INSERT INTO penguins
    - (name, age, species)
    - VALUES
    - ("Penny", 2, "Macaroni");
click execute
- INSERT INTO penguins
    - (name, species)
    - VALUES
    - ("Rocky", "Southern rock hopper");
click execute

(when you want to add additional values to the table) for now => right click on penguins table and drop then re insert penguins list
in create table
- add wearingTuxedo BOOLEAN DEFAULT TRUE

dbSetup (revising table)
- create Table penguins(
    - id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    - name VARCHAR(225) NOT NULL, 
    - age INT DEFAULT 1, 
    - species CARCHAR(5000));
    - wearingTuxedo BOOLEAN DEFAULT true
(drop table and re insert penguins)
- select * from penguins limit 100
    - "*" means select ALL info from penguins
(you can also do)
    - select name, species from penguins;
OR
    - select name, species from penguins where id=1;
    - select id, name, species, wearingTuxedo from penguins where wearingTuxedo = true;
    - select name from penguins where name LIKE "S$";
        - brings up names that have s

how can i take info from database and update it
- UPDATE penguins SET
    - wearingTuxedo = true
    - where id = 4

reference delete from ..

making cars for gregslist...
create table cars
- reference

insert into cars
- reference
- select * from cars;
(- select * from cars ORDER BY "createdAt" DESC;)

reference mysqltutorial for documentation



in scaleGrid (mysql)
go to databases tab and create database
in overview tab
- reference master and credentials

in database extension
- click plus sign (window should pop up and )
- host should be master connection endpoint
- username to be sgroot (copy from mysql scaleGrid)
- whatever you named your database in scaleGrid, must be same
save
this connects vscode to database

getting all this stuff into server
appsettings.Development.json (makes project connect to database)
- update connection string (reference) and auth domain

create Car.cs
- public class car
    - reference

create CarsController
- reference

crete CarsService
- right click services file new class and name it
- private readonly...
- add constructor

create CarsController
- private readonly ...
- generate constructor

startup.cs
- add repo
- add services
- (reference)

create CarsRepo
- private readonly IDbConnection _db (this already exists in startup.cs)
- generate constructor for this

CarsController
- [HttpGet]
- public ActionResult <List<Car>> GetAllCars(){}
- try 
    -List<Car> cars = _carsService.GetAllCars();
    - return Ok(cars);
- catch
    - return bad request

CarsService
- public List<Car> GetAllCars
- reference

CarsRepo
- internal List<Car> GetAllCars()
    - string sql = "SELECT * FROM cars;";
    - List<Car> cars = _db.Query<Car>(sql).ToList;
    - return cars;

CarsController
-[HttpGet("{carId}")]
- public ActionResult<Car> GetById (int carId)
- reference

CarsService
- internal Car GetById (int carId)
- reference
- Car car = _repo.GetByID(carId)
- reference

CarsRepo
- internal Car GetById(int carId)
- string sql = $"SELECT * FROM cars WHERE id = @carId;"; <--- sad spider(;";)
- Car car = _db.Query<Car>(sql, new {carId}).FirstOrDefault();
- return car;

comment out ConfigureAuth(services) in startup?
testing in postman

post request
CarsController
- [HttpPost]
- public ActionResult<Car> CreateCar([FromBody] Car carData)
    -reference

CarsService
- internal Car CreateCar(Car carData)
    - reference

CarsRepo
- internal Car CreateCar(Car carData)
    - string sql = @"INSERT INTO cars (make,model,year, price,color,description)
        VALUES (@Make,@Model,@Year,@Price,@Color,@Description)";
    - _db.Execute(sql, carData);

dbSetup
- Select * from cars where id = last...

CarsRepo
- internal Car CreateCar(Car carData)
    - string sql = @"INSERT INTO cars (make,model,year, price,color,description)
        VALUES (@Make,@Model,@Year,@Price,@Color,@Description)";
        - Car car = _db.Query... reference
    - return car;

adding in created at and updated at in Car.cs (reference)

deleting a car
CarsController
- [HttpDelete("/{id}")]
    - public ActionResult<string> RemoveCar(int carId)
        - string message = _carsService.RemoveCar(carId)
        - return Ok(message);

CarsService
- internal  string RemoveCar(int carId)
    - Car car = GetById(carId);
    - int rows = _repo.RemoveCar(carId);
    - return $"she gone"
    
CarsREpo
- internal int RemoveCar(int carId)
    - string sql = "DELETE From cars where id = @carId limit 1;";
    - int rows = _db.Execute(sql, new {carId});
    - return rows;

CarsController
- [HttpPut ("{carId}")]
- reference

CarsService
- internal car UpdateCar(Car updateData)
- reference
(year and price and int's so they are non nullable. need to go into model and add elvis operator)
- reference

CarsRepo
- internal void UpdateCar(Car updateData)
- string sql = @"" (reference)
- _db.Execute(sql, updateData)
- Car car = GetById(updateData.Id);
- car = updateData;


<!-- 6/28 Post it backend, joining tables, enabling authorization lecture (sql, c#) -->

bcw create dotnet-vue

appSettings.dev
- connection string auth domain and audience
env.js
- fill in
spin both ends
when console shows that a table doesn't exist, go to dbSetup and execute template table (localhost 8080)

dbSetup
- create table 
- reference info
- reference creatorId
    - varchar (# must be same as if varchar (#) in accounts table at the very top)
- foreign key constraint (ties two tables together)
    - FOREIGN KEY: (creatorId) REFERENCES accounts(id) ON DELETE CASCADE (this makes it so that when you delete your account, it deletes all content within the albums the account created)(this is not similar to a virtual schema)
- insert album
    - when adding id to creatorId, go to accounts table and grab id from there
- SELECT title, name FROM albums JOIN accounts ON albums.creatorId = accounts.id;
trying to find the album that a certain account made
- SELECT * FROM albums join accounts ON albums.creatorId = accounts.Id WHERE accounts.id = 'account you want to find'; 

create Model, Controller, Service, Repo

Album.cs
- public class Album
- reference

create AlbumRepo
create AlbumsService
- generate constructor
AlbumController

add services to startup
- repo before service

AlbumsController
- private readonly Auth0Provider _auth;
- _auth = auth;
- httpPost
- [Authorize]
    - add async
        - public async Task<ActionResult<Album>> CreateAlbum ([FromBody] Album albumData)
    - reference 
    -  Account userInfo = await _auth.GetUserInfoAsync<Account>(HttpContext);
        (must be logged in/ figure out who you are and put your id on an object)
    - albumData.CreatorId = userInfo.id;

AlbumsService
- internal Album CreateAlbum(Album albumData)
    - Album album - _repo.CreateAlbum...

AlbumRepo
- internal Album..
    - string sql = @" INSERT INTO albums (title, category, coverImg, creatorId, archived) VALUES (@title, @category, @coverImg, @creatorId...) SELECT * FROM albums alb JOIN accounts creator ON alb.creatorId...."
    - reference

AlbumsController
- HttpGet
-public ActionResult<List<Album>> GetAllAlbums
- reference

AlbumsService
- internal List<Album> GetAllAlbums
- reference

AlbumsRepo
- internal List<Album> GetAllAlbums()
- string sql = @"SELECT  
                alb.* 
                creator.*
                FROM albums alb(alias) 
                JOIN accounts creator ON alb.creatorId = creator.id"
            - List<Album> albums = _db.Query<ALbum, Account, Album>(sql, (album, creator)=> )
                - reference here

deleting
AlbumsController
-HttpDelete
-reference

AlbumsService
- internal Album GetById( int albumId)
- reference

AlbumsRepo
- reference

archive an album
AlbumsController
- [HttpDelete("{albumId}")]
-[Authorize]
- public async Task<ActionResult<string>> ArchiveAlbum(int album)
    - reference

ALbumsService
- internal Album
- reference

AlbumsRepo
- reference



<!-- 6/29 postItSharp continued day two --> 

dbSetup
- check active connection (shown at bottom/click activeConnection)
- create table if NOT exists pictures
    - reference
    - albumId int ...
    - foreign key (creatorId) references accounts(id) on delete cascade
    - foreign key (albumId) references albums(id) on delete cascade
    (this table is a one to many relationship)

create picture.cs
- public class picture
    - reference properties

create PicturesRepo
- namespace
- public class PicturesRepo
    - private readonly IDbConnection _db;
        - crtl . on _db to generate constructor

create PicturesService
- namespace
- public class PicturesService
    - private readonly PicturesRepo _repo
        - generate constructor 

startup.cs
- add repo and service 

create PicturesController
- namespace
- [ApiController]
-[Route("api/[controller]")]
- public class PicturesController
    - private readonly PicturesService _picturesService;
        - generate constructor (public PicturesController(PicturesService picturesService)...)
    - private readonly Auth0Provider;
        - generate constructor
-httpPost
    - [Authorize]
    - public async Task<ActionResult<Picture>> CreatePicture([FromBody] Picture pictureData)
        - try
            -Account userInfo = await _auth0.GetUserInfoAsync<Account>(HttpContext);
            - pictureData.CreatorId = userInfo.Id;
            - Picture newPicture = _picturesService.CreatePicture(pictureData);
            - return Ok(newPicture);
        - catch (Exception e)
            - return BadRequest...

PicturesService
- internal Picture CreatePicture(Picture pictureData)
    - Picture newPicture = _repo.CreatePicture(pictureData)
    - return newPicture;

PicturesRepo
- internal Picture CreatePicture (Picture pictureData)
    - string sql = @"
    INSERT INTO pictures 
    (imgUrl, creator_id, albumId)
    VALUES
    (@imgUrl, @creatorId, @albumId)

    SELECT
    pic. *,
    act.*
    FROM pictures pic
    JOIN accounts act ON act.id = pic.creatorId
    WHERE pic.id = LAST_INSERT_ID();
    ;" 
    - Picture newPicture = _db.Query<Picture, Account, Picture>(sql, (picture, creator)=> {
        picture.Creator = creator;
        return picture;
    }, pictureData).FirstOrDefault();
    return newPicture;

spin up server to test in postman (7045)
spin up client to get bearer token(8080)

// get request (getting pictures by album id)
AlbumsController
-private readonly PicturesService _picturesService
    - add to constructor
    (need this in order to direct get request below to picturesService)

- [HttpGet("{albumId}/pictures")]
- public ActionResult<List<Picture>> GetPicturesByAlbumId( int albumId)
    - try
        - List<Picture> pictures = _picturesService.GetPicturesByAlbumId(albumId);
        - return Ok(pictures);
    - catch (Exception e)
        - return BadRequest...

PicturesService 
- internal List<Picture> GetPicturesByAlbumId(int albumId)
    - List<Picture> pictures = _repo.GetPicturesByAlbumId(albumId)
    - return pictures;

PicturesRepo
- internal List<Picture> GetPicturesByAlbumId(int albumId)
    - string sql = @"
    SELECT 
    pic.*,
    act.*
    FROM pictures pic
    JOIN accounts act ON act.id = pic.creatorId
    WHERE pic.albumId = @albumId;
    ";
    - List<Picture> albumPictures = _db.Query<Picture, Account, Picture>(sql, (picture, account(account is a banana word))=> {
        picture.Creator = account;
        return picture;
    }, new {albumId}).ToList();
    - return albumPictures;

deleting picture
PicturesController
- [HttpDelete("{pictureId}")]
- [Authorize]
    - check if they are authorized to delete
    - public async Task<ActionResult<Picture>> DeletePicture (int pictureId)
        -reference

PicturesService
- internal void DeletePicture(int pictureId, string userId)
    - Picture picture = GetById(pictureId)
        (create in GetById / reference)
    - if(picture.CreatorId != userId) throw new exception...
    - _repo.DeletePicture(pictureId);

PicturesRepo
- internal int DeletePicture(int pictureId)
    - string sql = @"
    DELETE FROM pictures
    WHERE id = @pictureId
    LIMIT 1;
    ";
    - int rows = _db.Execute(sql, new {picture});
    - return rows;

PicturesService
- int rows = _repo.DeletePicture(pictureId);
- if (rows>1) new Exception("....")

many to many relationships
dbSetup
- CREATE TABLE IF NOT EXISTS collaborators
    - reference
    - FOREIGN KEY (albumId) REFERENCES albums(id) ON DELETE CASCADE,
    - FOREIGN KEY (accountId) REFERENCE accounts(id) ON DELETE CASCADE
- Reference insert and select/join for collaborators

create Collaborator.cs
- public class COllaborator
    - reference

Account.cs
- public class CollaboratorAccount : Account (view model)
    - public int CollaborationId {get; set;}

ALbum.cs
- public class CollaboratorAlbum : Album
    - public int CollaborationId {get; set;}

Create CollaboratorsRepo
- namespace, public class, constructor

create CollaboratorsService
- namespace, public class, constructor

create CollaboratorsController
- reference
- HttpPost
- Authorize
- public async Task<ActionResult<Collaborator>> CreateCollab ([FromBody]Collaborator collabData)
    - reference

CollabService
- internal Collaborator CreateCollab...
    - reference

CollabRepo
- reference