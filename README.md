<h1>Android Advanced Interview Questions</h1>

<h2>Kotlin and Object-Oriented Concepts</h2>
<p>What is the <code>open</code> keyword in Kotlin?</p>
<p>What is the difference between <code>lateinit</code> and <code>lazy</code>?</p>
<p>What is the <code>init</code> block in Kotlin?</p>
<p>What is a Companion Object in Kotlin?</p>
<p>What is the difference between an <code>Object</code> and a <code>Companion Object</code>?</p>
<p>How to write custom extensions in Kotlin?</p>
<p>What are Scope Functions in Kotlin?</p>
<p>What is the difference between <code>let</code> and <code>also</code>?</p>
<p>What is the difference between an <code>Enum Class</code> and a <code>Sealed Class</code>?</p>
<p>What happens when a class implements multiple interfaces with the same method?</p>

<h2>Advanced Kotlin Features</h2>
<p>What is <code>@JvmStatic</code>, <code>@JvmOverloads</code>, and <code>@JvmField</code> in Kotlin?</p>
<p>Can we call a method in a constant declaration?</p>
<p>How to find duplicates in an array in Kotlin?</p>

<h2>Kotlin Coroutines</h2>
<p>What is a Coroutine?</p>
<p>How is a Coroutine different from a Thread?</p>
<p>Why are Coroutines more powerful than Threads?</p>
<p>What is the difference between <code>launch</code> and <code>async</code>?</p>
<p>What are Scopes in Kotlin Coroutines?</p>
<p>What are Dispatchers in Kotlin Coroutines?</p>
<p>Explain the different types of Dispatchers.</p>
<p>What is <code>runBlocking</code> in Coroutines?</p>
<p>What is <code>withContext</code> in Kotlin Coroutines?</p>
<p>What is the difference between <code>coroutineScope</code> and <code>supervisorScope</code>?</p>
<p>Why do we declare a function as a <code>suspend</code> function?</p>
<p>What is <code>flow</code> in Kotlin Coroutines?</p>
<p>Explain the internal working of Coroutines.</p>

<h2>Multithreading & Performance</h2>
<p>What is the difference between Threads and Coroutines?</p>
<p>Why is multiple inheritance not possible in Java? Explain with an example.</p>
<p>What are the key differences between Plugins and Dependencies in Android?</p>

<h2>Android Components & Lifecycle</h2>
<p>Explain the Activity Lifecycle when transitioning from one Activity to another.</p>
<p>Explain the Fragment Lifecycle when replacing one Fragment with another.</p>
<p>Explain <code>ViewPager</code> and how it stores multiple Fragments.</p>

<h2>Android Concepts</h2>
<p>How can two distinct Android apps interact?</p>
<p>What is a <code>ContentProvider</code>, and what is it typically used for?</p>
<p>Is there any other way to handle screen rotation without recreating an Activity?</p>
<p>What is <code>clearTextTraffic</code> in Android?</p>
<p>How can you handle layouts for foldable phones?</p>
<p>How can you protect an app from screen sharing?</p>
<p>Is it possible to run an Android app in multiple processes?</p>
<p>What is <code>AIDL</code>? What are the steps to create a bound service using <code>AIDL</code>?</p>

<h2>Security & Encryption</h2>
<p>What is SSL Pinning?</p>
<p>Explain AES Encryption.</p>

<h2>String Handling in Java/Kotlin</h2>
<p>Why is <code>String</code> immutable in Java/Kotlin?</p>
<p>What is the difference between <code>StringBuilder</code> and <code>StringBuffer</code>? Explain with an example.</p>

<h2>ViewBinding & DataBinding</h2>
<p>What are <code>ViewBinding</code> and <code>DataBinding</code>? How do they differ?</p>


<h1>What is Coroutine?</h1> -
coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive. 

<h1>What is difference between launch and async ?</h1>
Both launch and async are the functions in Kotlin to start the Coroutines.

launch{}
async{}
The difference is that the launch{} returns a Job and does not carry any resulting value whereas <br>
the async{} returns an instance of Deferred<T>, which has an await() function that returns the result of the coroutine like we have future in Java in which we do future.get() to get the result.

In other words:

<b>launch:</b> fire and forget. <br>
<b>async:</b> perform a task and return a result. <br>
Let's take an example to learn launch vs async.

We can use the launch as below:

```
val job = GlobalScope.launch(Dispatchers.Default) {
    // do something and do not return result
}
```

It returns a job object which we can use to get a job's status or to cancel it.

In the above example of launch, we have to do something and NOT return the result back.

But when we need the result back, we need to use the async.

```
val deferredJob = GlobalScope.async(Dispatchers.Default) {
    // do something and return result, for example 10 as a result
    return@async 10
}
val result = deferredJob.await() // result = 10
```

Here, we get the result using the await().

In async also, we can use the Deferred job object to get a job's status or to cancel it. <br>

Note: I have used GlobalScope for quick examples, we should avoid using it at all costs. In an Android project, we should use custom scopes based on our usecase such as lifecycleScope, viewModelScope and etc. <br>

Another difference between launch and async is in terms of exception handling. <br>

If any exception comes inside the launch block, it crashes the application if we have not handled it. <br>

However, if any exception comes inside the async block, it is stored inside the resulting Deferred and is not delivered anywhere else, it will get silently dropped unless we handle it. <br>

Let's understand this difference with code examples. <br>

Suppose we have a function that does something and throws an exception: <br>

```
private fun doSomethingAndThrowException() {
    throw Exception("Some Exception")
}
```
Now using it with the launch: <br>

```
GlobalScope.launch {
    doSomethingAndThrowException()
}
```
It will CRASH the application as expected.

We can handle it as below:

```
GlobalScope.launch {
    try {
        doSomethingAndThrowException()
    } catch (e: Exception) {
        // handle exception
    }
}
```
Now, the exception will come inside the catch block and we can handle it.

And now, using it with the async:

```
GlobalScope.async {
    doSomethingAndThrowException()
}
```
The application will NOT crash. The exception will get dropped silently.

Again, we can handle it as below:

```
GlobalScope.async {
    try {
        doSomethingAndThrowException()
    } catch (e: Exception) {
        // handle exception
    }
}
```
Now, the exception will come inside the catch block and we can handle it.

Let me tabulate the difference between launch and async.

<b>Launch</b><br>
Fire and forget. <br>
launch{} returns a Job and does not carry any resulting value.	<br>
If any exception comes inside the launch block, it crashes the application if we have not handled it.	<br>

<b>Async</b><br>
Perform a task and return a result.<br>
async{} returns an instance of Deferred<T>, which has an await() function that returns the result of the coroutine.<br>
If any exception comes inside the async block, it is stored inside the resulting Deferred and is not delivered anywhere else, it will get silently dropped unless we handle it.<br>

<h1>Scopes in Kotlin Coroutines ? </h1> 
<h3>Global Scope : </h3>
Global Scope is one of the ways by which coroutines are launched. When Coroutines are launched within the global scope, they live long as the application does. If the coroutines finish it’s a job, it will be destroyed and will not keep alive until the application dies, but let’s imagine a situation when the coroutines has some work or instruction left to do, and suddenly we end the application, then the coroutines will also die, as the maximum lifetime of the coroutine is equal to the lifetime of the application. Coroutines launched in the global scope will be launched in a separate thread. Below is the example which shows that the in global scope coroutines are launched in a separate thread.

```
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
	val TAG = "Main Activity"
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)
		GlobalScope.launch {
			Log.d(TAG, Thread.currentThread().name.toString())
		}
		Log.d("Outside Global Scope", Thread.currentThread().name.toString())
	}
}

```

