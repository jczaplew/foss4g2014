# Intro

  - Hello! I'm John Czaplewski. I am a web application developer in the Geoscience Department at the University of Wisconsin - Madison, but as is the case with many of you, my work often centers around the storage and representation of spatial data. I am also presenting with Rashauna Mead of the Applied Population Lab at the University of Wisconsin - Madison, and Rich Donohue of the University of Kentucky. 

*change*

  - Today I'll be talking about adaptive map experiences, and how we as a community can not only improve the products we create, but hopefully also contribute something back to the web development community. I should also note here, that, we are not proposing any specific solutions, but rather would like to start a conversation and get some ideas from you about where to take this idea of adaptive mapping. 

  - But first, because there really isn't a definition of what an adapative map is, let's start by getting an idea of what adaptive design is, how it differs from responsive, and why it matters to mapping.

*change*

  - Responsive is always adaptive, but adaptive is not always responsive. Rather, responsive is just a piece of the adaptive puzzle. Responsive web design is largely about optimizing the layout of a page for the screen size of the user and delivering appropriately sized media. You may have heard of, or used, frameworks that make this less painful, such as Bootstrap and Foundation. 

*change*

  - Adaptive design, on the other hand, deals much more with altering the purpose, context, and functionality of a website in order to meet the needs of users. Of course, optimzing the layout is an important part of achieving this goal, but it also can go so far as to deliver an entirely different experience, depending on the screen size or device of the user. 

*change*

  - A great non-map example of adaptive web design is the Lufthansa website. As you can see, the desktop site is a fairly normal airline website - and like most, it has an exploratory focus. The user can easily explore where in the world Lufthansa flies, find flight specials, and book travel. It also has links to other common tools, such as flight check-in and tracking. Now look at what happens when you go to the same URL on a phone - 

*change*

  - Notice that not only does the layout change, the obvious functionality and purpose does too. On a mobile device, the first thing we see is a travel update - impending pilot strikes, which is something a traveler might want to know about. The next two things we see are "Flight status" and "Check-in", probably the two most common things someone may pull out there phone to do or check while traveling. Of course the functionality of the desktop site is also there, but the focus is on common tasks handled via a phone while traveling as opposed to exploring travel options. 

  - This example sums up adaptive design fairly well - not only does it make the content fit the screen, but it changes the focus and context of the content in an attempt to more readily meet the users' needs

*change*

# User location for adaptive mapping
  - The current state of adaptive mapping is characterized by many independent pieces that enable the creation of a robust adaptive map. Perhaps the factor that can influence an adaptive map the most is the location of the user, and the things that can be interpolated from that information. 

*change*

  - Tools like the Open Street Map Overpass API can leverage the location of a user to assist in decision making. If you are not already familiar with the OSM Overpass API, it allows you to query vector objects from the OSM database, which could in turn be used for various adaptive purposes. 

*change*

  - For example, I coded up an example that uses this API to roughly determine a road density around a given location, which is used for deciding what kind of tiles the user at this location might want. For example, if I load a map and am obviously in a city, chances are a road map is what will be the most useful for me. Likewise, if I load a map and one, or only a few, roads are found near me, perhaps a topographic map might be more useful. (you may not want these, but we can at least guess at your next action and provide it)

*change*

  - You can also imagine extending this concept of adaptive content for specific mobile applications, like an app that is used while driving. Rashauna brought up a great example the other day - you're driving around in an unfamiliar, rural area, and presumably you open a map to figure out where you are in relation to some meaningful landmark - most likely a village or city. Usually when you open the app, you are zoomed in to an unusably large scale, and then in order to figure out where you are, a few readjustments on a slow connection are required.

*change*

  - Ideally, we could easily solve this issue by loading the map to fit both your current location and that of the nearest city or village.

*change*

# Adaptive representation
  - Another type of adaptive mapping deals with changing attributes of the map, say the projection or the type of symbology, to meet the immediate needs of the user. 

*change*

  - Of course, one of the first examples that comes to mind is Berhard Jenny's brilliant Adaptive Composite Map Projection, which dynamically changes the projection of the map based on the scale and center. 

*change*

  - This is one of the coolest examples of an adaptive map today - instead of just changing size to fit the screen, *change* it dynamically picks the optimal projection for whatever slice of earth you are looking at. *change* Although it is an amazing proof-of-concept, it has not been implemented within popular mapping APIs (save a few attempts to accomplish the same thing with D3, but those have not been widely adapted). It would be incredible to see this concept applied everywhere in the future - and in fact the most recent version of Google Maps takes a scaled-down version of this approach 

*change*

*change*

*change*

*change*

  - Another great example of adaptive mapping is the dynamic hill shading from Mapbox. Using vector tiles, they have developed methods that allow you to dynamically change the azimuth, altitude, depth, shadow, and highlight of the lighting on your terrain in the browser. 

*change*

  - Although I have yet to see an example of this in the wild, the technology is incredibly promising and I'm sure will change the way we think about adaptive maps. 

*change*

  - Adaptive representation can also apply of course to the map symbology. The most common example is changing the scale used for a proportional symbol map as the user zooms in and out. In most mapping APIs, by default the map will often look very crowded at small scales, and the symbols will be very hard to interact with at large scales 

  - Rich coded up an example to show how this can work

