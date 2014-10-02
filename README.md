DynaWorks
===========

This library contains the library files for using Navisworks and Dynamo

 Installing the Library
Once you have downloaded the relevant library from here. Once the package manager is fixed you will be able to download the library as a package but until then you need to use Github.

Once you downloaded the files as a zip or individually you need to copy paste the following files (Depending on whether you have 2015 or 2015 Navisworks) to a folder location. It doesn't matter where as long as the following files are available.

    DynaWorks15.dll
    DynaWorks15.xml
    DynaWorks15_DynamoCustomization.xml
    Interop.NavisworksAutomationAPI12.dll
    Interop.NavisworksIntegratedAPI12.dll

Once you have those files simply load the DynaWorks.dll ONLY the rest will all load up.


Once you have this you can get packages from the package Manager or get others yourself.


With all that aside lets get into my Navis Library.

As you can see there are currently 4 main parts to the Navisworks library.

Custom:- Where I put my Custom Nodes
Clash Detection:- Run, get tests, get results, get clashing objects.
FileSettings:- Open a Navis File, append files, Save As.
Objects:- GetNodes, search/query objects, get properties, get values, get attributes
Views:- get view names, camera positions vectors and looking points.


Now I don't want to go through everything so I thought I would point out the major stuff here.

For the moment I have built a number of custom nodes that are doing alot of the hard yards for you, so download them and check them out!! This is done for 2 reasons, first because these nodes save a ton of time in setting up what can be some really basic stuff, and 2 so you can get an understanding of how I have built the system and what elements need to interact with what.


FileSettings > OpenNavisFile
This is starting point of any use of the DynaWorks, it allows the user to open a file.
It has two options, the file path and the whether the session actually opens, this is one of the coolest features in this library as you can actually get data, run clashes and query objects without having to actually open the Navisworks UI. This saves a ton of resources in access and use of the tool.
If you do want to access it just set it to true and you can still navigate, update, save and close the file if you need to.



WARNING: Do not put this node inside a Custom Node, first it will always close the session for some reason after it runs if the node is in a custom node, this is a known issue. Second if you run multiple custom nodes or other options with it embedded it tries to open the file each time, encounters file locks etc......


Accessing Navisworks Objects (Also known as Nodes)
NOTE: Nodes are every single selectable object in a Navisworks Selection tree. Each has it's own attributes and properties, but Navisworks treats each one as a Node Instance with parents or children.

These are different from Nodes in Dynamo, I know it's annoying but to keep ease of use of API, communication with developers and so on, I am not creating new acronyms. 

There are currently a number of ways to use Dynamo to access Navisworks Nodes

First you can run a clash detection, filter through the clashes and return the objects you want. There is a fair bit happening in Clash detection so I suggest you look at the published nodes and also the examples on GitHub.

Second in Objects>NavisNodes>GetFilesInProject this will give you the starting point of the node files in each loaded nwc. You can then use the GetNodeChildren to cycle through the lists.

Last you can use any existing selection sets in Objects>NavisSelection and get a list of selectionsets manipulate and create what you need then use the GetNodesFromSelectionSets.

ClassNames vs ClassUserNames
There are two options here, both are equally valuable, one allows you to select objects by the Navisworks objectname, the second allows you to select objects by the string name data from Revit or other external package.
To know which name you need to select expand the little data readout, the first name of any node is the ClassName and the second is the UserClassName. The same thing goes for attributes.



Accessing Navisworks Attributes
From the nodes you can access the attributes, these are the various Tabs, that you see in the Properties panel. You can return attributes from nodes by using the Objects>NavisAttributes>GetNavisAttributesFromNodesList. This will return all the available attributes for those particular nodes in a list.

Accessing Navisworks Properties
The way the NavisCOMAPI library works when we want properties we have to setup a few things.
There are under Object>NavisPropertyList, the first returns a list of the available properties from the attribute.
These get the Attribute by ClassName or ClassUserName, then you need to identify the property and it will return all values as strings. I may work on numbers and other data in future, but this makes it easy for now.

Navisworks GetData
Second in objects there is GetData and NavisObjects and each has a Get Value. The option in GetData is for getting the properties and values of ANY Navis object so tests, results, the works. The other option is for getting the data from Navisworks property tabs, properties and values.

The query objects add alot of value in finding out what things are and what are in elements so feel free to use that!!

This is a great tool for querying or filtering objects by various attributes and so on!!!
It should work on every type of object in Navisworks.

Do not use this to get Property values, a special methods has to be done hence the once I have above, using this will cause it to crash or crazy behavior.

Navisworks ClashDetection
I am not going to detail all the requirements of the Clash Detection tool, just the setup.
First you need to have setup some clash tests, you can use Dynamo to Run tests and cleanup resolved issues if you want, otherwise you can simple extract the clash data.

To do this get an OpenNavisFile and connect it to ClashDetection>Create>GetClashDetection then under ClashDetection>Actions> you can get the clash tests, then the clash results by each clash or by group.

You can get various data from these XYZ points, views, properties/comments however, if you want to select the objects you must use the GetClashNodes available from the same area. Once you have the nodes you can perform other operations.

NOTE: Here is some great news, if not all your team has Navisworks Manage, they can still open the file and extract all data and clash data with Simulate!!! That way only the team members with manage need it to setup the clashes and so on. Users can access the NWF project to retrieve all the data from Simulate.

You can also run new clash tests, resolve clashing issues and so forth from Simulate!

Hopefully this stays open but who knows!!!
Navisworks Views
Lastly we can get View Data to do this we need to access the Views>GetSavedViews from the list.
You can then cycle through get comments, data camera positions etc...

In future if people want we can look at creating Navisworks views from with Revit, and updating the Navisworks file itself. for now I needed data and clash extraction.

Note: I have created a default and metres conversion. Please note this will only work if the conversion is set to feet, which so far in 2015 I can't seem to change the underlying API units. The reason for the conversion is the dynamo 3D view creation nodes are also in metres and this should save a ton of time.
Getting Started
To access DynaWorks15, download the published custom nodes packages from package manager. This should install and setup the .dll's and nodes. Be aware the Custom Nodes available so far are only for grabbing data/items in Navisworks, if you want to see complete definitions with revit access please download the examples from Github.

Other you can download the .dll zip file and all the data it contains and load them directly.

 Examples on the github repo you can find a number of definitions to get your started, these are commented to show you what the nodes are doing and should work with DynaWorks14 and 15.

If you need both versions of Navisworks then becareful on how you manage them!!!

Examples

The following are some examples of using DynaWorks to do the following.

    RevitClashesElementUpdate - Updates Revit elements comments to say whether something clashes from Navisworks
    RevitClashFamilies - Places Family Instances as Clash Points
    CreateSuitableRevitView - Creates a suitable Revit View of the family.