<h3>LifeCycle Scope :</h3>
The lifecycle scope is the same as the global scope, but the only difference is that when we use the lifecycle scope, all the coroutines launched within the activity also dies when the activity dies. It is beneficial as our coroutines will not keep running even after our activity dies. In order to implement the lifecycle scope within our project just launch the coroutine in lifecycle scope instead of global scope, ie just change the global scope to lifecycle scope in the main activity within which the infinite loop is running. All the code will remain the same except for some changes in the code of the main activity as mentioned above.

```
// program to show how lifecycle scope works

import android.content.Intent
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.lifecycleScope
import kotlinx.android.synthetic.main.activity_main.*
import kotlinx.coroutines.GlobalScope
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch

const val TAG = "Main Activity"

class MainActivity : AppCompatActivity() {
	override fun onCreate(savedInstanceState: Bundle?) {
		super.onCreate(savedInstanceState)
		setContentView(R.layout.activity_main)

		btnStartActivity.setOnClickListener {
			// launching the coroutine in the lifecycle scope
			lifecycleScope.launch {
				while (true) {
					delay(1000L)
					Log.d(TAG, "Still Running..")
				}
			}

			GlobalScope.launch {
				delay(5000L)
				val intent = Intent(this@MainActivity, SecondActivity::class.java)
				startActivity(intent)
				finish()
			}
		}
	}
}

```
<h3> ViewModel Scope </h3>
It is also the same as the lifecycle scope, only difference is that the coroutine in this scope will live as long the view model is alive. ViewModel is a class that manages and stores the UI-related data by following the principles of the lifecycle system in android

<h1>Dispatchers in Kotlin Coroutines </h1>
It is known that coroutines are always started in a specific context, and that context describes in which threads the coroutine will be started in. In general, we can start the coroutine using GlobalScope without passing any parameters to it, this is done when we are not specifying the thread in which the coroutine should be launch. This method does not give us much control over it, as our coroutine can be launched in any thread available, due to which it is not possible to predict the thread in which our coroutines have been launched.

```
class MainActivity : AppCompatActivity() { 
	override fun onCreate(savedInstanceState: Bundle?) { 
		super.onCreate(savedInstanceState) 
		setContentView(R.layout.activity_main) 

		// coroutine launched in GlobalScope 
		GlobalScope.launch() { 
		Log.i("Inside Global Scope ",Thread.currentThread().name.toString()) 
			// getting the name of thread in 
			// which our coroutine has been launched 
		} 

		Log.i("Main Activity ",Thread.currentThread().name.toString()) 
	} 
}
```
We can see that the thread in which the coroutine is launched cannot be predicted, sometimes it is DefaultDispatcher-worker-1, or DefaultDispatcher-worker-2 or DefaultDispatcher-worker-3.

<h3>How Dispatchers solve the above problem? </h3>
Dispatchers help coroutines in deciding the thread on which the work has to be done. Dispatchers are passed as the arguments to the GlobalScope by mentioning which type of dispatchers we can use depending on the work that we want the coroutine to do. 

<h3>Types of Dispatchers</h3>
There are majorly 4 types of Dispatchers.<br>

<h5>Main  Dispatcher</h5>
<h5>IO Dispatcher</h5>
<h5>Default Dispatcher</h5>
<h5>Unconfined Dispatcher</h5>

<h3>Main Dispatcher: </h3>
It starts the coroutine in the main thread. It is mostly used when we need to perform the UI operations within the coroutine, as UI can only be changed from the main thread(also called the UI thread).

<h3>IO Dispatcher:</h3>
It starts the coroutine in the IO thread, it is used to perform all the data operations such as networking, reading, or writing from the database, reading, or writing to the files eg: Fetching data from the database is an IO operation, which is done on the IO thread

```
GlobalScope.launch(Dispatchers.IO) { 
	Log.i("Inside IO dispatcher ",Thread.currentThread().name.toString()) 
		// getting the name of thread in which 
			// our coroutine has been launched 
	} 
	Log.i("Main Activity ",Thread.currentThread().name.toString())
```

<h3> Default Dispatcher: </h3>

It starts the coroutine in the Default Thread. We should choose this when we are planning to do Complex and long-running calculations, which can block the main thread and freeze the UI eg: Suppose we need to do the 10,000 calculations and we are doing all these calculations on the UI thread ie main thread, and if we wait for the result or 10,000 calculations, till that time our main thread would be blocked, and our UI will be frozen, leading to poor user experience. So in this case we need to use the Default Thread. The default dispatcher that is used when coroutines are launched in GlobalScope is represented by Dispatchers. Default and uses a shared background pool of threads, so launch(Dispatchers.Default) { … } uses the same dispatcher as GlobalScope.launch { … }.

```
GlobalScope.launch(Dispatchers.Default) { 
		Log.i("Inside Default dispatcher ",Thread.currentThread().name.toString()) 
			// getting the name of thread in which 
			// our coroutine has been launched 
		} 
		Log.i("Main Activity ",Thread.currentThread().name.toString())
```

<h3>Unconfined Dispatcher: </h3>

As the name suggests unconfined dispatcher is not confined to any specific thread. It executes the initial continuation of a coroutine in the current call-frame and lets the coroutine resume in whatever thread that is used by the corresponding suspending function, without mandating any specific threading policy. 

<h1> what is runblocking in coroutine </h1>
runBlocking is a coroutine builder in Kotlin used to create a new coroutine and block the current thread until the coroutine completes. It is typically used in main functions or in tests to bridge the gap between synchronous and asynchronous code. <br>

Here's a simple example of how runBlocking is used: <br>
 <br>
```
import kotlinx.coroutines.*

fun main() {
    runBlocking {
        // This code will be executed in a coroutine
        delay(1000) // Delay the coroutine for 1 second (non-blocking)
        println("Coroutine completed")
    }
    // The main thread will be blocked until the coroutine inside runBlocking completes
    println("Main thread completed")
}
```
 <br>
In this example, runBlocking creates a coroutine that delays for 1 second using delay, which suspends the coroutine without blocking the main thread. The main thread is then blocked until the coroutine completes, and "Coroutine completed" is printed to the console. Finally, "Main thread completed" is printed once the main thread is unblocked. <br>

It's important to note that using runBlocking to block the main thread is generally discouraged in production code, as it can lead to performance issues and should be used sparingly. In most cases, it's better to use non-blocking constructs like launch, async, or suspendCoroutine to handle asynchronous operations in a more efficient manner. <br>


<h1>why we declare function as suspend function in coruotine </h1>
we declare a function as a suspend function when it needs to perform long-running or asynchronous operations inside a coroutine. By marking a function as suspend, we are telling the compiler that this function can be safely called from a coroutine and that it may suspend its execution at some point.  <br>

Here's why we use suspend functions in coroutines: <br>

<h3>Asynchronous Operations: </h3>
Suspend functions allow us to perform asynchronous operations such as network requests, disk I/O, or database queries without blocking the main thread or the coroutine in which they are called. <br>

<h3>Sequential Code: </h3>
Suspend functions can be called in a sequential, imperative style, even though they may perform asynchronous operations internally. This makes the code easier to read and understand compared to callback-based or reactive code. <br>

<h3>Cancellation Support: </h3>
Suspend functions can be cancelled when their coroutine is cancelled. This allows for proper resource management and cleanup when a coroutine is no longer needed. <br>

<h3>Exception Handling: </h3>
Suspend functions can throw exceptions like regular functions, and these exceptions can be caught using try-catch blocks within the coroutine. This makes error handling more straightforward compared to other asynchronous patterns. <br>

