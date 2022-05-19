### MVC 
* Controller — requests, handles request flow, never handles data logic
* Model — handles data logic, interacts with database
* View — handles data presentation, dynamically rendered

Controller sends requests to model, model interacts with database 
and returns the data to controller. 
Controller sends it to View which decides how to display the data.
Then View sends the way data should be presented to controller 
and the controller displays it to user.

![mvc](https://user-images.githubusercontent.com/86350117/169338140-f9b18e32-303d-4815-b54b-a3d19e2d5156.png)
