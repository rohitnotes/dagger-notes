# dagger-notes
Notes of learning Dagger and Dagger-Android

Sources:
  * https://codingwithmitch.com/courses/dagger22-android/what-dagger22/

Source code:
  * https://github.com/mitchtabian/Dagger-Examples

## Dagger Dependencies in an Android Project
```Groovy
dependencies {
    implement 'com.google.dagger:dagger:2.x'
    annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}
```
If using classes in `dagger.android`:
```Groovy
    implement 'com.google.dagger:dagger-android:2.x'
    implement 'com.google.dagger:dagger-android-support:2.x' // if using support libraries
    annotationProcessor 'com.google.dagger:dagger-android-processor:2.x'
```

## What is Dagger2.2?
Dagger is essentially a code generator.

```Java
public class User{
    private int id;
    private String username;
    public User(int id, String username){
        this.id = id;
        this.username = username;
    }
}
```
The integer `id` and string `username` are dependencies of the `User` class; bcuz it depends on them to be created.

Dagger takes this general concept to provide dependencies to higher level of abstraction. This makes your code very **clear**, **concise** and **testable**.

One of the things you can do with dagger is injecting a **Singleton** Pattern for an object.

### Diagrams of the sample app
![diagram-of-the-sample-app.png](diagram-of-the-sample-app.png)

<img src="auth-component-lifetime.png" height="400px">

## Application Component
Creating an Application-level Component class is always required for using Dagger in Android.

For using Dagger we should inherit our `BaseApplication` class from `DaggerApplication`:
```Java
public class BaseApplication extends DaggerApplication {
    \\ This is the function returning the AppComponent
    @Override
    protected AndroidInjector<? extends DaggerApplication> applicationInjector() {
        return DaggerAppComponent.builder().application(this).build();
    }
}
```
We can consider the `BaseApplication` class as the client and the `AppComponent` class as the server.
`AppComponent` is actually an interface extending from `AndroidInjector<BaseApplication>`:
```Java
@Component(
        modules = { // Include some additional dependencies.
                AndroidSupportInjectionModule.class,
        }) // Telling Dagger (code generator): I'm believing this class as a component class
public interface AppComponent extends AndroidInjector<BaseApplication> { // in Android we should extend from AndroidInjector
// Meaning we're injecting BaseApplication class into AppComponent.

    // Overridong the Component.builder() method whose code will be created by Dagger from Builder interface.
    @Component.Builder
    interface Builder{

        // Binds an instace of an object to the component at the time of construction.
        @BindsInstance
        Builder application(Application application);

        AppComponent build();
    }
}

```

In general:
  * Components = Services
  * Activities/Fragments = Clients

You will **ALWAYS** need an AppComponent and you will **ALWAYS** need to put it inside your BaseApplication class.

## Injecting Activities and @ContributesAndroidInjector
