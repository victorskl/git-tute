# Azure Backend

## Setting up the backend

Somehow the generated code does not apply the migration file

- In folder `App_Start` change the class file `StartUp.MobileApp.cs` with the following codes

```
mobileConfig
        .AddTablesWithEntityFramework()
        .ApplyTo(httpConfig);

    // Automatic Code First Migrations
    var migrator = new DbMigrator(new Migrations.Configuration());
    migrator.Update();

```

And comment out `Database.SetInitializer` line

In `Migrations\Configuration.cs` add in the following lines inside `Public Configuration()` method

```
SetSqlGenerator("System.Data.SqlClient", new EntityTableSqlGenerator());
```

## Migration

For every new Table created, need to follow the following steps:

1. Create new class in `DataObjects` folder, you could follow the template code `TodoItem.cs`
2. Create new controller class in `Controllers` folder (you could right click the `Controllers` folder to ask Visual Studio to generate controller class code for you but it does not work in my case), otherwise you need to follow the template code `TodoItemController.cs`. Make sure you change the names in the template code correctly to match your new table name.
3. In `Models` folder you need to modified the context class (the name of the context class file is dependent on your app service name, for example my one is called `bingfengappserviceContext.cs`)
	- Inside the file you should added something like `public Dbset<TeamMember> TeamMembers {get; set;}` if your new Table is called `TeamMembe`
4. Go to `Tools -> Nuget Package Manager -> Package Manager Console` and type the following command in the console
	- `enable-migrations`, note that you only need to do this once to create 'Migrations' folder
	- `add-migration` command will make a new migration files for you to create table in the azure database if you work through the steps 1 to 3 correctly
	- `update-database` is not used in our case, since we do not have a local sql database. However, one tricky thing is that, if we dont run this `update-database` we can not run `add-migration` next time it will show the warning `Unable to generate expilcit migration`, so you need to delete the last generated migration class manually.

## Error Handling

- Enable error message on the website
	- Click `Web.config` on the solution explorer (on the right hand side of your Visual Studio Gui), add `<customErrors mode="Off" />` in between the `<system.web>` tag. Otherwise you can't see the run time error on the website after you published your code.

## Tips

- You could use `Tools ->  SQL Server -> New Query` to connect to the azure database and send sql query.

- When you do `add-migration`, try to comment out the existed table in `bingfengappserviceContext.cs`, otherwise it will create migration class file to add in the same table and cost error

## Good readings:
- Please read [this](https://adrianhall.github.io/develop-mobile-apps-with-csharp-and-azure/chapter3/server/)