<h3> Coroutine Context: </h3>
Suspend functions can access the coroutine's context, including its scope, dispatcher, and other contextual information. This allows for fine-grained control over the execution of the suspend function. <br>

Overall, suspend functions are a key feature of Kotlin coroutines that enable us to write asynchronous code in a more natural and sequential way, making our code more readable, maintainable, and efficient. <br>


<h1>• How can two distinct Android apps interact? </h1>

Intents: <br>
Apps can communicate with each other using intents. An app can send an intent to request an action from another app, such as opening a specific activity or sharing data. The receiving app can then handle the intent and respond accordingly. <br>

Content Providers: <br>
Content providers allow apps to share data with each other. An app can define a content provider to make its data accessible to other apps. Other apps can then query or modify this data using content resolver methods. <br>

Broadcasts:  <br>
Apps can send broadcast messages to notify other apps of events or trigger actions. Apps can register to receive specific broadcasts using intent filters. <br>

Bound Services:  <br>
Apps can bind to a service provided by another app to interact with it. This allows for more complex and ongoing interactions between apps. <br>

Shared Preferences:  <br>
Apps can use shared preferences to store and retrieve small amounts of data in key-value pairs. If two apps use the same shared preferences file, they can share data through this mechanism. <br>

File Sharing:  <br>
Apps can share files with each other using file providers or by writing to a shared directory that both apps have access to. <br>

It's important to note that some of these methods require permissions and may have security implications. Apps should use these methods responsibly and follow best practices for inter-app communication. <br>

<h3>• Is it possible to run an Android app in multiple processes? </h3>

Yes, it is possible to run an Android app in multiple processes. This can be achieved by setting the android:process attribute in the <application> tag of the app's manifest file. By doing this, you can specify a different process name for components of your app, such as activities, services, or broadcast receivers, allowing them to run in separate processes. <br>

However, it's important to note that running an app in multiple processes can have implications for performance and resource usage. Each additional process consumes memory and CPU resources, so it's generally recommended to use multiple processes only when necessary and to carefully consider the trade-offs involved. <br>

<h1>• What is AIDL? Enumerate the steps in creating a bounded service through AIDL.? </h1> <br>

AIDL (Android Interface Definition Language) is a language specifically developed by Google for defining the interface requirements between a client and server in Android's inter-process communication (IPC). It allows you to define the programming interface that both the client and server agree upon in order to communicate with each other.

To create a bounded service through AIDL, you need to follow these steps:

Define the AIDL interface: Create an .aidl file that defines the methods that the client can call on the service. This file should be placed in the src/main/aidl directory of your Android project. For example, if your service is named MyService, the AIDL file could be named IMyService.aidl.

aidl
```
package com.example;

interface IMyService {
    int add(int a, int b);
}
```

Implement the AIDL interface in the service: Create a service that implements the AIDL interface. This service should extend android.app.Service and provide an implementation for each method defined in the AIDL interface.

```
public class MyService extends Service {
    private final IMyService.Stub binder = new IMyService.Stub() {
        @Override
        public int add(int a, int b) {
            return a + b;
        }
    };

    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }
}
```

Declare the service in the manifest: Add the service to your AndroidManifest.xml file, specifying the AIDL interface that the service implements.

```
<service android:name=".MyService">
    <intent-filter>
        <action android:name="com.example.IMyService"/>
    </intent-filter>
</service>
```

Bind to the service from the client: In the client app, bind to the service using an Intent that specifies the AIDL interface.


```
public class MainActivity extends AppCompatActivity {
    private IMyService myService;

    private final ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            myService = IMyService.Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            myService = null;
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Intent intent = new Intent();
        intent.setComponent(new ComponentName("com.example", "com.example.MyService"));
        bindService(intent, connection, Context.BIND_AUTO_CREATE);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unbindService(connection);
    }
}
```
Call methods on the service: Once the client is connected to the service, you can call methods on the service using the interface defined in the AIDL file.

```
if (myService != null) {
    try {
        int result = myService.add(5, 3);
        Log.d(TAG, "Result: " + result);
    } catch (RemoteException e) {
        e.printStackTrace();
    }
}
```
These steps outline the basic process of creating a bounded service in Android using AIDL for inter-process communication.




<h3>• What is a ContentProvider and what is it typically used for? </h3>

A ContentProvider is a component in Android that manages access to a central repository of data. It provides a standard interface for interacting with the underlying data source, which can be a SQLite database, a file system, an in-memory data structure, or any other persistent storage mechanism. ContentProviders are typically used for sharing data between different applications, though they can also be used within a single application to manage access to shared data.

<b>ContentProviders offer several key features:</b>

<b>Data Abstraction:</b> <br>
ContentProviders abstract the underlying data source, allowing clients to interact with the data without needing to know its specific implementation details.

<b>Data Security:</b> <br>
ContentProviders can enforce permissions to control access to the data, ensuring that only authorized clients can read or write data.

<b>Data Sharing:</b> <br>
ContentProviders enable data sharing between different applications. By using a ContentProvider, an application can make its data accessible to other applications, subject to the permissions it defines.

<b>Data Querying:</b> <br>
ContentProviders support querying data using a structured query language (SQL)-like syntax, making it easy to retrieve specific subsets of data.

<b>Data Modification:</b> <br>
ContentProviders support data modification operations, such as inserting, updating, and deleting data, ensuring that changes to the data are managed consistently.

Overall, ContentProviders play a crucial role in the Android platform's architecture by providing a standardized way to manage and share data between different parts of an application or between different applications.

<h1> How to find duplicates in array in kotlin ?</h1>
We can use any function from the following to remove duplicates from an array in Kotlin:

```
distinct()
toSet()
toMutableSet()
toHashSet()
```

Let's start learning one by one by example.

Consider a data class Mentor like below:

data class Mentor(val id: Int, val name: String)
And, an array of Mentor:

```
val mentors = arrayOf(
    Mentor(1, "Amit Shekhar"),
    Mentor(2, "Anand Gaurav"),
    Mentor(1, "Amit Shekhar"),
    Mentor(3, "Lionel Messi"))
```

Remove duplicates using distinct()
In Kotlin, we can use distinct() function available in Collection functions to remove duplicates.

```
val distinct = mentors.distinct()
println(distinct)
```

This will print the following:

```
[Mentor(id=1, name=Amit Shekhar),
Mentor(id=2, name=Anand Gaurav),
Mentor(id=3, name=Lionel Messi)]
```

Note:

Maintain the original order of items.
Among equal elements of the given array, only the first one will be present in the output.
Returns List
Here, as we have used it to remove duplicate mentors from the array, similarly, we can use it to remove duplicate strings from an array.

Remove duplicates using toSet()
In Kotlin, we can use toSet() function available in Collection functions to remove duplicates.

```
val toSet = mentors.toSet()
println(toSet)
```

This will print the following:

```
[Mentor(id=1, name=Amit Shekhar),
Mentor(id=2, name=Anand Gaurav),
Mentor(id=3, name=Lionel Messi)]
```

Note:

Maintain the original order of items.
Returns Set which is a read-only set. It means that we can't perform operations like add on the set. Next, we will see toMutableSet() which returns read/write set.
Remove duplicates using toMutableSet()
In Kotlin, we can use toMutableSet() function available in Collection functions to remove duplicates.

```
val toMutableSet = mentors.toMutableSet()
println(toMutableSet)
```

This will print the following:
```
[Mentor(id=1, name=Amit Shekhar),
Mentor(id=2, name=Anand Gaurav),
Mentor(id=3, name=Lionel Messi)]
```

Note:

Maintain the original order of items.
Returns MutableSet which is a read/write set. It means that we can perform operations like add on the mutable set.
Remove duplicates using toHashSet()
In Kotlin, we can use toHashSet() function available in Collection functions to remove duplicates.

```
val toHashSet = mentors.toHashSet()
println(toHashSet)
```

This will print the following:

```
[Mentor(id=3, name=Lionel Messi),
Mentor(id=1, name=Amit Shekhar),
Mentor(id=2, name=Anand Gaurav)]
```

Note:

Similar to MutableSet but do NOT maintain the original order of items.
Returns HashSet
Here, as we have used it to remove duplicate mentors from the array, similarly, we can use it to remove any duplicate elements like strings, numbers, etc from an array.


<h1>what is open keyword in kotlin?</h1>

In Kotlin, we can mark a class, a function, or a variable with the open keyword like below:

Class
```
open class Mentor {

}
```

Function
```
open fun guide() {

}
```

Variable
```
open val slotsAvailable = 5
```

Let's understand why we use the open keyword in Kotlin.

open keyword with class
The open keyword with the class means the class is open for the extension meaning that we can create a subclass of that open class.

In Kotlin, all the classes are final by default meaning that they can not be inherited.

Suppose we have a class Mentor:

```
class Mentor {

}
```

We will not be able to create a subclass of the class.
```
class ExperiencedMentor: Mentor() {

}
```

The compiler will show an error.

This type is final, so it cannot be inherited from
If we want to allow inheritance, we need to mark the class with the open keyword. Basically, opening it for the extension.

Our updated Mentor class:

```
open class Mentor {

}
```

Now, we will be able to create a subclass of this class.

```
class ExperiencedMentor: Mentor() {

}
```

It will work perfectly.

You must have noticed that it is completely opposite to what we do in Java.

Let's see how it is different in Kotlin and Java.

```
In Java		In Kotlin
final class Mentor { }	->	class Mentor { }
class Mentor { }	->	open class Mentor { }
```

Basically, in Kotlin, all the classes are final by default but in Java, this is exactly the opposite. In Java, to make the class final, we need to use the final keyword.

That is why we need the open keyword in Kotlin with the class to allow inheritance.

open keyword with function
Similar to the classes, all the functions in Kotlin are by default final meaning that you can't override a function.

Consider a function guide() inside the Mentor class:

```
open class Mentor {

    fun guide() {

    }

}
```

We will not be able to override the function.

```
class ExperiencedMentor : Mentor() {

    override fun guide() {

    }

}
```

The compiler will show an error.

guide' in 'Mentor' is final and cannot be overridden
In order to allow the overriding of the function, we need to add the open keyword.

Our updated Mentor class:

```
open class Mentor {

    open fun guide() {

    }
}
```

We will be able to override the function now like below:

```
class ExperiencedMentor : Mentor() {

    override fun guide() {

    }
}
```

It will work perfectly.

Again, in Kotlin, all the functions are final by default but in Java, this is exactly the opposite. In Java, to make the function final, we need to use the final keyword.

That is why we need the open keyword in Kotlin with the function to allow it to be overridden.

open keyword with variable
Similarly, the variables in Kotlin are final by default. So, to override it in the child class, we need to set the variables as open in our base class.

Here is the base class Mentor:
```
open class Mentor {

    val slotsAvailable = 5

}
```

Here is the child class.
```
class ExperiencedMentor : Mentor() {

    override val slotsAvailable = 10

}
```

We will not be able to override the variable.

The compiler will show an error.

'slotsAvailable' in 'Mentor' is final and cannot be overridden
In order to allow the overriding of the variable, we need to add the open keyword.

Our updated Mentor class:
```
open class Mentor {

    open val slotsAvailable = 5

}
```

We will be able to override the variable now like below:

```
class ExperiencedMentor : Mentor() {

    override val slotsAvailable = 10

}
```

It will work perfectly.

Again, in Kotlin, all the variables are final by default but in Java, this is exactly the opposite. In Java, to make the variable final, we need to use the final keyword.

That is why we need the open keyword in Kotlin with the variable to allow it to be overridden.


<h1>What is internal modifier in kotlin ?</h1>
The declarations marked with the internal modifier are visible everywhere in the same module.

A module is a set of Kotlin files compiled together:

an IntelliJ IDEA module;
a Maven project;
a Gradle source set (with the exception that the test source set can access the internal declarations of main);
a set of files compiled with one invocation of the <kotlinc> Ant task.


// Internal.kt file

// Visible to everyone in the same module
```
internal const val numberThree = 3

// Visible to everyone in the same module
internal open class User() {
    // Visible to everyone in the same module that has visibility on User
    internal val numberEight = numberThree.plus(5)
}
```

// SameModule.kt file
```
// numberThree is visible because this file is in the same module than Internal.kt
// numberEight is visible because this file is in the same module than User
private const val numberEleven = numberThree.plus(User().numberEight)
```
// DifferentModule.kt file
```
// ERROR: numberThree is not visible because this file is in a different module than Internal.kt
// ERROR: numberEight is not visible because User() is in a different module than Internal.kt
private const val numberEleven = numberThree.plus(User().numberEight)
```

<h1>JvmStatic, JvmOverloads, JvmField Annotations in Kotlin</h1>
https://medium.com/@anandgaur22/jvmstatic-jvmoverloads-jvmfield-annotations-in-kotlin-d856fe38fd92


<h1> what is difference between letinit and lazy?</h1>
<b>lateinit</b>
<b>lazy</b>
Let's begin with the lateinit property.

<b>lateinit</b>
lateinit in Kotlin is useful in a scenario when we do not want to initialize a variable at the time of the declaration and want to initialize it at some later point in time, but we make sure that we initialize it before use.

One way(that is not a good way) to achieve this is by creating a nullable variable as below:

```
private var mentor: Mentor? = null
```

And as we all know that there is always a better way in Kotlin to achieve what we need.

What if we do not want to make the variable nullable?

The answer is lateinit.

Let's update our code:

```
private lateinit var mentor: Mentor
```

Here, we can notice that the lateinit keyword and also the variable is non-nullable.

That is what we wanted.

And later we can initialize the variable when we need it as below:

```
fun bookASlot() {
    mentor = Mentor()
}
```

But we must take care that we initialize the variable before accessing it, or else it will throw the following exception.

kotlin.UninitializedPropertyAccessException: lateinit property mentor has not been initialized
In Kotlin, we also have a way to check if the lateinit variable is initialized or not.

By using isInitialized:

```
if(this::mentor.isInitialized) {
    // access mentor
} else {
    // do something else
}
```

Things to consider when we use the lateinit property:

Can be only used with the var keyword.
Can be only used with a non-nullable variable.
Should be used if the variable is mutable and can be initialized later.
Should be used if you are sure about the initialization before use.
This was about the lateinit property in Kotlin.

Now, it's time to learn about the lazy property in Kotlin.

<b>lazy</b>
lazy in Kotlin is useful in a scenario when we want to create an object inside a class, but that object creation is expensive and that might lead to a delay in the creation of the object that is dependent on that expensive object.

```
class Session {
    private val mentor: Mentor = Mentor()
}
```

Suppose Mentor is an expensive object. And Session is the object that is dependent on the Mentor object. <br>
<br>
If Mentor object creation takes time, it will delay the creation of Session object. <br>
<br>
So, this is where the lazy keyword in Kotlin will help us. <br>
<br>
Let's update our code: <br>

```
class Session {
    private val mentor: Mentor by lazy { Mentor() }
}
```

Here, we have used the lazy keyword. <br>

