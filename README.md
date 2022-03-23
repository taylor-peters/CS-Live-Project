# C# Live Project

## Introduction
For the last two weeks of my time at the tech academy, I worked with my peers in a team developing a MVC web application as part of larger group project.  In my application, I was tasked with completing both [back end stories](#back-end-stories) and [front end stories](#front-end-stories). Built out, styled, and tested multiple sections of client’s website using MVC, C#, JavaScript, and CSS.  Rebuilt client’s database with Entity Framework.  Worked closely with project manager and teammates to ensure project quality standards were met.  During this two week sprint, I gained valuable experience working with version control software, operating as part of a team, and completing tasks in a timely manner.

Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.

## Front End Stories
* [CRUD Pages](#crud-pages)
* [Display Categorization](#display-categorization)
* [Accordion Table, Conditional Icon and Text Display, and Dropdown Menu](#accordion-table-conditional-icon-and-text-display-and-dropdown-menu)
* [Change Label Element](#change-label-element)

### CRUD Pages
#### Production Photos
![ezgif com-gif-maker (3)](https://user-images.githubusercontent.com/93218689/159784389-ad4f8f7a-5d41-48ee-94bc-7b439faf0467.gif)

#### Rental Histories
![ezgif com-gif-maker (5)](https://user-images.githubusercontent.com/93218689/159789059-bff99762-a326-489b-a836-88b8df4fef03.gif)

### Display Categorization
Database items are categorized by title.  This was accomplished with nestead foreach loops that selected distinct titles, then populated the corresponding items underneath.

    @foreach (var title in Model.Select(m => m.Title).Distinct().ToList())
        {
            <h2 class="mb-3 ml-3 mt-5">@title</h2>

            <div class="container-fluid">
                <div class="card-deck">
                    @foreach (var item in Model)
                    {
                        if (item.Title == title)
                        {
                            <div>
                                <div class="card ProductionPhoto-Index--Card  mb-3 ml-3" style="width: 20rem;">
                                    <a href="@Url.Action("Details", "ProductionPhotos", new { id = item.ProductionPhotoId })">
                                        <img class="card-img-top" src="data:image;base64,@System.Convert.ToBase64String(item.Photo)" alt="Card image cap" href=")">
                                    </a>
                                    <h5 class="card-title ml-3">
                                        @item.Title
                                    </h5>
                                    <h6 class="card-subtitle mb-2 text-muted ml-3">
                                        @item.Description
                                    </h6>
                                    <span class="text-center mb-2">
                                        <a class="ProductionPhoto-Index--CardPill1 badge badge-pill" href="@Url.Action("Edit", "ProductionPhotos", new { id = item.ProductionPhotoId })">Edit</a>
                                        <a class="ProductionPhoto-Index--CardPill2 badge badge-pill" href="@Url.Action("Delete", "ProductionPhotos", new { id = item.ProductionPhotoId })">Delete</a>
                                    </span>
                                </div>
                            </div>
                        }
                        else { continue; }
                    }

    
### Accordion Table, Conditional Icon and Text Display, and Dropdown Menu
Initial creation of the accordion table resulted in all hidden elements displaying regardless of which item was clicked.  Solving this required instantiallizing and incrementing a variable which was then added to the id elements and the targeting href sections.  Icons were implemented using Font Awesome and a simple if statement to check if the relevant data was true or false.  Text color modificaions was accomplished in the same way.  The dropdown menu utilizes both Font Awesome icons and standard bootstrap dropdown features.  Modifications were made to both blocks of code to match user story requirements.

![ezgif com-gif-maker (5)](https://user-images.githubusercontent.com/93218689/159789059-bff99762-a326-489b-a836-88b8df4fef03.gif)

      
     @{int i = 0;}
        @foreach (var item in Model)
        {
            <tr>
                <td class="text-center align-middle" style="width: 5%">

                    @if (item.RentalDamaged == true)
                    {

                        <i class="fas fa-times-circle fa-lg" id="testxmark"></i>
                    }
                    else
                    {
                        <i class="fas fa-check-circle fa-lg" id="testcheckmark"></i>
                    }
                </td>
                <td class="text-center align-middle" style="width: 15%">
                    <h4>
                        <span class="badge badge-dark align-middle">
                            @Html.DisplayFor(modelItem => item.Rental)
                        </span>
                    </h4>
                </td>
                <td class="text-left align-middle" style="width: 70%" data-toggle="collapse" href="#collapseExample_@i" role="button" aria-expanded="false" aria-controls="collapseExample_@i">
                    @if (item.RentalDamaged == true)
                    {
                        <div class="text-dark"> 
                            Damages
                        </div>
                        <div class="collapse" id="collapseExample_@i">
                            <div>
                                @Html.DisplayFor(modelItem => item.DamagesIncurred)
                            </div>
                        </div>


                    }
                    else
                    {
                        <div class="text-muted">
                            Notes
                        </div>
                        <div class="collapse" id="collapseExample_@i">
                            <div class="text-muted">
                                @Html.DisplayFor(modelItem => item.DamagesIncurred)
                            </div>
                        </div>
                        

                    }
                </td>
                <td class="text-center align-middles">
                    <div class="dropdown float-right" style="position: static">
                        <button class="btn btn-secondary" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                            <i class="fas fa-ellipsis-v"></i>
                        </button>
                        <div class="dropdown-menu menutest" aria-labelledby="dropdownMenuButton">

                            <a class="dropdown-item" href="@Url.Action("Details", "RentalHistories", new { id = item.RentalHistoryId })"><i class="fas fa-info-circle"></i> Details</a>

                            <a class="dropdown-item" href="@Url.Action("Edit", "RentalHistories", new { id = item.RentalHistoryId })"><i class="fas fa-edit"></i> Edit</a>
                            <div class="dropdown-divider"></div>


                            <a class="dropdown-item text-danger" href="@Url.Action("Delete", "RentalHistories", new { id = item.RentalHistoryId })"><i class="fas fa-trash"></i> Delete</a>
                        </div>
                    </div>
                </td>
            </tr>
            i++;
        }
         
### Change Label Element
Added JavaScript to the create page that changed the title of the "Damages" label to "Notes" when selected.
         
   @section Scripts {
        @Scripts.Render("~/bundles/jqueryval")
        <script type="text/javascript">

            $(document).ready(function () {
                $('#RentalDamaged').click(function () {
                    var isChecked = $('#RentalDamaged').prop('checked');
                    if (isChecked) {
                        document.getElementById("testID").innerHTML = "Damages Incurred";
                    }
                    else {
                        document.getElementById("testID").innerHTML = "Notes";
                    }
                });
            });

      
        </script>
    }


## Back End Stories
* [Photo to Byte Array Conversion](#photo-to-byte-array-conversion)
* [Rental Histories Sorting Algorithm](#rental-histories-sorting-algorithm)


### Photo to Byte Array Conversion
Converted user-submitted photos to byte arrays to allow for convenient storage in database.  Reverse process was used to display image on applicable CRUD pages.  
      
     public ActionResult Create([Bind(Include = "ProductionPhotoId,Title,Description")] ProductionPhoto productionPhoto, HttpPostedFileBase file1)
        {
             if (ModelState.IsValid && file1 != null)
            {
                byte[] bytes;
                using (BinaryReader br = new BinaryReader(file1.InputStream))
                {
                    bytes = br.ReadBytes(file1.ContentLength);
                }           
                productionPhoto.Photo = bytes;
                db.ProductionPhotoes.Add(productionPhoto);
                db.SaveChanges();
                return RedirectToAction("Index");
            }

            return View(productionPhoto);
        }
    

 ### Rental Histories Sorting Algorithm
I needed to provide the user with multiple options for how to sort the information that was being displayed from the database.  To accomplish this, I utilized a switch statement to modify the way the rental list was passed to the Index view. 

      public ActionResult Index(string sortOrder)
        {
            ViewBag.IdSortParm = String.IsNullOrEmpty(sortOrder) ? "No Extra Sorting" : "";
            ViewBag.NameSortParm = sortOrder == "Rental A - Z" ? "Rental A - Z" : "Rental Z - A";
            ViewBag.DamageSortParm = sortOrder == "Damaged Rentals" ? "Damaged Rentals" : "Undamaged Rentals";

            var rentals = from r in db.RentalHistories
                          select r;
            switch (sortOrder)
            {
                case "Rental A - Z":
                    rentals = rentals.OrderBy(r => r.Rental);
                    break;
                case "Rental Z - A":
                    rentals = rentals.OrderByDescending(r => r.Rental);
                    break;
                case "Damaged Rentals":
                    rentals = rentals.OrderByDescending(r => r.RentalDamaged);
                    break;
                case "Undamaged Rentals":
                    rentals = rentals.OrderBy(r => r.RentalDamaged);
                    break;
                default:
                    rentals = rentals.OrderBy(r => r.RentalHistoryId);
                    break;

            }
            foreach (var r in rentals)
            {
                Console.WriteLine(r);
            }

            return View(rentals.ToList());
        }
        
 ![ezgif com-gif-maker (6)](https://user-images.githubusercontent.com/93218689/159791224-e6ae6efc-8f68-497d-8923-e974ac9a200a.gif)



*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*

## Other Skills Learned

* Working with a group of developers to identify front and back end bugs to the improve usability of an application
* Learning new efficiencies from other developers by observing their workflow and asking questions  

  
*Jump to: [Front End Stories](#front-end-stories), [Back End Stories](#back-end-stories), [Other Skills](#other-skills-learned), [Page Top](#live-project)*
