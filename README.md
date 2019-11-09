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



