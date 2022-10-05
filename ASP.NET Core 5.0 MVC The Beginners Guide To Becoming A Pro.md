# ASP.NET Core 5.0 MVC The Beginners Guide To Becoming A Pro

## Creating a controller

1. Create a controller (`ItemController`)
1. Declare a DbContext Object (`ApplicationDbContext _dp` )
1. create a constructor with DbContextOptions parametar (`DbContextOptions<ApplicationDbContext> options`) and initialize `_dp` wih it.
1. in the index method send the data to view `_dp.items`
1. create a view by
    1. right click on the `index` action then add view.
    1. select `Razor Pages`  then leave everyting as default.
    1. retrive the data that had been sent from the controller by `@model IEnumerable<InAndOut.Models.Item>`
    1. create a table and display the data
1. Add a navigation tab in `_Layout.cshtml` file

## Examples

- DbContext

```C#
public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
        {

        }
        public DbSet<Item> Items { get; set; }
    }
```

- Item Model

```C#
public class Item
    {
        [Key]
        public int Id { get; set; }
        public string Name { get; set; }
        public string Borrower { get; set; }
        public string Lender { get; set; }

    }
```

- Item Controller

```c#
using InAndOut.Data;
using InAndOut.Models;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace InAndOut.Controllers
{
    public class ItemController : Controller
    {
        ApplicationDbContext _dp;
        public ItemController(DbContextOptions<ApplicationDbContext> options)
        {
            _dp = new ApplicationDbContext(options);
        }
        public IActionResult Index()
        {
            IEnumerable<Item> items = _dp.Items;
            return View(items);
        }
    }
}

```

- Item View

```C#
  @model IEnumerable<InAndOut.Models.Item>

@{
    ViewData["Title"] = "Index";
}


<div class="container p-3">
    <div class="row pt-4">
        <div class="col-6">
            <h2 class="text-primary">Borrowed Items List</h2>
        </div>
        <div class="col-6">
            Create new Item
        </div>
    </div>
    <br />


    @if(Model.Count() > 0)
    {
        <table class="table table-bordered table-striped" style="width:100%">
            <thead>
                <tr>
                    <th>
                        Id
                    </th>
                    <th>
                        Item Name
                    </th>
                    <th>
                        Borrower
                    </th>
                    <th>
                        Lender
                    </th>
                </tr>
            </thead>
            <tbody>
                @foreach (var item in Model)
                {
                <tr>
                    <td width="30%">@item.Id</td>
                    <td width="30%">@item.Name</td>
                    <td width="30%">@item.Borrower</td>
                    <td width="30%">@item.Lender</td>
                </tr>
                }
            </tbody>

        </table>
    }
    else
    {
        <p>No items created yet</p>
    }

</div>
```

- Navigation tab in `_Layout.cshtml`

```C#
 <a class="nav-link text-dark" asp-area="" asp-controller="Item" asp-action="Index">Items</a>
```