So, we need to understand that the object mentor will get initialized only when it is accessed for the first time, else it will not get initialized. <br>


It will lead to the fast creation of the Session object in the above example because the Mentor object will not get initialized unnecessarily during the object creation of Session. It will get initialized when it is accessed for the first time.

Things to consider when we use the lazy property:

Can be only used with the val keyword, hence read-only property.
We want the variable to be initialized only if we need it for the first time.
Must understand that it only creates the object when we access it for the very first time and then in the subsequent access, it returns the same object.
This was about the lazy property in Kotlin.

Now we must have understood the lateinit vs lazy properties in Kotlin

<h1>coroutineScope vs supervisorScope</h1>

• A coroutineScope will cancel whenever any of its children fail. <br>
a coroutineScope is a coroutine builder that creates a new coroutine scope and ensures that all its child coroutines complete before returning. <br> If any child coroutine fails (throws an unhandled exception), the coroutineScope itself will cancel, which in turn cancels all its remaining child coroutines. Here's an example to illustrate this behavior: <br>

```
import kotlinx.coroutines.*

suspend fun main() {
    try {
        myCoroutineFunction()
    } catch (e: Exception) {
        println("Coroutine function failed with exception: $e")
    }
}

suspend fun myCoroutineFunction() {
    coroutineScope {
        val job1 = launch {
            delay(1000) // Simulate some work
            println("Job 1 completed")
        }

        val job2 = launch {
            delay(2000) // Simulate some work
            println("Job 2 completed")
            throw RuntimeException("Something went wrong in job 2")
        }

        // Wait for both jobs to complete
        job1.join()
        job2.join()

        // This line will not be reached if any child coroutine fails
        println("All jobs completed successfully")
    }
}
```
In this example: <br>
 <br>
- We define a myCoroutineFunction() which creates a coroutineScope.  <br>
- Inside the coroutineScope, we launch two child coroutines (job1 and job2).  <br>
- job1 completes successfully after a delay of 1 second, printing "Job 1 completed".  <br>
- job2 completes after a delay of 2 seconds, but it throws an exception (RuntimeException) before completing.  <br>
- Since job2 fails, the coroutineScope detects the failure and cancels all its children. <br>
- Consequently, the completion message "All jobs completed successfully" will not be printed, and the control will flow to the catch block in the main() function, where we catch the exception thrown by the failed coroutine.  <br>
- This demonstrates how a coroutineScope will cancel whenever any of its children fail. <br>

• A supervisorScope won't cancel other children when one of them fails. <br>
supervisorScope does not cancel the other children when one of them fails. Instead, it allows the other children to continue their execution independently. Let me provide an example to illustrate this behavior:

```
import kotlinx.coroutines.*

suspend fun main() {
    try {
        myCoroutineFunction()
    } catch (e: Exception) {
        println("Coroutine function failed with exception: $e")
    }
}

suspend fun myCoroutineFunction() {
    supervisorScope {
        val job1 = launch {
            delay(1000) // Simulate some work
            println("Job 1 completed")
        }

        val job2 = launch {
            delay(2000) // Simulate some work
            println("Job 2 completed")
            throw RuntimeException("Something went wrong in job 2")
        }

        // Wait for both jobs to complete
        job1.join()
        job2.join()

        // This line will be reached even if one child coroutine fails
        println("All jobs completed")
    }
}
```
In this example: <br>
 <br>
- We define a myCoroutineFunction() which creates a supervisorScope. <br>
- Inside the supervisorScope, we launch two child coroutines (job1 and job2). <br>
- job1 completes successfully after a delay of 1 second, printing "Job 1 completed". <br>
- job2 completes after a delay of 2 seconds, but it throws an exception (RuntimeException) before completing. <br>
- Unlike coroutineScope, the supervisorScope does not cancel the other child (job1). <br>
- Consequently, the completion message "All jobs completed" will be printed, indicating that even though one child coroutine failed, the other one was allowed to complete normally. <br>

If we want to continue with the other tasks even when one fails, we go with the supervisorScope. <br> <br>


<h1> Things to know while using the init block in Kotlin: </h1>

• The init block gets executed immediately after the primary constructor. <br>
• The init block gets executed before the secondary constructor. <br>
• Primary constructor parameters can be used in the initializer blocks. <br>
• A class can have more than one init block, in this case, the initializer blocks are executed in the same order as they appear in the class body considering the properties if there are any in between. <br>
• It does not take any parameters. <br>


<h1>withContext in Kotlin Coroutines: </h1>
It does not launch a coroutine and it is just a "suspend function" used for shifting the context of the existing coroutine.<br>
It is typically used to switch to a different coroutine context, such as switching from a background thread to the main UI thread or vice versa. withContext is used to perform a block of code in a different context and then return to the original context. Here's a basic example:

```
suspend fun fetchData(): String {
    return withContext(Dispatchers.IO) {
        // Perform some network operation or heavy computation
        "Data fetched from network"
    }
}

// In a coroutine scope
launch {
    val data = fetchData() // This will switch to the IO dispatcher, fetch data, and then switch back
    // Use the fetched data
}
```
<h1> What is flow explain? </h1>
Flow is an asynchronous data stream(which generally comes from a task) that emits values to the collector and gets completed with or without an exception.

This will make more sense when we go through the example. Let's take a standard example of image downloading.

Assume that we have a task: To download an image, emit the items(values) which are the percentage of the image downloading like 1%, 2%, 3%, and so on. It can get completed with or without an exception. If everything goes well, the task will be completed without an exception. But, in case of network failure, the task will be completed with an exception.

So, there will be a task that will be done and will emit some values which will be collected by the collector.

Now, let's discuss the major components of Flow.

The major components of Flow are as below:

<b>Flow Builder</b>
• Operator
• Collector

Let's understand this with the following analogy. <br>

Flow Builder	->	Speaker <br>
Operator	->	Translator <br>
Collector	->	Listener <br>

<b>Flow Builder</b> <br>
In simple words, we can say that it helps in doing a task and emitting items. Sometimes it is just required to emit the items without doing any task, for example, just emit a few numbers (1, 2, 3). Here, the flow builder helps us in doing so. We can think of this as a Speaker. The Speaker will think(do a task) and speak(emit items).

<b>Operator</b> <br>
The operator helps in transforming the data from one format to another.

We can think of the operator as a Translator. Assume that the Speaker is speaking in French and the Collector(Listener) understands English only. So, there has to be a translator to translate French into English. That translator is an Operator. <br>

Operators are more than this actually, using the operator, we can also provide the thread on which the task will be done. We will see this later. <br>

<b>Collector</b> <br>
The collector collects the items emitted using the Flow Builder which are transformed by the operators. <br>

We can think of a collector as a Listener. Actually, Collector also comes under the operator which is known as Terminal Operator. The collector is a Terminal Operator. For now, we will skip the Terminal Operator as that is not needed for this blog on Flow API. <br>

<b>Flow API Source Code</b> <br>
 <br>
The Flow interfaces look like the below in the source code of Coroutines: <br>

```
public fun interface FlowCollector<in T> {

    public suspend fun emit(value: T)

}
public interface Flow<out T> {

    public suspend fun collect(collector: FlowCollector<T>)

}
```

Hello World of Flow

```
flow {
    (0..10).forEach {
        emit(it)
    }
}.map {
    it * it
}.collect {
    Log.d(TAG, it.toString())
}
```


flow { }	->	Flow Builder <br>
map { }	    ->	Operator <br>
collect {}	->	Collector <br>

Let's go through the code. <br>

