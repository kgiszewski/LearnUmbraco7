#Create a NuGet Package#

First and foremost, creating a NuGet package isn't a trivial task the first time you do it.  However the benefits of creating a NuGet package are very much worth the learning curve pains you'll have to overcome.  The main benefits are below:

* Developers can install your package easier through a command line
* NuGet will manage your dependencies (i.e. requires version x.y.z of Umbraco)
* Developers won't have to commit your package to source control
* Extra exposure and install options for your package if both the built-in and NuGet versions are available

What follows next is a guide on how to a process to create packages on demand.

##NuGet Package Format##
As with the built-in method, a NuGet package is pretty much just files and a manifest zipped together into a `.nupkg` file. 
>If you were to rename a `.nupkg` file and add `.zip` to the end, you can look inside one and get a feel for what's in it.

We will have to address certain formatting of the file structure/manifest aspects of creating a NuGet package.  Fortunately there are some tools we can use to get to that point.
>Please note that there are a few ways to create a NuGet package, this is just one method

##Node.js/Grunt##
So if you're already using Node.js and/or Grunt, this will be familiar to you. If not, please visit the [Appendix](/z-Appendix F - Node.js/README.md) to learn how to install these tools.

The idea is we will use Grunt to automate the building of our NuGet package.  In your repo root (not in your web root), create these files:
>For a live example, see the file structure on this project: https://github.com/kgiszewski/UmbracoBookshelf

###package.json###
```
{
  "name": "MyPackageName",
  "version": "0.0.0",
  "dependencies": {
    "grunt": "~0.4.2",
    "bower": "~1.2.8",
    "grunt-contrib-copy": "~0.5.0",
    "grunt-contrib-less": "~0.8.3",
    "grunt-contrib-concat": "~0.3.0",
    "grunt-contrib-uglify": "~0.2.7",
    "grunt-contrib-watch": "~0.5.3",
    "grunt-contrib-jshint": "~0.7.2",
    "grunt-cli": "~0.1.11",
    "grunt-contrib-clean": "~0.5.0",
    "grunt-touch": "~0.1.0",
    "guid": "0.0.12",
    "adm-zip": "~0.4.3",
    "rimraf": "~2.2.5",
    "grunt-nuget": "~0.1.1",
    "grunt-template": "~0.2.2",
    "fs-extra": "~0.8.1",
    "grunt-msbuild": "~0.1.9",
    "grunt-dotnet-assembly-info": "~1.0.9",
    "load-grunt-tasks": "~0.4.0",
    "grunt-umbraco-package": "0.0.6"
  },
  "repository": {
    "type": "git",
    "url": "http://mydomain.local"
  },
  "devDependencies": {
    "grunt-msbuild": "^0.1.12"
  }
}
```
>These are the dependencies Node.js needs.

