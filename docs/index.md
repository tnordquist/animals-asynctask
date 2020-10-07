---
title: AsyncTask
description: ""
menu: 
order: 10
---

## Asynchronous Programming

An asynchronous model allows multiple things to happen at the same time. When a program calls a 
long-running function, it doesn’t block the execution flow, and the program continues to run. When 
the function finishes, the program knows and gets access to the result (if there’s a need for that).
Asynchronous programming was designed for a case like Animals in which there was a Call for a 
resource from a web service that would then be processed and displayed by the UI thread once that 
resource was returned for processing and display by the UI thread - the UI thread could continue to
carry out it's functions while waiting for the resource.

AsyncTask was designed to be a helper class around Thread and Handler and does not constitute a 
generic threading framework. AsyncTasks should ideally be used for short operations (a few seconds 
at the most.)

An asynchronous task is defined by a computation that runs on a background thread and whose result 
is published on the UI thread. An asynchronous task is defined by 3 generic types, called Params, 
Progress and Result, and 4 steps, called onPreExecute, doInBackground, onProgressUpdate and 
onPostExecute.

## Using AsyncTask

AsyncTask must be subclassed to be used. The subclass will override at least one method 
`doInBackground(Params...`), and most often will override a second one `onPostExecute(Result)`.
Animals overrode `onPreExecute()` in addition to the two above.

```
private class RetrieveImageTask extends AsyncTask<Void, Void, List<Animal>> {

    AnimalService animalService;

    @Override
    protected void onPreExecute() {
      // code
    }

    protected List<Animal> doInBackground(Void... voids) {
        // code
        return animals;
    }

    @Override
    protected void onPostExecute(List<Animal> animals) {
      // code
    }
```

## AsyncTask's generic types

The three types used by an asynchronous task are the following:
* Params, the type of the parameters sent to the task upon execution.
    No such input was required for Animals to carry out the task of fetching a resource from Animals
    web service.
* Progress, the type of the progress units published during the background computation.
    An example would be Integer units for a countdown display. No progress updating was carried out 
    in the case of Animals.
* Result, the type of the result of the background computation.
    `List<Animal>` was the type of result returned.

Void marked the types that were not used:

`private class RetrieveImageTask extends AsyncTask<Void, Void, List<Animal>> { ... }`
 
## AsyncTask has 4 steps:

* `onPreExecute()` invoked on the UI thread before the task is executed. This step is normally used to 
setup the task, for instance by showing a progress bar in the user interface. Animals set up Gson
and Retrofit here in order to initialize an AnimalService instance.
* `doInBackground(Params...)` invoked on the background thread immediately after `onPreExecute()` 
finishes executing. This step is used to perform a background computation that can take a long time. 
The parameters of the asynchronous task are passed to this step: `List<Animal>` in the case of
Animals. The result, again `List<Animal>` in the case of Animals, of the computation must be returned 
by this step and will be passed back to the last step. This step can also use 
`publishProgress(Progress...)` to publish one or more units of progress. These values are published 
on the UI thread, in the `onProgressUpdate(Progress...)` step.
* `onProgressUpdate(Progress...)` invoked on the UI thread after a call to 
`publishProgress(Progress...)`. The timing of the execution is undefined. This method is used to 
display any form of progress in the user interface while the background computation is still 
executing. For instance, it can be used to animate a progress bar or show logs in a text field.
* `onPostExecute(Result)` invoked on the UI thread after the background computation finishes. The 
background computation passes the result to this step as a parameter. `doInBackground(Params...)`
passed `List<Animal>` in the case of Animals.

## Cancelling a task
A task can be cancelled at any time by invoking cancel(boolean). Invoking this method will cause 
subsequent calls to isCancelled() to return true. After invoking this method, 
onCancelled(java.lang.Object), instead of onPostExecute(java.lang.Object) will be invoked after 
doInBackground(java.lang.Object[]) returns. To ensure that a task is cancelled as quickly as 
possible, you should always check the return value of isCancelled() periodically from 
doInBackground(java.lang.Object[]), if possible (inside a loop for instance.)

## Threading rules

There are a few threading rules that must be followed for this class to work properly:

* The AsyncTask class must be loaded on the UI thread. This is done automatically as of 
Build.VERSION_CODES.JELLY_BEAN.
* The task instance must be created on the UI thread.execute(Params...) must be invoked on the UI 
thread.
* Do not call onPreExecute(), onPostExecute(Result), doInBackground(Params...), 
onProgressUpdate(Progress...) manually.
* The task can be executed only once (an exception will be thrown if a second execution is 
attempted.)
