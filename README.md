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



