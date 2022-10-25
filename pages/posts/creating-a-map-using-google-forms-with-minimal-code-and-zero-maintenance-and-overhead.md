---
  type: posts
  title: Creating the EcoBricks UK site
  date: 2019-08-14
---
  
Let's start by defining what problem needs to be solved and the main attributes it **must** have in the solution.  If you want to skip to the final product here is the [code](https://glitch.com/edit/#!/momentous-sawfish) and the [page](https://momentous-sawfish.glitch.me/) where you can add your own ecobrick, try not to troll too much.

## The task

A friend asked me for help creating a simple map of everyone in the UK who is making or collecting ecobricks.  For the uninitiated, ecobricks are plastic bottles stuffed tightly with little bits of unrecylable plastic to make an eco friendly alternative to building materials.  There is loads of information on the site [ecobricks.org](https://ecobricks.org)

![wall of ecobricks](https://skinnylaminx.com/wp-content/uploads/2018/02/ecoladrillo.jpg)

A way to do this is currently implemented on [gobrik.com](https://www.gobrik.com/) features include logging your brick on the site and adding yourself as a collector.

The members of the Facebook group [Ecobricks UK](https://www.facebook.com/groups/1639389892816439/) thought it was a bit too complicated and wanted something simple on one page.  I kind of agree, it feels wrong to save all that waste and then post them, seems wasteful

## Things to keep in mind

I don't want to have to maintain this for the rest of my life so it needs to be easy to understand and change as possible while keeping the costs as near to zero as possible to run.  This isn't a charity it needs to be sustainable.

## The solution

I want to keep it simple and performant as possible to limit any bespoke code and not maintain a server.

### Static pages

To be as easy as possible to alter I chose [Glitch](glitch.com) and configured sharing to enable to see the code and alter it. It's just a simple html/css/js stack nothing complicated. 

![glitch icon](https://cdn.gomix.com/2bdfb3f8-05ef-4035-a06e-2043962a3a13%2Flogo-day.svg)

### Capturing and storing the data

The end goal is for the user to figure out where in the country they can drop off thier lovingly crafted ecobrick. We need to store who is collecting bricks and where they want them dropped off. Its probably not a good idea to store names and addresses publicly so facebook name seemed like the best compromise with a link on the main page to the group so the collector can be contacted for the address of the drop off location.

#### Where to store the data

There are a few solutions to this problem and it took a few iterations to figure out the best solution.  They all involve the G-Suite (Google created services) because they all work together and many non tech people understand spreadsheets.

Google forms are also mobile friendly and you don't need to worry about any data input security issues like people putting scripts in there.  The data asked for in the form will be a choice of Collecting or Making a brick, Facebook name and nearest city / village to drop brick into. 

##### Google form + google spreadsheet

It's really easy to set up the form to save in a spreadsheet, just a click of a button.  
![google forms logo](https://www.gstatic.com/images/branding/product/2x/forms_96dp.png) ![google sheets logo](https://www.gstatic.com/images/branding/product/2x/sheets_96dp.png) 

### Displaying the data on a map

Now we have a list of cities we need to put them on a map.  I'm going to skirt over the issue that if the user types in Newcastle they could mean Newcastle upon Tyne in the North of England or Newcastle under Lyme in the midlands.  I'm assuming that this works 99% of the time and if it's wrong the users can be more specific.

Google maps allows custom maps using the `my map` feature.  On the map interface there is a share button where you can imbed the map using an iframe.

#### Manual map import

This works great if you have the patience, you can add different markers etc.

#### Auto import into maps

Google map import has a great feature to import a spreadsheet with place names and it will automatically geocode them, give them lat/long coordinates and put them on the map.  You have to put the markers on manually.

Both of these solutions work but are not sustainable, it relies on human interaction.

### Sheetsee to the rescue.

Using the power of both tabletop and leaflet.js it is possible to take the content of the spreadsheet and convert it into a map! 

![sheetsee](http://jlord.us/sheetsee.js/img/next-sheetsee.png)

#### Problem solved!

Its never that easy 😸Leaflet.js exects values to have a lat/long value for each location.  It focuses on display and not geocoding vague addresses.  The lat/long addresses need to be in the spreadsheet before using the leaflet.js library.

#### Google API to the rescue

Or maybe not--my idea was to read the place name and geocode the addresses on the fly using the google maps API.  But alas, roadblock this way as well I'm afraid, it used to be possible to access the google API without a developer API key as explained by this [shocked blogger](http://geoawesomeness.com/developers-up-in-arms-over-google-maps-api-insane-price-hike/) .  Now they require a key and that requires a valid credit card 🤑. Booo!!!

#### Maybe, google scripts to the rescue?

![google scripts](https://www.gstatic.com/images/icons/material/product/2x/apps_script_64dp.png)

This idea finally worked.  It feels a bit hacky but it's all in the name of simplicity.  Google scripts are little snippets of javascript that allow you to add a bit of extra customisation not normally available.  Think VB scripts for excel but with the power to interact with any google service.  To add a script to a google sheet use the tools menu of the responses spreadsheet for the form made before.  Below is the script that should be configured to run on each **form submission**.  The script is very hard coded and could definately be improved, it expects the location in column 3 lat in column 6 and long in column 7.  It also is bias towards the united kingdom when searching for the correct location.

```javascript
function myFunction() {
  // Get the current sheet we are working on.
  const sheet = SpreadsheetApp.getActiveSheet();
  const data = sheet.getDataRange().getValues();
  
  const coordinates = geoCodeAddress(data)
  
  // Update the lat / long columns in the spreadsheet with the ones got from maps.
  const latCell = sheet.getRange(data.length, 6)
  const longCell = sheet.getRange(data.length, 7)
  latCell.setValue(coordinates.lat);
  longCell.setValue(coordinates.lng)
  
  Logger.log('Added cordincates lat: '+coordinates.lat+', long '+coordinates.lng)

  
}

function geoCodeAddress(data) {
  
  const location = data[data.length-1][2]
  
  return Maps.newGeocoder()
     // The latitudes and longitudes of United kingdom off some forum so it might not work :).
    .setBounds(49.383639452689664, -17.39866406249996, 59.53530451232491,8.968523437500039)
    .geocode(location)
    .results[0]
    .geometry
    .location
}

```

#### Where are my coordinates?

If all went well on every form submission there should be a new row in the spreadsheet with auto populated lat/long values.  It's possible to check on google maps by putting the lat and long values directly in google.  Make sure you get the lat and long the right way around otherwise you end up in the middle of the ocean.  It needs to be in the format `lat,long`

## Put it all together

We now need to glue everything together using the [site on glitch](https://glitch.com/edit/#!/momentous-sawfish).  I copyed the sheetsee code from the [official source](https://github.com/jlord/sheetsee.js/tree/master/js) into a file called sheetsee.js. Just copy it over, it feels dirty, it is dirty, don't think about it too much.  Then add the `script` tags anywhere inside your `head` tags in the html file.

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/tabletop.js/1.5.1/tabletop.min.js" defer></script> 
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.0.3/dist/leaflet.css" />
    <script type="text/javascript" src="/sheetsee.js" defer></script>
```

Next you need a javascript file to configure the map and show it.  First make a file called `script.js`.  Add that between the `head` tags of your `index.html` file.

```
<script type="text/javascript" src="/script.js" defer></script>
```

Your `script.js` file should include the setup for your map, it will look the same as below with the exception of a different reference in the `publicSpreadsheetUrl` variable and different column names, see the sheetsee documentation for any other settings or to understand what is going on here.

```javascript
var publicSpreadsheetUrl = 'https://docs.google.com/spreadsheets/d/1rjTkOAwDVe1-FD9IyHsaymXP-LfNFp9TS433uRKtxaM/edit?usp=sharing';
var addressColumn = 'Which town are you in? (If are frequently in more than one town, please fill out the form again for that town)'
var nameColumn = "What is your name on Facebook (this will be used for people to contact you so we don't need to share our address publically)"
var collecting = "Are you collecting or making ecobricks?"
document.addEventListener('DOMContentLoaded', function() {
        Tabletop.init( { key: publicSpreadsheetUrl,
                     callback: showInfo,
                     simpleSheet: true } )
      })
      function showInfo (data) {
        var popup = "<h3>{{"+collecting+"}}<br/>{{"+nameColumn+"}}<br/>{{"+addressColumn+"}}</h3>"
        var mapOptions = {
          data: data,
          mapDiv: 'map',
          geoJSONincludes: [nameColumn, collecting, addressColumn],
          template: popup,
          preferCanvas: true
        }
        Sheetsee.loadMap(mapOptions)
      }
```

### What reference do I use for the spreadsheet public URL?

The reference you need is in the sharing settings for the spreadsheet.  If you select the share button and choose `Anyone on the internet can find and view` then copy the link into the variable and change the column names.  Also don't forget **this really import step**, publish this sheet to the web, the option is in the `file` menu.

If by some miracle everything worked correctly you should see your spreadsheet as a map.  As long as you don't touch it after it works nothing bad should happen 😀