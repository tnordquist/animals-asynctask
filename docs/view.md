---
title: View
description: ""
menu: View
order: 30
---

`onCreateView()` is the Fragment equivalent of `onCreate()` for Activities and runs during the View 
creation. `onViewCreated()` runs after the View has been created.

It's better to do any assignment of subviews to fields in onViewCreated. This is because the 
framework does an automatic null check for you to ensure that your Fragment's view hierarchy has 
been created and inflated (if using an XML layout file) properly. 
The documentation for [Fragment.onCreateView](https://developer.android.com/reference/kotlin/androidx/fragment/app/Fragment?hl=en#oncreateview) 
states:

>It is recommended to only inflate the layout in `onCreateView()` and move logic that operates on the 
>returned View to `onViewCreated(View, Bundle)`.


`onCreateView()` is called immediately after `onCreateView` (the method you initialize and create all 
your objects, including your TextView), so it's not a matter of performance.

From the developer site:

onViewCreated(View view, Bundle savedInstanceState)
Called immediately after onCreateView(LayoutInflater, ViewGroup, Bundle) has returned, but before 
any saved state has been restored in to the view. This gives subclasses a chance to initialize 
themselves once they know their view hierarchy has been completely created. The fragment's view 
hierarchy is not however attached to its parent at this point.

ViewModelProvider (belongs to Maven artifact android.arch.lifecycle:viewmodel) is a utility
class that provides ViewModels for a scope. Source: [ViewModelProvider](https://developer.android.com/reference/androidx/lifecycle/ViewModelProvider#summary)