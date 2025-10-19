# Public Transportation System

## Team Members
1. Raul-Anton JAC, Group 1241EA
2. Tudor-Neculai BALBA, Group 1241EA

## Project Description
Our system proposal is a modern integrated solution designed to unify the public transportation experience across Bucharest metropolitan area. Nowadays, there are several applications (STB - InfoTB, BiletTB; Metrorex- website for consulting the timetable and another one for recharging the transportation card; CFR website and app) to plan a single journey. Our project aims to solve this by creating a single, user-friendly platform that provides multimodal route planning and real-time tracking. 

### Core Features
- #### Integrated Multimodal Route Planning
  The system will calculate the optimal route from point A to point B by combining available transportation options (bus, tram, trolleybus, underground metro, regional CFR trains), of course the user can choose from the available options its wishes
- #### All tickets and informations in one place
  Users can buy tickets directly from the app and have all the info's in the app about the tickets, stations, past journeys.
- #### Real Time Vehicle Tracking
  The system will be able to monitor every single transportation option via GPS on STS (ro Serviciul de Telecomunicatii Speciale / en Specials Telecommunication Service) map (maps.stscloud.ro). For this project we will simulate the tracking via the timeschedule available on STB website, CFR dataset and data.gov, alongside with simulation for delays.
- #### Personalization and User accounts
  The registered user can save frequent locations such as work, home and so on, but can also add locations with labels and colours to identify them easily.
- #### Email notifications
  The platform will integrate notifications such as purchases, planned trips, delays, accidents, manintenance for the app. The user can also activate notifications from the web app to be displayed as push notifications.

### Techincal Approach & Challanges
The main challenge is not creating an integreated transport application, it is the real time journey planner. For this, we have two main options: datasets from all public transport operators (STB, Metrorex, CFR) or taking the live locations from Google Routes API (here the only problem is if the public transport operators reports the schedule in real time). 

Using the Google Routes API comes with some limitations in terms of:
-  #### API Cost
Google Routes API is a premium service and allows only 10000 requests on free version.
- #### Data Accuracy
Google's data is generally accurate, but our system depends on the frequency of the GTFS-RT feeds that local operators (STB, metrorex and CFR) provide to Google.
- #### Customization with contraints
We will use their routing algorithms for better accuracy, however they limit the custom routing strategies and we cannot make it even better and friendly to use for our customers/clients

## Design Patterns
To manage this project's complexity and ensure that everything will work properly from both perspectives (development and user) we will follow the following patters:
- #### Strategy pattern
We will use this pattern to encapsulate different routing preferences of the user (e.g. "fastest", "fewest transfers"). Each preference will be a separate "strategy" that configures the request to the Google Routes API accordingly. This way, we can add new routing options in the future without changing the existing logic, avoiding complex and hard-to-maintain code.
- #### Observer pattern
This pattern will be used to "cut" the logic of updating real-time data from the UI components. A central service (Subject) will retrieve data from the API, and the various parts of the interface (map, weather forecast panel - Observers) will subscribe to this service. When new data arrives, all observers are automatically notified and update themselves, creating a reactive and modular system.
- #### Adapter pattern
The Google Routes API returns data in a specific and complex format. To isolate our application from this external structure, we will use an Adapter. This will "translate" the JSON response from Google into our internal objects (e.g. Route, Stop). This way, if Google changes their API format, we will only have to change the logic in the adapter, not the entire application.
- #### Facade pattern
Interacting with the Google Routes API involves several steps: authentication, request construction, response parsing, and error handling. We will create a "facade", a class that simplifies this complexity. The rest of the application will call a single simple method from the facade (e.g. findRoute()), and the facade will internally handle all the complex interactions with the API, keeping the code clean and decoupled.