###gruntfile.js###
```
module.exports = function(grunt) {
  require('load-grunt-tasks')(grunt);
  var path = require('path')

  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    pkgMeta: grunt.file.readJSON('config/meta.json'),
    dest: grunt.option('target') || 'dist',
    basePath: path.join('<%= dest %>', 'App_Plugins', '<%= pkgMeta.name %>'),

    watch: {
      options: {
        spawn: false,
        atBegin: true
      },
	  
	  less: {
        files: ['src/Assets/less/*.less'],
        tasks: ['less:dist']
      },

      app_plugins: {
        files: ['src/**/**'],
        tasks: ['copy:app_plugins']
      },

      dll: {
        files: ['src/**/*.cs'],
        tasks: ['msbuild:dist', 'copy:dll']
      }
    },
	
	less: {
      dist: {
        options: {
          paths: ["src/assets/less"],
        },
        files: {
          '<%= basePath %>/css/application.css': 'src/assets/less/*.less',
        }
      }
    },

    copy: {
      app_plugins: {
        cwd: 'src/App_Plugins/MyPackage',
        src: ['**'],
        dest: '<%= basePath %>',
        expand: true
      },
      dll: {
        cwd: 'src/bin/',
        src: 'MyPackage.dll',
        dest: '<%= dest %>/bin/',
        expand: true
      },

      nuget: {
        files: [
          {
            cwd: '<%= dest %>/App_Plugins',
            src: ['**/*', '!bin', '!bin/*'],
            dest: 'tmp/nuget/content/App_Plugins',
            expand: true
          },
		  {
            cwd: '<%= dest %>/MyPackage/',
            src: ['**/*'],
            dest: 'tmp/nuget/content/MyPackage',
            expand: true
          },
          {
            cwd: '<%= dest %>/bin',
            src: ['*.dll'],
            dest: 'tmp/nuget/lib/net40',
            expand: true
          }
        ]
      },
      umbraco: {
        cwd: '<%= dest %>',
        src: '**/*',
        dest: 'tmp/umbraco',
        expand: true
      }
    },

    nugetpack: {
    	dist: {
    		src: 'tmp/nuget/package.nuspec',
    		dest: 'pkg'
    	}
    },

    template: {
    	'nuspec': {
			'options': {
    			'data': { 
					name: '<%= pkgMeta.name %>',
					version: '<%= pkgMeta.version %>',
					url: '<%= pkgMeta.url %>',
					license: '<%= pkgMeta.license %>',
					licenseUrl: '<%= pkgMeta.licenseUrl %>',
					author: '<%= pkgMeta.author %>',
					authorUrl: '<%= pkgMeta.authorUrl %>',
					files: [{ path: 'tmp/nuget/content/App_Plugins', target: 'content/App_Plugins'}]
	    		}
    		},
    		'files': { 
    			'tmp/nuget/package.nuspec': ['config/package.nuspec']
    		}
    	}
    },

    umbracoPackage: {
      options: {
        name: "<%= pkgMeta.name %>",
        version: '<%= pkgMeta.version %>',
        url: '<%= pkgMeta.url %>',
        license: '<%= pkgMeta.license %>',
        licenseUrl: '<%= pkgMeta.licenseUrl %>',
        author: '<%= pkgMeta.author %>',
        authorUrl: '<%= pkgMeta.authorUrl %>',
        manifest: 'config/package.xml',
        readme: 'config/readme.txt',
        sourceDir: 'tmp/umbraco',
        outputDir: 'pkg',
      }
    },

    clean: {
      build: '<%= grunt.config("basePath").substring(0, 4) == "dist" ? "dist/**/*" : "null" %>',
      tmp: ['tmp']
    },

    assemblyinfo: {
      options: {
        files: ['src/MyPackage.csproj'],
        filename: 'AssemblyInfo.cs',
        info: {
          version: '<%= (pkgMeta.version.indexOf("-") ? pkgMeta.version.substring(0, pkgMeta.version.indexOf("-")) : pkgMeta.version) %>', 
          fileVersion: '<%= pkgMeta.version %>'
        }
      }
    },

    msbuild: {
      options: {
        stdout: true,
        verbosity: 'quiet',
        maxCpuCount: 4,
		version: 4.0,
        buildParameters: {
          WarningLevel: 2,
          NoWarn: 1607
        }
      },
      dist: {
        src: ['src/MyPackage.csproj'],
        options: {
          projectConfiguration: 'Debug',
          targets: ['Clean', 'Rebuild'],
        }
      }
    }
  });

  grunt.registerTask('default', ['clean', 'less', 'assemblyinfo', 'msbuild:dist', 'copy:dll', 'copy:app_plugins']);
  grunt.registerTask('nuget',   ['clean:tmp', 'default', 'copy:nuget', 'template:nuspec', 'nugetpack']);
  grunt.registerTask('umbraco', ['clean:tmp', 'default', 'copy:umbraco', 'umbracoPackage']);
  grunt.registerTask('package', ['clean:tmp', 'default', 'copy:nuget', 'template:nuspec', 'nugetpack', 'copy:umbraco', 'umbracoPackage', 'clean:tmp']);
};
```
>There is a lot going on here, but this file is defining commands.  One of those commands is to create a NuGet package.  You need to switch out paths and your package name.

The commands we just registered are:

* `grunt`- This runs the operations `clean`, `less`, `assemblyinfo`, `msbuils:dist`, `copy:dll` and `copy:app_plugins`.  These are the default operations.
* `grunt nuget` - This runs all of the default ones plus the NuGet ones that ultimately create a package.
* `grunt umbraco` - This runs the default ones plus creates a built-in package (from the last section)
* `grunt package` - Runs the `umbraco` and `NuGet` commands creating both types of packages.

>We will run these commands from the CLI and they will package up everything.

