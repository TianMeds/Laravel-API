  <h1>Create an API  Step by Step via Laravel</h1>

</div>

  <h3>So, right now we’re going to talk about how to create an API in Laravel?</h3>

  <h4>Before starting we should be aware or have knowledge into MVC Frameworks for more information. <a href="https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm">Click here</a></h4>

  <h1>Make sure to have this already setup</h1>
  
  <ul>
    <li>Laravel Installed</li>
    <li>Visual Studio Code or any IDE you want</li>
    <li>Postman</li>
  </ul>

  <h1>Steps for Creating a REST API for Laravel</h1>
  <ul>

  <h2>Creating all the file we need for our API </h2>
  
  <li>
  <h4>Step 1: In your created Laravel Folder make sure to go in the folder path of it</h4>
  </li>

  <h6>Before hopping to the code what is some shortcut in creating. This is the shortcuts mostly we will be using </h6>

  ```
  -m  = migration
  -f  = factory 
  -s  = seeder
  -c  = controller
  -a  = all at once
  ```

<li>
  <h4>Step 2: So, first we will be creating a Model </h4>
  </li>

  ```
    php artisan make:model `NameofModel` -mfs
  ```

  <p>After pressing Enter in that it will be  able to create a Model with the Migration, Factories, and Seeder Files of that model.</p>
  ![image](https://github.com/TianMeds/Laravel-API/assets/99672958/9b56dc57-711d-4b6e-8362-93cd6c772e6a)

  <li>
  <h4>Step 3: Next is we create a Controller for our Models and API. It would look like this in your terminal</h4>
  </li>

  ```
    php artisan make:controller `NameofController` --api --model=`NameofModel` 
  ```

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/570d8506-477c-4984-bc76-275bd17454c2)

  <li>
  <h4>Step 4: Next is we create a Resource and Collection of Data for our API. It would look like this in your terminal</h4>
  </li>

  ```
    php artisan make:resource `NameofResource`
    php artisan make:resource `NameofCollection`
  ```

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/15171858-7127-4304-b896-8e77f5a48d9a)

<h2>Modifying the Models, Controller, and Resources</h2>

