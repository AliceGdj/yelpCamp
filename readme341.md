=============
# YELPCAMP V10
=============


# Editing Capgrounds

* Add method-override
* Add Edit Route for Campgrounnds
* Add Lnk to Edit Page
* Add Update Route
* Fix $set problem

we want to have another button for edit
create edit.ejs
copy paste new.ejs 
the only difference will be where the form is going and the type of request
    
    In campground.js
   
    1) first step
    
    // EDIT CAMPGROUND ROUTE 
    router.get("/:id/edit", function(req, res){
        res.render("campgrounds/edit", {campground: foundCampground});
    });
    Rem: we dont know foundCampground yet
    
    2) second step
    
    // EDIT CAMPGROUND ROUTE 
    router.get("/:id/edit", function(req, res){
        Campground.findById(req.params.id, function(err, foundCampground){
            if(err){
                res.redirect("/campgrounds")
            } else {
                res.render("campgrounds/edit", {campground: foundCampground});      
        }
    });
    
    In edit.ejs
    <form action="/campgrounds/<%= campground._id %>/edit?_method=PUT" method="POST">
    Rem: Not as for new.ejs we need a PUT request here 
    Also we change title of page 
    <h1 style="text-align: center">Edit <%= campground.name %></h1> 
    
349. AUTHORIZATION PART ONE

Code before  checkCampgroundOwnership code

// EDIT CAMPGROUND ROUTE 
router.get("/:id/edit", function(req, res){
    //is user logged in?
    if(req.isAuthenticated()){
        Campground.findById(req.params.id, function(err, foundCampground){
            if(err){
                res.redirect("/campgrounds");
            } else {
                //does user own the campground?
                // SAME NUMBER BUT DIFF / MANGOOSE OBJECT VS STRING 
                //if(findCampground.author.id === req.user._id)
                if(foundCampground.author.id.equals(req.user._id)){
                    res.render("campgrounds/edit", {campground: foundCampground});
                } else{
                    res.send("YOU DO NOT HAVE THE PERMISSION TO DO THAT");
                }
            }
        });
    } else {
        console.log("YOU NEED TO BE LOGGED IN");
        res.send("YOU NEED TO BE LOGGED IN");
    }
    
        //otherwise redirect somewhere
    //if not redirect somewhere
});


