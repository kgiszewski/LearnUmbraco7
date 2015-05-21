#MVC Forms Inside Umbraco#

If you need to have full control over your forms, you may want to implement your own forms.  To do forms in Umbraco with MVC is easy, but you'll need some of the basic elements:

* A Model - The form fields as a POCO
* [A Surface Controller](/Chapter 06 - Surface, WebAPI and RenderMVC Controllers/02 - Surface Controllers.md) - The controller that handles the form submission
* A View - The form markup

For the following example, we'll do a simple contact form.

##The Model##
The model will need to capture some simple information from the user and is defined like so:

```
using System.ComponentModel.DataAnnotations;

namespace MyNamespace
{
    public class ContactUsModel
    {
        [Required]
        public string Name { get; set; }
        [EmailAddress]
        public string Email { get; set; }
        [Required]
        public string Message { get; set; }
    }
}
```
>Notice how we can decorate properties with validators.

##The SurfaceController##
The `SurfaceController` will handle the submission request and is defined like so:

```
using System.Web.Mvc;
using Umbraco.Core.Logging;
using Umbraco.Web;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    public class MyContactFormSurfaceController : SurfaceController
    {
        [ValidateAntiForgeryToken()] 
        public ActionResult HandleFormSubmit(ContactUsModel model)
        {
            TempData["hasError"] = false;

            if (!ModelState.IsValid)
            {
                TempData["hasError"] = true;

                //return to the current form page with the fields still populated
                return CurrentUmbracoPage();
            }

            //if we're here, the form is valid so let's do something with the form data
            //log the submission   
            LogHelper.Info<ContactUsModel>(model.Email);

            //we could send emails here

            //we could insert the form data into a DB or create Umbraco nodes to represent the form data
           
            TempData["success"] = true;

            //return to the form page with empty form fields
            //we could also redirect to a completely different page if we want
            return RedirectToCurrentUmbracoPage();
        }
    }
}
```

##The View##
Next we have to show the form.  The form is split into a view and a partial like so:

###The Form Markup###
```
@using MyNamespace
@inherits Umbraco.Web.Mvc.UmbracoTemplatePage
@{
    Layout = null;
}

@Html.Partial("~/Views/Partials/ContactForm.cshtml", new ContactUsModel())
```

###The Partial###
Then the partial would have this:
```
@model MyNamespace.ContactUsModel

@using (var form = Html.BeginUmbracoForm<MyNamespace.ContactFormSurfaceController>("HandleFormSubmit"))
{
    @Html.AntiForgeryToken()

    if (Convert.ToBoolean(TempData["hasError"]) == true)
    {
        <div class="col-md-12">
            <div class="alert alert-danger"><span class="glyphicon glyphicon-alert"></span><strong> Error! Please check the inputs.</strong></div>
        </div>
    }

    <div class="col-lg-6">
        <div class="well well-sm required-well"><strong><i class="glyphicon glyphicon-ok form-control-feedback"></i> Required Field</strong></div>
        <div class="form-group">
            <label for="">Your Name @Html.ValidationMessageFor(m => m.Name)</label>
            <div class="input-group">
                @Html.TextBoxFor(m => m.Name, new { @class = "form-control", placeholder = "Enter Name", required = "required" })
                <span class="input-group-addon"><i class="glyphicon glyphicon-ok form-control-feedback"></i></span>
            </div>
        </div>
        <div class="form-group">
            <label for="">Your Email @Html.ValidationMessageFor(m => m.Email)</label>
            <div class="input-group">
                @Html.TextBoxFor(m => m.Email, new { @class = "form-control", placeholder = "Enter Email", required = "required" })
                <span class="input-group-addon"><i class="glyphicon glyphicon-ok form-control-feedback"></i></span>
            </div>
        </div>
        <div class="form-group">
            <label for="">Message @Html.ValidationMessageFor(m => m.Message)</label>
            <div class="input-group">
                @Html.TextAreaFor(m => m.Message, new {@class = "form-control", rows = 5, required = "required"})
                <span class="input-group-addon"><i class="glyphicon glyphicon-ok form-control-feedback"></i></span>
            </div>
        </div>
        <input type="submit" name="submit" id="submit" value="Submit" class="btn btn-info pull-right">
    </div>
}
```

[<Back Overview](README.md)