<h3>CONTROLLER </h3>

  <li>
    <h4>Step 5: Import all the Resource and Collection and also the Response in top of the Controller it should look like this</h4>
  </li>

  ![image](https://github.com/TianMeds/Laravel-API/assets/99672958/56b07f47-fa2c-4920-b449-66a792d7690c)

<h6>GET METHOD</h6>

<p>Index is the one getting the response or the data </p>

```
public function index()
    {
        return response()->json(new ProjectPartnerCollection
				(ProjectPartner::all()), Response::HTTP_OK);
    }
```
<p>Show is for displaying specified Resources or viewing some of the information </p>

```

public function show(ProjectPartner $projectPartner)
    {
        return new ProjectPartnerResource($projectPartner);
    }
```

<h6>POST METHOD</h6>

<p>Store Function  the data inside are what supposed to be inside your table </p>

```
  public function store(Request $request)
      {
          $projectPartner = ProjectPartner::create($request->only([
              'scholarship_categ_id',
              'project_partner_name',
              'project_partner_mobile_num',
              'school_id',
          ]));
  
          return new ProjectPartnerResource($projectPartner);
      }
```

<h6>PUT OR PATCH METHOD</h6>

<p>Updating the displayed data and editing the information in it </p>
<i>Remember here: I Call the $id its not default by Laravel</i>

```
public function update(Request $request, ProjectPartner $projectPartner, $id)
    {
        $projectPartner = ProjectPartner::find($id);
        $projectPartner->update($request->all());
        return new ProjectPartnerResource($projectPartner);
    }
```

<h6>DELETE METHOD</h6>

<p>Deleting the Resource or the Data in it also in here $id is not default</p>

```

public function destroy(ProjectPartner $projectPartner, $id)
    {
        $deleted = ProjectPartner::destroy($id);
        
        if($deleted === 0) {
            return response()->json(['message' => 'Project Partner not found'], 404);
        } elseif ($deleted === null) {
           return response()->json(['message' => 'Error deleting project partner'], 500);
        }

        return response()->json(['message' => 'Project Partner deleted'], 200);
    }
```

<p>Now were done in our Controller File now lets hop in our Model Part</p>

<h3>MODEL</h3>

<p>So, the Model will be the one handling the columns in your Table or in the Database</p>

  <li>
    <h4>Step 6: Remember to put the needed column in the Model</h4>
  </li>

```

<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class ProjectPartner extends Model
{
    use HasFactory;

    protected $fillable = [
        'scholarship_categ_id',
        'project_partner_name',
        'project_partner_mobile_num',
        'school_id',
    ];
}
```

<p>Now were done in our Model File now lets hop in our Resources and Collection Part</p>

<h3>RESOURCE & COLLECTION</h3>

  <li>
    <h4>Step 7: Now we will be creating a parent array for our data, so we will put this code in the Collection we created </h4>
  </li>

  ```
<?php

namespace App\Http\Resources;

use Illuminate\Http\Request;
use Illuminate\Http\Resources\Json\ResourceCollection;

class ProjectPartnerCollection extends ResourceCollection
{
    /**
     * Transform the resource collection into an array.
     *
     * @return array<int|string, mixed>
     */
    public function toArray(Request $request): array
    {
        return [
            'data' => $this->collection,
        ];
    }
}
```
  <li>
    <h4>Step 8: Now we will be creating the needed data or our expected data to be returned when we do a request. This will be the file for Resource </h4>
  </li>

```
    public function toArray(Request $request): array
    {
        return [
            'id' => $this->id,
            'scholarship_categ_id' => $this->scholarship_categ_id,
            'project_partner_name' => $this->project_partner_name,
            'project_partner_mobile_num' => $this->project_partner_mobile_num,
            'created_at' => $this->created_at,
            'updated_at' => $this->updated_at,
            'deleted_at' => $this->deleted_at,
            'school_id' => $this->school_id,
        ];
    }
```

<p>Now were done in our Resources and Collection File now lets hop in our Migration Files Part</p>

<h3>MIGRATION</h3>

  <li>
    <h4>Step 9: Here we create type the column that the database should have so this will be the column present in the database all of it  </h4>
  </li>

```
public function up(): void
    {
        Schema::create('project_partners', function (Blueprint $table) {
            $table->id();
            $table->unsignedBigInteger('scholarship_categ_id');
            $table->string('project_partner_name');
            $table->string('project_partner_mobile_num');
            $table->timestamps();
        });
    }

```
  
<p>Now were done in our Migration File now lets hop in our Seeder Files Part</p>

<h3>SEEDERS</h3>

  <li>
    <h4>Step 9: Now lets hop on the seeders so we will be matching the data needed or the columns we put in our table. Below is the example of data im providing or inserting in the table </h4>
  </li>
  
```
DB::table('project_partners')->insert([
    [
        'project_partner_name' => 'BACPAT Youth Development Foundation Inc.',
        'project_partner_mobile_Num' => '09011223344'
    ],
    [
        'project_partner_name' => 'Welcome Home Foundation Inc. (BACPAT Scholars)',
        'project_partner_mobile_Num' => '09055667788'
    ],
    [
        'project_partner_name' => 'Education for Former OSY',
        'project_partner_mobile_Num' => '09098765432'
    ],
    [
        'project_partner_name' => 'Bahay Maria Children Center Formal',
        'project_partner_mobile_Num' => '09011112222'
    ],
    [
        'project_partner_name' => 'Canossian Sisters – Endorsed Grantees c/o Sr. Elizabeth Tolentino',
        'project_partner_mobile_Num' => '09033334444'
    ],
    [
        'project_partner_name' => 'Canossian Sisters – Endorsed Grantees c/o Sr. Elizabeth Tolentino & Sr. Mila Reyes',
        'project_partner_mobile_Num' => '09055556666'
    ],
    // ... (add other partner details in the same format)
]);

```

  <li>
    <h4>Step 10: Now to create an API Endpoint for our Laravel is we need to modify the API.php in the routes folder of Laravel	</h4>
  </li>


<i>Things to Remember before modifying API.php</i>

<li>
	<ul>Remember to import your controller in the top of api.php first, so in our end is we need to import ProjectPartnerController in top of it</ul>
	<ul>Then lets create now our method function for every data this consist the GET, PUT, POST, DELETE</ul>
</li>

<i>So before we create the code in Routes lets dicuss first the Format Structure of Routing. For now we use GET Method to discuss Format Structure</i>

```
Route::apiResource('/project-partners', ProjectPartnerController::class)->only(['index', 'show']);

```

<li>
	<ul><b>Route</b> will be the word to use for routing or API this will be the default in the API.php and this will be the use in every methods</ul>
	<ul><b>apiResource</b>so this will be one using for knowing what method we will be using we can use also get,put,post, and delete here</ul>
	<ul><b>The "/project-partner" </b>will be the endpoint we will be calling in Postman we can modify this on what we want</ul>
	<ul><b>ProjectPartnerController</b>this one will be the controller file name and this will be the basis where is our function will be running were we put our index, show, update functions and based on your code.</ul>
	<ul><b>The "index" and "show"</b> This can be modify by you but in my case i create my function name to index and show so basing on the name of functions</ul>
</li>


<p>So in my end i create my endpoints and function in this format </p>

```
Route::apiResource('/project-partners', ProjectPartnerController::class)->only(['index', 'show']);
Route::post('/project-partners', [ProjectPartnerController::class, 'store']);
Route::put('/project-partners/{id}', [ProjectPartnerController::class, 'update']);
Route::delete('/project-partners/{id}', [ProjectPartnerController::class, 'destroy']);
```

<li>
	<ul>GET method (apiResource): Used for retrieving or showing data.</ul>
	<ul>POST method: Stores new data in our table.</ul>
	<ul>PUT method: Updates existing data in our table, typically using an ID to specify which entry to modify.</ul>
	<ul>DELETE method: Removes data from our table.</ul>
</li>




<i>Now that we completed configuring the file lets go back to our terminal and folder of the backend </i>


  <li>
    <h4>Step 11: Make sure to put the .env file first in the host ip address of	</h4>
  </li>

  ```
	127.0.0.1 or it depends to you 
```

  <li>
    <h4>Step 12: Run this in your terminal </h4>
  </li>

  ```
	php artisan migrate
```

<i>So, it will be looking like this </i>
![image](https://github.com/TianMeds/Laravel-API/assets/99672958/f65b5af2-6654-41fb-ab31-bd0321f27e27)

<i>In the Database your using you should see this now </i>

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/84ebbbce-8bd4-4680-9ebb-3777fda6e0aa)


  <li>
    <h4>Step 12: Now we run the seeder of the table so we we need to import this first in the top of our seeder or else it will return an error of  </h4>
  </li>

```
Class "Database\Seeders\DB" not Found
```

  <li>
    <h4>Step 13: After that you can Run the seeder now and it will look like this  </h4>
  </li>

  ```
php artisan db:seed --class=ProjectPartnerSeeder
```


<h2>POSTMAN TESTING THE CODE</h2>

  <li>
    <h4>Step 14: Then now access the API you created and make sure your server is working it should look like this and same with other method function   	</h4>
  </li>

<p>It would look like this in POSTMAN when you do the GET method </p>

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/daaaa915-ca8b-4ffb-a5a1-3543ff52ab5e)

<i>Note that if you will do POST you will be going to the Body and add the Columns needed and if PUT is you will use the ID and add it in the API Endpoint same with the Delete</i>