First, we have a flow builder which is emitting 0 to 10. <br>
Then, we have a map operator which will take each and every value and square(it * it). The map is Intermediate Operator. <br>
Then, we have a collector in which we get the emitted values and print them as 0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100. <br>
Note: When we actually connect both the Flow Builder and the Collector using the collect method, then only, it will start executing. <br>

Now it's time to learn more about the Flow Builder. <br>

Types of flow builders <br>
There are 4 types of flow builders: <br>

<b>flowOf(): </b> <br>
It is used to create flow from a given set of items. <br>

<b>asFlow(): </b> <br>
It is an extension function that helps to convert type into flows. <br>

<b>flow{}: </b>  <br>
This is what we have used in the Hello World example of Flow. <br>

<b>channelFlow{}: </b> <br>
This builder creates flow with the elements using send provided by the builder itself. <br>

Examples:

flowOf()

```
flowOf(4, 2, 5, 1, 7)
.collect {
    Log.d(TAG, it.toString())
}
```

asFlow()

```
(1..5).asFlow()
.collect {
    Log.d(TAG, it.toString())
}
```

flow{}

```
flow {
    (0..10).forEach {
        emit(it)
    }
}
.collect {
    Log.d(TAG, it.toString())
}
```

channelFlow{}

```
channelFlow {
    (0..10).forEach {
        send(it)
    }
}
.collect {
    Log.d(TAG, it.toString())
}
```

At the end of this article, we will also learn to create Flow using Flow Builder. Now we need to learn about the flowOn operator.

<b>flowOn Operator </b>

flowOn Operator is very handy when it comes to controlling the thread on which the task will be done.

Usually, in Android, we do a task on a background thread and show the result on the UI thread.

Let's see this with an example: We have added a delay of 500 milliseconds inside the flow builder to simulate delay.

```
val flow = flow {
    // Run on Background Thread (Dispatchers.Default)
    (0..10).forEach {
        // emit items with 500 milliseconds delay
        delay(500)
        emit(it)
    }
}
.flowOn(Dispatchers.Default)
```

```
CoroutineScope(Dispatchers.Main).launch {
    flow.collect {
        // Run on Main Thread (Dispatchers.Main)
        Log.d(TAG, it.toString())
    }
}
```

Here the task inside the flow builder will be done on the background thread which is Dispatchers.Default.

Now, we need to switch it to the UI thread. To achieve that, we need to wrap our collect API inside the launch with Dispatchers.Main.

This is how the flowOn operator can be used to control the thread.

flowOn() is like subscribeOn() in RxJava

<b>Dispatchers: </b> <br>
They help in deciding the thread on which the work has to be done. There are majorly three types of Dispatchers which are IO, Default, and Main. IO dispatcher is used for network and disk-related tasks. Default is used for CPU-intensive work. The Main is the UI thread of Android.

Now, we will learn how to create our Flow using the Flow builder. We can create our Flow for any task using the Flow Builder.

Creating Flow Using Flow Builder
Let's learn it through examples.

1. Move File from one location to another location

Here, we will create our Flow using the Flow Builder for moving the file from one location to another in the background thread and send the completion status on Main Thread.

```
val moveFileflow = flow {
        // move file on background thread
        FileUtils.move(source, destination)
        emit("Done")
}
.flowOn(Dispatchers.IO)
```

```
CoroutineScope(Dispatchers.Main).launch {
    moveFileflow.collect {
        // when it is done
    }
}
```

2. Downloading an Image

Here, we will create our Flow using the Flow Builder for downloading the Image which will download the Image in the background thread and keep sending the progress to the collector on the Main thread.

```
val downloadImageflow = flow {
        // start downloading
        // send progress
        emit(10)
        // downloading...
        // ......
        // send progress
        emit(75)
        // downloading...
        // ......
        // send progress
        emit(100)
}
.flowOn(Dispatchers.IO)
```

```
CoroutineScope(Dispatchers.Main).launch {
    downloadImageflow.collect {
        // we will get the progress here
    }
}
```

This is how we can create our Flow.

In Kotlin, Coroutine is just the scheduler part of RxJava but now with Flow API coming alongside it, it can be an alternative to RxJava in Android


<h1>What is companion or object?</h1>

Before jumping into the companion object in Kotlin, we need to see how we use the static keyword in Java. <br>

First, let's consider an example without the static keyword in Java. <br>

```
public class Mentor {

    public void guide() {

    }

}
```

Here, if we have to call the method guide() , we will have to create an object of the Mentor class and then call.

Mentor mentor = new Mentor();
mentor.guide();
So, how can we call the method without creating the object of the class?

The answer is the static keyword in Java.

Our updated Mentor class:

```
public class Mentor {

    public static void guide() {

    }

}
```

Now, we can call directly without creating the object of the Mentor class.

```
Mentor.guide();
```

So, we are able to call the method without creating the object of the class in Java with the use of the static keyword.

Now, the next big question is: Can we call a method in Kotlin without creating the object of a class like we just did for Java with the use of a static keyword?

Answer: companion object.

Note: We do not have a static keyword in Kotlin.

In Kotlin, we can call a method of a class without creating the object of that class with the use of a companion object.

Let's create a class Mentor in Kotlin:

```
class Mentor {

    companion object {

        fun guide() {

        }

    }
}
```

Now, we can call the method guide() directly without creating the object of the class Mentor in Kotlin.

Mentor.guide()
Similarly, we can also access any variable. Let's add a variable inside the companion object.

```
class Mentor {

    companion object {

        const val MAX_SLOTS = 10

        fun guide() {

        }

    }

}
```

Now, we can access the variable:

```
val maxSlots = Mentor.MAX_SLOTS
```
We can also access as below:

val maxSlots = Mentor.Companion.MAX_SLOTS
Using the Companion object reference, but that is redundant.

Note: The default name of a companion object is Companion.

We can also name our companion object as we have done below:

```
class Mentor {

    companion object Config {

        const val MAX_SLOTS = 10

    }

}
```

We can access the variable as below without the companion reference name:

val maxSlots = Mentor.MAX_SLOTS
and also as below with the companion reference name:

val maxSlots = Mentor.Config.MAX_SLOTS
But the companion reference name is redundant.

Important points to keep in mind while defining a companion object or using it:

It cannot be defined outside a class.
The object is common in all instances of the class.
It is instantiated for the first time as soon as the containing class is loaded. It means that it is instantiated even if we have not used the companion object.
Now, we might have started thinking about why there is a companion object when we already have a regular object in Kotlin.

Example of the regular object in Kotlin.

Defining outside a class:
```
object Config {

    const val MAX_SLOTS = 10

}
```
It can be accessed as below:

```
val maxSlots = Config.MAX_SLOTS
```

Defining inside a class:
```
class Mentor {

    object Config {

        const val MAX_SLOTS = 10

    }

}
```

It can be accessed as below:

```
val maxSlots = Mentor.Config.MAX_SLOTS
```

When defined inside a class, we can't skip the name while accessing it.

However, this is not the case with the companion object. Companion object allows us to skip the name and access without using the name.

Let me tabulate the differences between both of them for your better understanding so that you can decide which one to use based on your use case.

<b>Companion object<b> <br>
• It needs to be defined inside a class.	
• The companion object is instantiated for the first time as soon as the containing class is loaded. 
• It is equivalent to a static keyword in Java.	
• Gets a default name as a Companion when we do not provide a name.
• We can skip the name while calling a method or accessing a variable.

<b>Regular object -<b> <br>
• It can be defined anywhere.
• It means that it is instantiated even if we have not used the companion object.	The object is instantiated lazily when we access it for the first time.
• Must be named by us.
• If defined inside a class, we can't skip the name while calling a method or accessing a variable.
• Mainly used for providing Singleton behavior.

