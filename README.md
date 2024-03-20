# Creating an API Step by Step with Laravel

## Introduction

This guide will walk you through the process of creating a REST API using Laravel, a popular PHP framework. Before diving in, make sure you are familiar with MVC frameworks. You can find more information [here](https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm).

## Prerequisites

Make sure you have the following set up:

- Laravel Installed
- Visual Studio Code or any IDE of your choice
- Postman

## Steps for Creating a REST API with Laravel

### 1. Creating Necessary Files
#### Step 1: Navigate to Your Laravel Folder

```bash
cd your_laravel_folder
```
#### Step 2: Create a Model	
	
  <h6>Before hopping to the code what is some shortcut in creating. This is the shortcuts mostly we will be using </h6>

  ```
  -m  = migration
  -f  = factory 
  -s  = seeder
  -c  = controller
  -a  = all at once
  ```

```
php artisan make:model NameofModel -mfs
```

  <p>After pressing Enter in that it will be  able to create a Model with the Migration, Factories, and Seeder Files of that model.</p>

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/980cbb6c-20fb-4a05-abdb-a439f940e5ab)


#### Step 3: Create a Controller

  ```
    php artisan make:controller NameofController --api --model=NameofModel
  ```

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/570d8506-477c-4984-bc76-275bd17454c2)

#### Step 4: Create a Resource and Collection

```
php artisan make:resource NameofResource
php artisan make:resource NameofCollection
```
![image](https://github.com/TianMeds/Laravel-API/assets/99672958/15171858-7127-4304-b896-8e77f5a48d9a)

### 2. Modifying Models, Controllers, and Resources

Controller

#### Step 5: Import Resources and Response

```
use App\Http\Resources\ProjectPartnerResource;
use App\Http\Resources\ProjectPartnerCollection;
use Illuminate\Http\Response;

```
or 

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

  #### Step 6: Remember to put the needed column in the Model

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

#### Step 7: Now we will be creating a parent array for our data, so we will put this code in the Collection we created

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
#### Step 8: Define Resource Data

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

#### Step 9: Here we create type the column that the database should have so this will be the column present in the database all of it 

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

  #### Step 10: Now lets hop on the seeders so we will be matching the data needed or the columns we put in our table. Below is the example of data im providing or inserting in the table
  
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

#### Step 11:  Now to create an API Endpoint for our Laravel is we need to modify the API.php in the routes folder of Laravel

<i>Things to Remember before modifying API.php</i>

<ol>
  <li><ul><li>Remember to import your controller at the top of api.php. In our case, we need to import ProjectPartnerController at the top.</li></ul></li>
  <li><ul><li>Then let's create our method functions for handling every type of data. This includes GET, PUT, POST, and DELETE.</li></ul></li>
</ol>



<i>So before we create the code in Routes lets dicuss first the Format Structure of Routing. For now we use GET Method to discuss Format Structure</i>

```
Route::apiResource('/project-partners', ProjectPartnerController::class)->only(['index', 'show']);

```
<ol>
  <li><b>Route:</b> This will be the word to use for routing or API, and it will be the default in the API.php file. This will be used in every method.</li>
  <li><b>apiResource:</b> This is used to specify the method we will be using. We can use get, put, post, and delete here.</li>
  <li><b>The "/project-partner":</b> This will be the endpoint we will be calling in Postman, and we can modify this to suit our needs.</li>
  <li><b>ProjectPartnerController:</b> This will be the controller file name and will serve as the basis for where our functions will be running. This is 	where we put our index, show, and update functions, based on your code.</li>
  <li><b>"index" and "show":</b> These can be modified by you, but in my case, I created my function names as index and show, basing them on the names of functions.</li>
</ol>


<p>So in my end i create my endpoints and function in this format </p>

```
Route::apiResource('/project-partners', ProjectPartnerController::class)->only(['index', 'show']);
Route::post('/project-partners', [ProjectPartnerController::class, 'store']);
Route::put('/project-partners/{id}', [ProjectPartnerController::class, 'update']);
Route::delete('/project-partners/{id}', [ProjectPartnerController::class, 'destroy']);
```

<ol>
  <li>GET method (apiResource): Used for retrieving or showing data.</li>
  <li>POST method: Stores new data in our table.</li>
  <li>PUT method: Updates existing data in our table, typically using an ID to specify which entry to modify.</li>
  <li>DELETE method: Removes data from our table.</li>
</ol>





<i>Now that we completed configuring the file lets go back to our terminal and folder of the backend </i>


  #### Step 12: Make sure to put the .env file first in the host ip address of

  ```
	127.0.0.1 or it depends to you 
```

  #### Step 13: Run this in your terminal

  ```
	php artisan migrate
```

<i>So, it will be looking like this </i>
![image](https://github.com/TianMeds/Laravel-API/assets/99672958/f65b5af2-6654-41fb-ab31-bd0321f27e27)

<i>In the Database your using you should see this now </i>

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/84ebbbce-8bd4-4680-9ebb-3777fda6e0aa)

  #### Step 14: Now we run the seeder of the table so we we need to import this first in the top of our seeder or else it will return an error of

```
Class "Database\Seeders\DB" not Found
```

#### Step 15: After that you can Run the seeder now and it will look like this

  ```
php artisan db:seed --class=ProjectPartnerSeeder
```


### 3. Testing With Postman

  #### Step 16: Then now access the API you created and make sure your server is working it should look like this and same with other method function

<p>It would look like this in POSTMAN when you do the GET method </p>

![image](https://github.com/TianMeds/Laravel-API/assets/99672958/daaaa915-ca8b-4ffb-a5a1-3543ff52ab5e)

<i>Note that if you will do POST you will be going to the Body and add the Columns needed and if PUT is you will use the ID and add it in the API Endpoint same with the Delete</i>

## Reminder
You can copy and paste this markdown code directly into your README.md file. Make sure to replace placeholders like `NameofModel`, `NameofController`, etc., with your actual names. Also, adjust image URLs to point to your actual image locations.

## Feedback
If you have any feedback, please reach out to me at tianmeds.business@gmail.com

## Author
- [@tianmeds](https://github.com/TianMeds)

## License
[MIT License](LICENSE)



