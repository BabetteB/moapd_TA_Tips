# A Note on Folder Structure and Application Patterns

While it is not mandatory - and I know that Fabricio gave you freedom to implement the folder structure as you wished, there are some reflections that I think could benifit you, that are being missed here:

Often, when building really any system, you want your folder structure to reflect the pattern(s) that are applied to your system. And, since Mode-View-Controller (MVC) is one of the sole patterns that you have been taught - it is not surprising that your folder structure reflects that pattern; Which would be correct if that where the pattern we are implementing - however, it is "unfortuanately" not. The app we are implementing, throughout the course, follows a Model-View-ViewModel (MVVM) pattern, which is a very common pattern in (android) applications.

## What is MVVM

The Model-View-ViewModel pattern is an architectural approach that separates an application into three distinct parts. In mobile applications, the Model is in charge of handling data and the logic for how that data is changed or retrieved (e.g. `Event`), while the View is responsible for displaying the user interface and register user interaction (hence, all activities and fragments). The ViewModel sits in the middle, connecting the Model and the View by preparing data for display and receiving user actions without directly touching the View’s layout code.
This means changes in the data automatically show up in the View, and user actions automatically flow back to the logic. Because the ViewModel takes care of this communication, the View can stay simpler and only worry about drawing the screen and sending input events, which makes the code easier to test, maintain, and read.

MVVM is commonly preferred in mobile applications because it simplifies how data is tied to what users see on the screen. Instead of writing a lot of code to manually connect changes in data to the user interface, the ViewModel takes care of making sure that whenever the data changes, the screen updates automatically - and user actions are sent back to the ViewModel without the View needing to handle much of the application’s logic.

When you start working with more data and databases, you will implement your first ViewModel in exercise set for week 06, and then this pattern will start making more sense :)

## Differences between MVC and MVVM

The main difference between MVC and MVVM is that in MVC, the Controller tends to handle both updating the View and dealing with the Model, so the View and Controller end up being more tightly connected. In MVVM, the ViewModel takes on most of that workload by exposing all the data the View needs, so the View only focuses on how things look. When the data in the ViewModel changes, the View updates automatically, reducing the need to manually link each change in the Model to every piece of the user interface.