<h1> why coroutine is more preferable than thread ? </h1>
<br>
for example when we try to execute 100 thousands of cororuinte it will execute without any exception if we same we trys to execute 100 thousands of threads there is chances of outofmemory exception. <br>
<br>
OutOfMemoryError occurs in threads but is less common in coroutines because threads have higher memory overhead due to their own stack space, while coroutines are lightweight and share the same stack, allowing more efficient memory usage. <br>


<h1>Scope Functions - </h1>
scope functions are functions that allow you to execute a block of code within the context of an object. They provide a concise way to perform operations on an object and can help improve the readability of your code. Kotlin provides five scope functions: let, run, with, apply, and also. Here's a brief overview of each: <br>

<b>let: </b> <br>
Executes the given block of code on a non-null object and returns the result of the block.

```
val result = "Hello".let { it.length }
println(result) // Output: 5
```

<b>run:</b> <br>
Similar to let, but the object is accessed using this inside the block. It returns the result of the block.
```
val result = "Hello".run { length }
println(result) // Output: 5
```

<b>with: </b> <br>
Similar to run, but it's not an extension function. It takes the object as an argument and returns the result of the block.
```
val result = with("Hello") { length }
println(result) // Output: 5
```

<b>apply:</b> <br>
Used to configure properties of an object. It returns the object itself after applying the block.

```
val person = Person().apply {
    name = "John"
    age = 30
}
```

<b>also:</b> <br>
Similar to apply, but it returns the original object instead of the result of the block.

```
val length = "Hello".also { println("Length: ${it.length}") }.length
println(length) // Output: Length: 5 \n 5
```

Each scope function has its use cases, and choosing the right one depends on the context and the desired behavior. They can help make your code more concise and expressive by reducing the need for temporary variables and improving the readability of operations on objects.

<h1> why coroutine is more powerful than thread ?</h1>
The main differences between coroutines and traditional concurrency mechanisms like threads, handlers, and AsyncTask are in their design, usage, and performance characteristics:

<h4>Concurrency Model:</h4>
<b>Threads:</b> <br> Traditional threads are managed by the operating system, and each thread corresponds to a separate execution context.
<b>Coroutines:</b> <br> Coroutines are light-weight, user-space threads. They are managed by a coroutine dispatcher, which can be configured to use a small number of threads or even a single thread.
<h4>Concurrency Control:</h4>
<b>Threads:</b> <br> Managing threads involves dealing with low-level synchronization primitives like locks, mutexes, and semaphores to avoid race conditions.
<b>Coroutines:</b> <br> Coroutines use structured concurrency, where you can define scopes for coroutines and easily cancel or suspend them. This makes it easier to write and reason about concurrent code.
<h4>Performance:</h4>
<b>Threads:</b> <br> Creating and managing threads can be expensive in terms of memory and CPU usage, especially for a large number of threads.
<b>Coroutines:</b> <br> Coroutines are more lightweight compared to threads, as they can be multiplexed onto a smaller number of threads. This makes coroutines more efficient in terms of resource usage.
<h4>Error Handling:</h4>
<b>Threads:</b> <br> Error handling in multithreaded programs can be complex, as errors in one thread can affect other threads.
<b>Coroutines:</b> <br> Coroutines have built-in support for structured error handling, making it easier to handle errors locally within a coroutine.
<h4>Programming Model:</h4>
<b>Threads:</b> <br> Programming with threads often involves callbacks, listeners, or synchronization constructs, which can lead to callback hell or complex synchronization logic.
<b>Coroutines:</b> <br> Coroutines use suspend functions, which allow you to write asynchronous code in a more sequential and readable manner, similar to synchronous code.
In summary, coroutines provide a more efficient, scalable, and easier-to-use concurrency model compared to traditional threading mechanisms. They are particularly well-suited for asynchronous programming in modern applications.


<h1> Write higher order function </h1>

```
fun addOperation(a:Int,b:Int,func:(Int,Int)->Int):Int{
    return func(a,b)
}


fun main() {
    var result = addOperation(10,5) {a,b -> a+b}
    println(result)
}
```


<h1>if class extending interface having same method name and parameter which interface method will get called ? and if we want to call specific interface than how to call it ?</h1>

```
interface Car {
    fun drive()
}

interface Truck {
    fun drive()
}

class Vehicle : Car,Truck {
    
     override fun drive() { // method of first interface will get called. i.e. - Car
        println("Vehicle is driving")
    }
}
```

to call specific interface method -

```
(vehicle as Truck).drive() // Output: "Vehicle is driving"
```


<h1>how to write custom extensions ?</h1>
Each function has a different use case and scoping behavior. Here's an example of how you might implement a custom scope function called customScope: <br>

```
fun <T> T.customScope(block: T.() -> Unit): T {
    block()
    return this
}
```
In this example, customScope is an extension function on any type T. It takes a lambda block as an argument, which is an extension function on T and has a receiver of T. Inside customScope, it calls block() on the instance and then returns the instance itself.<br>

You can use customScope like this:<br>

```
val result = "Hello".customScope {
    println("Value is $this")
}
```
This will print "Value is Hello" and assign "Hello" to result.<br>

Keep in mind that while writing custom scope functions can be useful in specific situations, it's generally recommended to use the standard scope functions provided by Kotlin (let, run, with, apply, and also) as they are well-known and understood by other Kotlin developers.<br>


<h1>calling method in cost is it allowed? </h1>

```
const val name = getName()

fun main() {

}

fun getName() :String{
    return "Siddhant"
}
```

This code defines a constant name with the value returned by the getName() function, which is "Siddhant". However, this code won't compile because the getName() function is called during the initialization of name, but functions cannot be called at the top level of a Kotlin file. Functions need to be called from within another function or a code block. Here's a modified version of the code that will compile:

```
fun main() {
    val name = getName()
    println(name)
}

fun getName(): String {
    return "Siddhant"
}
```

<h1> Enum Class Vs Sealed Class ? </h1>
enum class and a sealed class are both used to represent restricted hierarchies. However, they have different use cases and behaviors.<br>
<br>
Enum Class :  <br>
An enum class in Kotlin is used to define a type that represents a finite set of distinct values, such as days of the week, directions, etc. <br>
Each enum constant is an object and can have properties, methods, and implement interfaces.<br>
Enum classes can be used in when expressions for exhaustive checking.<br>

Example:<br>
```
enum class Direction {
    NORTH, SOUTH, EAST, WEST
}
```

Sealed Class : <br>
Sealed classes offer a compelling way to represent restricted class hierarchies, allowing you to define a fixed set of subclasses within a sealed class declaration.  <br>
On the other hand, enums provide a concise method for defining a finite set of constants.


A sealed class is used to represent a restricted hierarchy where all subclasses must be nested inside the sealed class declaration. <br>
Sealed classes are often used in combination with when expressions to achieve exhaustive checking. <br>
Sealed classes can have subclasses with additional properties, and each subclass can have its own implementation of methods. <br>
Example:<br>
<br>
```
sealed class Result {
    data class Success(val data: String) : Result()
    data class Error(val message: String) : Result()
}
```

In summary, enums are used for defining a fixed set of values, while sealed classes are used for defining restricted class hierarchies.

<h1>Object vs Companion Object </h1>

Object <br>
Object in kotlin is a way of implementing Singletons. We all have come across the need of Singleton pattern in our career for various use-cases. Well, in kotlin this has been made very straight forward.
e.g.

```
object MyObject {
  
  // further implementation
  
  fun printHello() {
    println("Hello World!")
  }
  
}
```

