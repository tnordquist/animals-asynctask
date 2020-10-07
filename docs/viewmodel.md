---
title: ViewModel
description: ""
menu: ViewModel
order: 20
---

*[MVVM]: Model-View-ViewModel

## MVVM

### Definition

MVVM develops upon the MVC architectural pattern, which enhances separation of concerns. MVVM 
separates the user interface logic from the business (back-end) logic. The aim of the separation of
concerns pattern is to keep the code for user interface concerns simple by keeping it free of other
application logic in order to make the whole app easier to manage.

### MVVM Concerns
* Model  
    The Model deals with the data and business logic of the app. This concern makes its data
    available through what are called observables so that the data could be decoupled from the 
    ViewModel or any other observer/consumer.
* ViewModel  
    The ViewModel prepares and manages data for an Activity or a Fragment. 
    See [ViewModel](https://developer.android.com/reference/androidx/lifecycle/ViewModel)
    A ViewModel is always created in association with a scope (a fragment or an activity) which
    means that it will be retained as long as the scope stays alive. This means that a ViewModel will
    not be destroyed if its owner is destroyed when a configuration change (e.g. rotation) occurs. 
    The new instance of the owner scope will just be re-connected to the existing ViewModel.
    
    The purpose of the ViewModel is to acquire and keep the information that is necessary for an 
    Activity or a Fragment. The Activity or the Fragment observes changes in the 
    ViewModel. ViewModels usually expose this information via LiveData.
    
    ViewModel's only responsibility is to manage the data for the UI. It should never access your 
    view hierarchy or hold a reference back to the Activity or the Fragment, i.e, UI/View.
* View  
    The View observes (or subscribes to) a ViewModel observable in order to get data which is used
    to udate the UI elements as needed.
    
## LiveData
LiveData is an observable data holder. In Animals there was a special LiveData field created as 
```
private MutableLiveData<List<Animal>> animals;
```
in the ViewModel. The corresponding getter method
for the observable `animals` was 
```
public LiveData<List<Animal>> getAnimals() {
  return animals;
}
```  

Posts a task to a main thread to set the given value. `MainViewModel.this.animals.postValue(animals);`

Adding to this, there is also a great benefit in LiveData, LiveData respects the lifecycle state of 
your app components (activities, fragments, services) and handles object life cycle management which 
ensures that LiveData objects do not leak.

Since LiveData respects Android Lifecycle, this means it will not invoke its observer callback 
unless the LiveData host (activity or fragment) is in an active state (received onStart() but did 
not receive onStop() for example). Adding to this, LiveData will also automatically remove the 
observer when the its host receives onDestroy().

The observable nature of LiveData allows components in the app to observe LiveData objects for
changes without the need to create explicit and rigid connections between them. The LiveData 
architecture component decouples the LiveData object producer from the LiveData object consumer.