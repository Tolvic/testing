# Unit Testing Use Cases

* [Testing Controller Actions](#testing-controller-actions)
  + [The Controller Under Test](#the-controller-under-test)
  + [Testing the View Returned by a Controller](#testing-the-view-returned-by-a-controller)
  + [Testing the View Data Returned by a Controller](#testing-the-view-data-returned-by-a-controller)
  + [Testing the Action Result Returned by a Controller](#testing-the-action-result-returned-by-a-controller)

## Testing Controller Actions
The examples used in these use cases do not use NUnit and need to be updated to using NUnit.

### The Controller Under Test
The below controller named `ProductController` will be used to demonstrates how to test whether a controller action returns a particular view, returns a particular set of data, or returns a different type of action result.

```csharp
using System;
using System.Web.Mvc;

namespace Store.Controllers
{
     public class ProductController : Controller
     {
          public ActionResult Index()
          {
               // Add action logic here
               throw new NotImplementedException();
          }

          public ActionResult Details(int Id)
          {

               return View("Details");
          }
     }
}
```

The `ProductController` contains two action methods named `Index()` and `Details()`. Both action methods return a view. Notice that the `Details()` action accepts a parameter named `Id`.

### Testing the View Returned by a Controller
If we were to test whether or not the `ProductController` returns the correct view, we would want to make sure that when the `ProductController.Details()` action is invoked, the Details view is returned as demonstrated below.

```csharp
using System.Web.Mvc;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Store.Controllers;

namespace StoreTests.Controllers
{
     [TestFixture]
     public class ProductControllerTest
     {
          [Test]
          public void Details_ValidId_ReturnsDetailsView()
          {
              // Arrange
               var controller = new ProductController();

               // Act
               var result = controller.Details(2) as ViewResult;

               // Assert
               Assert.AreEqual("Details", result.ViewName);

          }
     }
}
```

The ViewResult.ViewName property represents the name of the view returned by a controller. One big warning about testing this property is that there are two ways that a controller can return a view:

```csharp
public ActionResult Details(int Id)
{
     return View("Details");
}
```
 or:

 ```csharp
 public ActionResult Details(int Id)
 {
      return View();
 }
 ```

The second example, also returns a view named `Details`. However, the name of the view is inferred from the action name.

If you want to test the view name, then you must explicitly return the view name from the controller action.

### Testing the View Data Returned by a Controller
An MVC controller can pass data to a view using `View Data`. For example, the details of a particular product could be passed when the `ProductController Details()` action is invoked. To do this, you could create an instance of a `Product` class (defined in your model) and pass the instance to the Details view by using `View Data`.

The modified `ProductController`below includes an updated `Details()` action that returns a `Product`.

```csharp
using System;
using System.Web.Mvc;

namespace Store.Controllers
{
     public class ProductController : Controller
     {
          public ActionResult Index()
          {
               // Add action logic here
               throw new NotImplementedException();
          }

          public ActionResult Details(int Id)
          {
               var product = new Product(Id, "Laptop");
               return View("Details", product);
          }
     }
}
```

The below test, tests whether the expected data is contained in the view data by testing whether or not a `Product` representing a laptop computer is returned when you call the `ProductController Details()` action method.

```csharp
using System.Web.Mvc;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Store.Controllers;

namespace StoreTests.Controllers
{
     [TestFixture]
     public class ProductControllerTest
     {

          [Test]
          public void Details_ValidId_PassesCorrectProductToViewData()
          {
              // Arrange
               var controller = new ProductController();

               // Act
               var result = controller.Details(2) as ViewResult;
               var product = (Product) result.ViewData.Model;

               // Assert
               Assert.AreEqual("Laptop", product.Name);
          }
     }
}
```

### Testing the Action Result Returned by a Controller
A controller action can return a variety of types of action results including a `ViewResult`, `RedirectToRouteResult`, or `JsonResult`.

A complex controller might return different types of action results depending on the values of the parameters passed to the controller action.

For example, the modified `Details()` action below returns the `Details` view when you pass a valid product `Id` and redirects to the `Index()` action when an invalid product `Id` - in this case an `Id` with a value less than 1.

```csharp
using System;
using System.Web.Mvc;
namespace Store.Controllers
{
     public class ProductController : Controller
     {
          public ActionResult Index()
          {
               // Add action logic here
               throw new NotImplementedException();
          }
          public ActionResult Details(int Id)
          {
               if (Id < 1)
                    return RedirectToAction("Index");
               var product = new Product(Id, "Laptop");
               return View("Details", product);

          }
     }
}
```
The unit test below verifies that you are redirected to the `Index` view when an `Id` with the value -1 is passed to the `Details()` method.

```csharp
using System.Web.Mvc;
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Store.Controllers;
namespace StoreTests.Controllers
{
     [TestFixture]
     public class ProductControllerTest
     {
          [Test]
          public void Details_InvalidId_RedirectsToIndex()
          {
              // Arrange
               var controller = new ProductController();

               // Act
               var result = (RedirectToRouteResult) controller.Details(-1);

               // Assert
               Assert.AreEqual("Index", result.Values["action"]);

          }
     }
}
```
When you call the RedirectToAction() method in a controller action, the controller action returns a `RedirectToRouteResult`. The test checks whether the `RedirectToRouteResul`t will redirect the user to a controller action named `Index`.
