#SurfaceController#

`SurfaceControllers` are Umbraco's way to provide quick MVC routes for custom behavior. These are mostly used to handle form requests but can also return text, JSON, XML and views.

Please see the [MVC forms section](/Chapter 07 - Forms/01 - Integrating Umbraco with MVC Forms.md) for using `SurfaceControllers` with forms.

To create a quick web service you can do the following:

```c#
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}

```

You can hit that web service at it's default URL: `/umbraco/surface/mynamesurface/getnames`

Which will send back:
```js
["Warren","Bob","Tom"]
```

If you would like the URL to be a little nicer, you can add an area for routing by adding a `PluginController` attribute like so:

```c#
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    [PluginController("MyNameThing")]
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}
```

You would then be able to get to the service with this URL: `/umbraco/MyNameThing/mynamesurface/getnames`

Of course the URL is public and if you want to restrict access to members only, you can add another attribute like so:

```c#
using System.Collections.Generic;
using System.Web.Mvc;
using Umbraco.Web.Mvc;

namespace MyNamespace
{
    [MemberAuthorize(AllowAll = true)]
    [PluginController("MyNameThing")]
    public class MyNameSurfaceController : SurfaceController
    {
        public ActionResult GetNames()
        {
            var listOfNames = new List<string>() {"Warren", "Bob", "Tom"};

            return Json(listOfNames, JsonRequestBehavior.AllowGet);
        }
    }
}
```

`SurfaceController`'s are not meant to be secured for the backoffice and are always public.

##Special Routing##
Surface controllers follow MVC convention for naming and routing, with one exception (see below). Please reference official Umbraco and Microsoft documentation.

Surface controllers are a little different than normal controllers in that they will also work with any URL. That is because when you render a surface controller using the `BeginUmbracoForm` extension method, a hidden input field with a name of `ufprt` (short for "Umbraco Form Post RouTe") will be rendered. The value is an encrypted string that contains all the routing information, such as the controller, action method, and area. If you decrypt the value stored in the `ufprt` field generated by a form rendered with the action method in the above code example, the value would be `c=MyNameSurface&a=GetNames&ar=MyNameThing` (i.e., the controller, action, and area).

>Note: The latest versions of Umbraco allow you to drop the `surface` from the name of the class to make your URL much nicer.

[<Back 01 - RenderMVC Controllers](01 - RenderMVC Controllers.md)

[Next> 03 - WebAPI Controllers](03 - WebAPI Controllers.md)