###config/meta.json###
Create this file in a sub-folder called `config`.
```
{
	"name": "MyPackage",
	"version": "0.0.1",
	"url": "http://mypackage.local",
	"author": "Your name",
	"authorUrl": "http://mypackage.local/",
	"license": "MIT",
	"licenseUrl": "http://opensource.org/licenses/MIT"
}
```
>Grunt will use this file to fill in blanks on templates.  It'll also update your  DLL version numbers based on this file.

###config/package.nuspec###
```
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
    <metadata>
        <id>MyPackage</id>
        <version><%= version %></version>
        <title><%= name %></title>
        <authors>nugetUsername</authors>
        <owners>nugetUsername</owners>
        <projectUrl>http://mypackage.local</projectUrl>
        <description><![CDATA[foo]]></description>
        <tags>umbraco</tags>
        <iconUrl>http://mypackage.local/logo.png</iconUrl>
        <licenseUrl><%= licenseUrl %></licenseUrl>
        <dependencies>
            <dependency id="UmbracoCms.Core" version="7.2.0" />
        </dependencies>
    </metadata>
</package>
```
>This file is used to create the NuGet manifest. Fill this out and some fields will be auto-populated.

###config/package.xml###
```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<umbPackage>
  <info>
    <package>
      <name><%= name %></name>
      <version><%= version %></version>
      <license url="<%= licenseUrl %>"><%= license %></license>
      <url><%= url %></url>
      <requirements>
        <major>0</major>
        <minor>0</minor>
        <patch>0</patch>
      </requirements>
    </package>
    <author>
      <name><%= author %></name>
      <website><%= authorUrl %></website>
    </author>
    <readme><![CDATA[<%= readmeContents %>]]></readme>
  </info>
  <DocumentTypes />
  <Templates />
  <Stylesheets />
  <Macros />
  <DictionaryItems />
  <Languages />
  <DataTypes />
  <control />
  <Actions>
  </Actions>
  <files>
  	<% files.forEach(function(file) { %>
  	<file>
      <guid><%= file.guid %>.<%= file.ext %></guid>
      <orgPath><%= file.dir %></orgPath>
      <orgName><%= file.name %></orgName>
    </file>
	<% }); %>
  </files>
</umbPackage>
```
>This file is used to generate the built-in package.  Nothing to change here as this is all auto-populated.

###config/readme.txt###
Create this file and put in what you'd like to appear in the built-in package manifest.  This will show in the backoffice when someone clicks on the package details.

##Wow##
OK that seemed like a lot of configuration (because it was).  Next we need to install Node.js dependencies.  To do so, open a command prompt and go to the same directory as the `package.json`.  Type in `npm install`.  If it's running properly you'll get some stuff that downloads.

Next choose a command to run.

If you simply want the NuGet package, just type in `grunt nuget`.  You're package will then show up in `/pkg` ready for your review.

Run `grunt package` to get both the NuGet and Umbraco built-in packages to be created.

Note that the file names of the packages are already generated with a version name.

##Troubleshooting##
If you are getting any errors (like Node cannot be found), try reinstalling Node and make sure Node is in your Windows environment path.

If you are getting "Grunt isn't a command", try installing again like so: `npm install -g grunt-cli.  You will have to be running the cmd prompt as admin.

>npm (or Node Package Manager) won't work if Node isn't installed properly.

If you need to update the operations that run in Grunt, you will need to update the `gruntfile.js` to suit your needs.

You can run commands with the `-verbose` tag to get more specific errors (i.e. `grunt -verbose`)

##Source Injection##
Tom Fulton added a cool feature that allows the output of running your commands to end up in a separate install.

For instance if you run `grunt --target="c:\dev\myproject\umbracofolder"`, any changes to my package solution will get injected into the target solution.  This happens because when grunt runs it normally ends up in `/dist` and then the package files are created and land in `/pkg`. By specifying a target, we are changing where the output goes (instead of `/dist`).

Addtionally if you run `grunt watch`, grunt will listen for changes to certain files and run the tasks you decide.

##Special Thanks##
Special thanks to Tom Fulton for authoring the Node Umbraco packager (https://www.npmjs.com/package/grunt-umbraco-package) and creating this workflow #h5yr!

##Yo Umbraco!##
There is also a Node.js based tool to help get Umbraco property editors off the ground and into NuGet quickly.  It's called Yo Umbraco (http://creativewebspecialist.co.uk/2014/03/24/yo-umbraco/) and was developed by Warren Buckley and others. Yo Umbraco scaffolds what you need quickly and includes NuGet packaging functionality.

[<Back 01 - Built-In](01 - Built-In.md)