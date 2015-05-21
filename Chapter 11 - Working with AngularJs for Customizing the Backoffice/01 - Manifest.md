#Manifest#

All items in our plugin should reside in the `~/App_Plugins/` folder.  To make things well organized, create a subfolder under that called: `HideTab`.

Create your manifest file by creating a plain old text file named `package.manifest` inside your `~/App_Plugins/HideTab` directory.

Then add this to the inside of that file:

```js
{
  "propertyEditors": [
    {
      "name": "Hide Tab",
      "alias": "HideTab",
      "editor": {
        "valueType": "JSON",
        "view": "~/App_Plugins/HideTab/hideTab.html"
      },
      "prevalues": {
        
        
      }
    }
  ],
  "css": [
   
  ],
  "javascript": [
    "~/App_Plugins/HideTab/hideTab.directive.js"
  ]
}
```

The manifest file declares that we have a single property editor named `HideTab` that will be saved as `JSON`.  The view for this property editor will be `hideTab.html` and we are registering a single directive in the `hideTab.directive.js` file.

>If you had additional javascript or CSS files to register, you'd do so here.  You can also define prevalues here (config options) that will appear in the data type screen when you create a new instance of this property editor.  So for now this won't have any options.

[<Back Overview](README.md)

[Next> 02 - The View and Directive](02 - The View and Directive.md)