# Implementing Android Jetpack Navigation

The `Android Jetpack Navigation` makes implementing the navigation between fragments, much easier. However, it also feels a little counterintuitive, and might need a little walkthrough. So let’s do a walkthrough.

You need to modify the following files:

- `build.gradle`: include the Jetpack Navigation packages
- `layout/main_activity.xml`: the activity that holds `layout/content_main.xml`
- `layout/content_main.xml`: that holds the container that shows the fragments, based on which button is pressed in the `menu/bottom_navigation.xml`
- `menu/bottom_navigation.xml`: that defines the buttons that the user presses to navigate between fragments
- `navigation/nav_graph.xml`: where we define the fragments that the `NavController` should be able to change between
- `MainActivity.kt`: the file where we instantiate the `NavController` and link the `bottom_navigation` to it.

## `build.gradle`

Remember to include the packages that gives the `NavigationController` it's functionality. Hence, your `build.gradle` should include:

```groovy
implementation "androidx.navigation:navigation-fragment-ktx:<version>"
implementation "androidx.navigation:navigation-ui-ktx:<version>"
```

## `layout/main_activity.xml`

In the `main_activity.xml` we should hold the elements that should be visible all throughout the activities lifecycle. E.g. while the `MainActivity` is in view we want to see:

- the `top_app_bar`: the top bar displaying the application name and buttons such as `login` or `logout`
- the `bottom_navigation`: the bar in the bottom, that we use to hold the navigation buttons
- the `FragmentContainerView` but we will wrap that with `content_main`.

*Note: with this setup, you will most likely experience that the view is hidden behind the top and bottom bars. I have written a guide that fixes just that here: [A quick note on Layout](https://github.com/BabetteB/moapd_TA_Tips/blob/main/A%20quick%20note%20on%20Layout/layout_and_navigationbars.md)*.

## `layout/content_main.xml`

In `content_main` we include the `FragmentContainerView`. The `FragmentContainerView` needs:

- an `id` (always in snake_casing !)
- a reference to the `androidx` object (as it has extra functionality that is abstracted away from you)
- setting `defaultNavHost` as true, marking the `FragmentContainerView` as the primary navigation element
- specifying the where the navigator can find the `navGraph`

Hence, `content_main.xml` should contain:

```xml
<androidx.fragment.app.FragmentContainerView
    android:id="@+id/fragment_container_view"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:defaultNavHost="true"
    app:navGraph="@navigation/nav_graph"
    .../>
```

## `menu/bottom_navigation.xml`

In the `bottom_navigation` menu we want to have a button for every item we want displayed in the menu in the bottom of the screen, so we solely need to include `id`'s of the item, a `title` of the menu button and an icon. Hence we have:

```xml
<menu ...>
    <item
        android:id="@+id/fragment_timeline"
        android:title="@string/timeline_title"
        android:icon="@drawable/baseline_calendar_view_day_24"
        .../>
    ...
    <item
        android:id="@+id/fragment_calendar"
        android:title="@string/calendar_title" 
        android:icon="@drawable/baseline_calendar_view_day_24"
        .../>
</menu>
```

## `navigation/nav_graph.xml`

In the `nav_graph` we want to include fragments of the program that we want to be able to navigate between. For now we will just include the fragments that we want the `NavController` to manage for us when the user clicks between the fragments in the bottom navigation bar - however, you could also include fragments that you want to be able to navigate to through other means [^1].

Please note that the `<action .../>` property is not needed for fragments that will be managed by the `NavController`.

For each fragment that we set the following properties:

- `id`: and here it is important that it matches the `id` that you sat for the fragment in `menu/bottom_navigation.xml`
- `label`: the title of the fragment, this will display the title of the fragment in the `top_app_bar`
- `android:name`: here you have to refer to the package of the fragment e.g. `dk.itu.moapd.copenhagenbuzz.<YOUR ITU INITIALS>.fragments.TimelineFragments`
- `tools:layout`: specify the layout you want the `NavController` to inflate.

Hence, you'd want the `nav_graph` to look something like this:

```xml
<navigation 
    ...
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/fragment_timeline">

    <fragment
        android:id="@+id/fragment_timeline"
        tools:layout="@layout/fragment_timeline"
        android:name="dk.itu.moapd.copenhagenbuzz.<YOUR ITU INITIALS>.activities.TimelineFragment"
        android:label="@string/timeline_title" />
    ...
    <fragment
        android:id="@+id/fragment_calendar"
        tools:layout="@layout/fragment_calendar"
        android:name="dk.itu.moapd.copenhagenbuzz.<YOUR ITU INITIALS>.activities.CalendarFragment"
        android:label="@string/timeline_title" />
</navigation>
```

[^1] : e.g. if we didn't have a bottom navigation menu button for `fragment_add_event` - and instead had a button in `fragment_timeline` that we wanted to be able to click to navigate to `fragment_add_event`: we would still include `fragment_add_event` in the `nav_graph` but then we would manage the navigation with the `<action .../>` property.

## `MainActivity.kt`

Lastly, we will need to assign and setup the `NavigationController` in `MainActivity.kt` in the `onCreate` method as such:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    ...
    val navHostFragment = supportFragmentManager
        .findFragmentById(R.id.fragment_container_view) as NavHostFragment

    val navController = navHostFragment.navController

    bottomNavigation.setupWithNavController(navController)
    ...
}
```

And that is all you need to implement the `Android Jetpack Navigation`.