This implementation is also called object declaration. Object declarations are thread-safe and are lazy initialized, i.e. objects are initialized when they are accessed for the first time.

Companion Object
If we want some implementation to be a class but still want to expose some behavior as static behavior, companion object come to the play. These are object declarations inside a class. These companion objects are initialized when the containing class is resolved, similar to static methods and variables in java world.
e.g.

```
class MyClass {

  // some implementations
  
  companion object {
    val SOME_STATIC_VARIABLE = "some_static_variable"
    fun someStaticFunction() {
      // static function implementation
    }
  }
}
```

Summary <br>
Given the above explanation, the use-case completely depends on the problem we are trying to solve. If we need to provide the Singleton behavior, then we are better off with Objects, else if we just want to add some static essence to our classes, we can use Companion objects.

<h1> What is lateinit vs lazy ? </h1>

<h3>What is the Lateinit? </h3>
We may not want to initialize our values at declaration time, but instead want to initialize and use them at any later time in our application. However, before using our value, we must not forget that our value must be initialized before it can be used. Let’s make an example for better understanding! <br>
<br>

```
class MainActivity : AppCompatActivity() {

    //Here is the value we don't want to initialize at declaration time,
    // so we used the lateinit keyword.
    private lateinit var myUser:User

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        //We can initialize our value at any time in our application.
        myUser = User("Hüseyin","Özkoç")
        println("MY NAME IS : " + myUser.name)
    }
}

data class User(var name: String, var surname: String)
```

A basic lateinit usage example.<br>
<br>
As we saw in the example above, we did not initialize our value at declaration time. Instead, we initialize our application’s OnCreate() method and print our value to LogCat with the print() method. But what if we tried to print our value to LogCat without initializing it? Let’s try and see!<br>
<br>

if we try to access our value without initializing it, we will encounter a “UninitializedPropertyAccessException” exception. To avoid this error, we can also use the isInitialized() method that Kotlin provides for us.<br>

Basic use case of isInitialized.<br>
As you can see, you can use isInitialized to check if my value is initialized if you need it.<br>

```
class MainActivity : AppCompatActivity() {
    //Here is the value we don't want to initialize at declaration time,
    //So we used the lateinit keyword.
    private lateinit var myUser: User

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        /**
        We can check if our value is initialize using isInitialized.
         */
        if (this::myUser.isInitialized) {
            println("MY NAME IS : " + myUser.name)
        } else {
            myUser = User("Hüseyin", "Özkoç")
            println("MY NAME IS : " + myUser.name)
        }
    }
}

data class User(var name: String, var surname: String)
```

<h3>What is the by Lazy? </h3> <br>
Lazy initialization is a design pattern that we often come across in the software world. With lazy initialization, we can create objects only the first time that we access them, otherwise we don’t have to initialize them. It ensures that objects that are expensive to create are initialized only where they are to be used, not on the app startup.<br>

lazy in Kotlin is useful in a scenario when we want to create an object inside a class, but that object creation is expensive and that might lead to a delay in the creation of the object that is dependent on that expensive object. So, we need to understand that the object will get initialized only when it is accessed for the first time, else it will not get initialized.<br>

Let’s make an example for better understanding!<br>

```
class MainActivity : AppCompatActivity() {
    //Here is the value we don't want to initialize at declaration time,
    //So we used the lateinit keyword.
    private lateinit var myButton: Button

    /**
     * Suppose we have this expensive object
     * that will initialize only when we need it.
     */
    private val myUser: User by lazy {
        print("Lazy initialization")
        User("Hüseyin", "Özkoç")
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        myButton = findViewById(R.id.myButton)

        myButton.setOnClickListener {

            /**
             * We want this object to be created only once
             * when the button is clicked, not inside the onCreate() method.
             * 
             *  Therefore, the moment we use our myUser value,
             *  our value will be initialized once and
             *  we will use that initialized value for all other clicks.
             */
            println(myUser.name)

        }

    }
}

data class User(var name: String, var surname: String)
```

As we saw in the example above, our value was only initialized once when we tried to access our object, and then we used this initialized object on each click. <br>

<h3>What are the differences between Lateinit and Lazy?</h3> <br>
1- The modifier “lateinit” is restricted to mutable(var) variable properties, whereas the modifier “lazy” is exclusively used with read-only(val) properties. <br>
<br>
2- A property marked with “lateinit” can be assigned a value multiple times as needed during runtime, whereas a property initialized with “lazy” can only be assigned a value once upon its first use. <br>

3- It is not possible to declare a primitive data type as a “lateinit” property, whereas a “lazy” property can be of either primitive or non-primitive data types. (Such as Int) <br>

4- While it is not possible to ensure thread safety for a “lateinit” property, for a “lazy” property we have the option to choose from synchronization options such as SYNCHRONIZED, PUBLICATION, or NONE. (That’s why we use lazy when using the Singleton design pattern.) <br>

5- Unlike a “lateinit” property, which cannot be declared as nullable, a “lazy” property can be defined as either nullable or non-nullable. <br>

6- While a “lateinit” property cannot have a customized getter, a “lazy” property contains a block of code that runs the first time the property is called. <br>

7- Attempting to access a “lateinit” property before it has been initialized results in a distinct exception that specifies the uninitialized property. On the other hand, a “lazy” property cannot be accessed before its initialization. It is important to note that a “lazy” property can be null, yet it will still be initialized the first time the property is accesse <br>

<h1> ViewBinding vs DataBinding - </h1>

ViewBinding - <br>
Only binding views to code.  <br>

DataBinding - <br>
Binding data (from code) to views + ViewBinding (Binding views to code) <br>
 <br>

There are three important differences <br>

With view binding, the layouts do not need a layout tag <br>

You can't use viewbinding to bind layouts with data in xml (No binding expressions, no BindingAdapters nor two-way binding with viewbinding) <br>

The main advantages of viewbinding are speed and efficiency. It has a shorter build time because it avoids the overhead and performance issues associated with databinding due to annotation processors affecting databinding's build time. <br>

In short, there is nothing viewbinding can do that databinding cannot do (though at cost of longer build times) and there are a lot databinding can do that viewbinding can"t  <br>

<h1>is there any other way than recreating activity than rotation of app? </h1>
is there any other way to recreate activity other than roatatiin <br>

Yes, there are several ways an activity can be recreated other than rotation. Here are a few examples:<br>

Configuration Changes: <br>
Apart from rotation, changes like language, font size, or night mode can trigger a configuration change, leading to the recreation of the activity. <br>

Multi-Window Mode: <br>
If your app supports multi-window mode and the user changes the app's size or enters/exits multi-window mode, the activity may be recreated to adapt to the new configuration. <br>

Theme Changes: <br>
If your app allows users to change themes dynamically, switching between themes can cause the activity to be recreated to apply the new theme. <br>

Locale Changes: <br>
Changing the device's locale can also recreate the activity if your app supports multiple languages and you haven't handled locale changes properly. <br>

Low Memory: <br> 
As mentioned earlier, if the system is low on memory, it may destroy and recreate activities to free up resources. <br>

<h1> What is difference between Plugin and dependancy ? </h1>
Key Differences: <br>
<h3> Purpose: </h3> 
Dependencies are essential external components that your project requires to function, whereas plugins are optional components that add specific features or functionalities to a software application.

<h3> Integration: </h3> 
Dependencies are usually integrated into your project during the build process based on configuration files, while plugins are integrated into an application at runtime or during its setup phase.

<h3> Usage: </h3>
Dependencies are typically essential for the basic operation of the software, while plugins provide additional or advanced features that may not be needed by all users or in all scenarios.