*change*

  - This example demonstrates both an application of what we're suggesting adaptive cartography is capable of, as well as new avenues of thinking when we bring traditional cartographic practices into a web environment. While Leaflet's CircleMarker provides a fixed radius circle that allows a proportional symbol map to work across different zoom levels, these often result in less than ideal sizes for visual comparison when zoomed into larger cartographic scales. The Leaflet Circle maintains a constant radius in terms of geographic distance, but this is even less useful for a proportional symbol map [as demoed in the video]. However, by applying some conditional JavaScript statements, we can customize the size of proportional symbols at varying zoom levels to maximize the effectiveness of the visual encoding. 

*change*

  - Another simple tweak we could make to many maps would be to change the minimum symbol size if we know the user is using a touch interface. For example, in a mouse/keyboard setting, it might be more aesthetically pleasing to have smaller symbols and depend on mouseover interactions, but in a touch environment these small symbols, while perhaps more attractive, can produce a very frustrating experience for those of us with wide fingers. In these situations, it might be useful to proactively make sure all symbols are a minimum of approximately 44px by 44px, the minimum recommended touch target according to Apple, or 48dp according to Google.

*change*

# Adaptive server side
  - Adaptive cartography is not necesarilly only a front-end problem - it can also be helped along by the server side. 

*change*

  - One example is the Paleobiology Database API. When requesting fossil collections, you can specify three different levels of point clustering - 6 degree bins, *change* 2 degree bins, *change* half degree bins, *change* and of course, no binning. First of all, the purpose of data aggregation, and in this case clusters, whether they be using a server side method or client side, such as Leaflet-markercluster, is to generalize the data. To represent a generalization of the data, we do not need to know where, or even what, each data point is. 

  - The advantage of server-side clusters is that, instead of constantly requesting way more points than needed to give the user an idea of what the data looks like, and then clustering them client-side, we can directly request an appropriate clustering level and return far less data. The biggest advantage of this approach is the savings in bandwidth, but also a savings in client side processing. Often times when you are clustering a very rich dataset, the exact location of each point is not very valuable, and the data attached to each point will almost never be retrieved. With this approach, if you were to click on a cluster, you can make another small request to the API asking what collections it contains. If you'd like to know more about one of those collections, you can make one more small request. While this does produce many more requests to the server, they are always way smaller than what they would be otherwise. 

*change*

# Other adaptive
  - So far all the examples of adaptive maps have dealt with rather complex scenarios, but many can be much simpler. 

*change*

  - Brad Frost, a web designer, gave the concept of adaptive maps some thought, and argued that for mobile devices, a static map as default was preferred, and coded up an example in which small screen sizes were given a static map, that, when clicked, would redirect the user to a full screen interactive map. With large screen sizes, an interactive map was simply embedded. While I'm not sure I totally agree with this approach (explain?), it is a great example of people realizing that adaptive maps are more than making them fit the right screen size. 

  - (Unsure about this paragraph...) Along similar lines, I have seen interactive maps that cover the width of a page, but contain content above and below that are meant to be scrolled to. Occasionally, while the user is scrolling down the page, the scroll will stop and all of a sudden the map will have zoomed in 8 levels. While this is mostly just an inconvenience, it can be problematic on a touch interface when the map is too large and there are no other elements to touch that enable you to scroll past the map. In those instances, your user is just stuck on the map and never knows there is other content

*change*

# Full adaptive example
  - While we have taken a look at many disperate bits of technology that can be used to create adaptive maps, we haven't really seen them come together in a single product. Rashauna has created an example protest map application that does a great job of bringing all of these concepts and technologies together. Protests are a great use case for an adaptive map, as it is easy to imagine two very different ways this sort of application could be used - either for exploratory purposes (for example, you're 1,000 miles away but want to see what is happening on the ground), or for immediate, on-the-go reporting and tracking. I'll walk you through both use cases...

  - On the left side we have the desktop version, and as was the case with the Lufthansa example, it has an exploratory focus. The user is greeted with a global view and query tools, which allow you to search by protest type, date, or a shape that you can draw on the map.

  - On the right side you can see the mobile view - instead of starting with a global extent, it shows a map of your current location, as you are more likely to want to know where protests are happening around you. The interface is much simpler as well, with the zoom buttoms removed and large icons to tap. It is also much easier to search for recent protests in the last 24 hours. It also provides a different interface for adding a protest, which is an immediate event and more suited to mobile use.

  - This demonstrates a common theme with adaptive design and adaptive mapping - mobile usually has more immediate, environmental needs that need to be addressed, and thus requires a tailored experience.

*change*

# Where do we go 
  - Where does adaptive mapping go from here? Hopefully some of the maps and ideas we shared will inspire you to go make your own. 

  - Many of the components discussed could be packaged up in to reusable modules for ease of use, such as proper scaling of symbology or the adaptive composite map projection. While this might make some tasks easier, it comes at the cost of maintaining additional libraries and attempting to reduce adaptive mapping down to one-size-fits-all solutions. 

*change*


*change*

  - Doneski 

