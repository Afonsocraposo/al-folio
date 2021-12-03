---
layout: post
title: "Flutter: How to Save Objects in SharedPreferences"
date: 2019-09-07
description: An easy solution for small amounts of data
comments: true
---

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/posts/2019-09-07-sharedPref/header.jpeg">
    </div>
</div>
<br>

While developing a mobile application, you may want to save a custom object for later use, for example, saving a user's info when the application is closed and using it when the app is opened later.

To do so, we'll encode our object into a JSON string and save it in the `SharedPreferences` space. When we want to read the saved data, we only have to decode from a JSON string back to our object. Saving data in `SharedPreferences` is an easy solution when you want to save small amounts of data.

<center>
<h1>
. . .
</h1>
</center>

# **Create a Custom Object**

For our example, we'll use a very simple object to store some user information.

Our object will have three attributes: name, age, and location; each one represented by a string.

Our class is really simple. In our example, it's initiated as an object with null attributes and each one is assigned later.

{% highlight dart %}
class User {
  String name;
  String age;
  String location;

  User();

  ...
  
};
{% endhighlight %}

<center>
<h1>
. . .
</h1>
</center>

# **JSON: Encode and Decode**

To encode our user object into JSON, we must create a function inside the class that will do this conversion. It will create a map where each attribute (value) corresponds to a key:

{% highlight dart %}
Map<String, dynamic> toJson() => {
  'name': name,
  'age': age,
  'location': location,
};
{% endhighlight %}

This will convert our user object into a `Map<String, dynamic>`. To convert it to a JSON string, late we'll later have to use the function `json.encode(Map<String, dynamic>)`. This will convert our user object to a JSON string like:

```
{"name":"Alfonso","age":"21","location":"Portugal"}
```

This string will be saved in the `SharedPreferences`.
<br>

To decode from a JSON string to a user object, we must first use the function `json.decode(JSON-String)`. This will return a `Map<String, dynamic>` which we can then convert into our user object:

{% highlight dart %}
User.fromJson(Map<String, dynamic> json)
    : name = json['name'],
      age = json['age'],
      location = json['location'];
{% endhighlight %}

Therefore, the final code for our user class will be:

{% highlight dart linenos %}
class User {
  String name;
  String age;
  String location;

  User();

  User.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        age = json['age'],
        location = json['location'];

  Map<String, dynamic> toJson() => {
        'name': name,
        'age': age,
        'location': location,
      };
}
{% endhighlight %}

<center>
<h1>
. . .
</h1>
</center>

# **SharedPreferences: Save and Read**

First, in order to use the `SharedPreferences`, we have to use Flutter's plugin for it. To do so, make sure `dependencies` in your `pubspec.yaml` file looks similar to this:


{% highlight yaml %}
dependencies:
  ...
  shared_preferences: any
{% endhighlight %}

To save (write) something in `SharedPreferences`, we must provide a value that we want to save and a key that identifies it.

In our case, the value will be a string—the key is always a string.

The first thing we have to do is `getInstance` of the `SharedPreferences`:

{% highlight dart %}
final prefs = await SharedPreferences.getInstance();
{% endhighlight %}

Here, the code await will hold the program's execution until we have the `SharedPreferences` instance. If you don't understand asynchronous programming very well, you can read more about it in the Dart documentation.

After having the `SharedPreferences` instance, we'll want to save the string value with a given key:

{% highlight dart %}
prefs.setString(key, value);
{% endhighlight %}

Remember that the value we want to save is a custom object, therefore, we need to encode it into a JSON string, as explained above. The code then becomes:

{% highlight dart %}
prefs.setString(key, json.encode(value));
{% endhighlight %}

Our `write` method will be:

{% highlight dart %}
static write(String key, value) async {
  final prefs = await SharedPreferences.getInstance();
  return await prefs.setString(key, json.encode(value));
}
{% endhighlight %}
<div class="caption">
    Give special attention to the fact that the function is "async".
</div>

The `read` method is similar to the `write` one and its code is the following:

{% highlight dart %}
static read(String key) async {
  final SharedPreferences prefs = await SharedPreferences.getInstance();
  if (prefs.containsKey(key)) {
    return json.decode(prefs.getString(key));
  } else {
    return null;
  }
}

{% endhighlight %}
<div class="caption">
    Here we must check if the key exists and then decode the stored String (JSON string).
</div>

Prior, we should have organized every function used to manage the `SharedPreferences` in a class called `SharedPref`. The code is:

{% highlight dart linenos %}
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';

class SharedPref {
  static read(String key) async {
    final SharedPreferences prefs = await SharedPreferences.getInstance();
    if (prefs.containsKey(key)) {
      return json.decode(prefs.getString(key));
    } else {
      return null;
    }
  }

  static write(String key, value) async {
    final prefs = await SharedPreferences.getInstance();
    return await prefs.setString(key, json.encode(value));
  }

  static remove(String key) async {
    final prefs = await SharedPreferences.getInstance();
    prefs.remove(key);
  }

  static clear() async {
    final prefs = await SharedPreferences.getInstance();
    prefs.clear();
  }
{% endhighlight %}
<div class="caption">
    Also added the "remove" and "clear" methods.
</div>

<center>
<h1>
. . .
</h1>
</center>

# **Demo: Use it in an App**

Since the `SharedPref` methods are static, we can save a user object to our `SharedPreferences` very simply:

{% highlight dart %}
SharedPref.save("user", userSave);
{% endhighlight %}

The tricky part of using this method for saving objects in a real app is retrieving the object itself, and not a `Future<Dynamic>`. The trick is having an asynchronous function in our app that will retrieve our object and update the application's state when it's ready.

The function will look something like:

{% highlight dart %}
loadSharedPrefs() async {
  try {
    User user = User.fromJson(await SharedPref.read("user"));
    setState(() {
      userLoad = user;
    });
  } catch (Excepetion) {
    // do something
  }
}
{% endhighlight %}

As before, the `await` will make sure that the function's execution is on hold until we retrieved the value corresponding to the key "user" from the `SharedPreferences`. After it, we create a user object from the retrieved JSON string and update our application's `State`.

I created a simple demo application to showcase all the above. Here is a gif of the app in action:

<div class="row mt-3">
    <div class="col-sm mt-3 pt-md-0 pl-xl-5 pr-xl-5">
    <div class="col-sm mt-3 pt-md-0 pl-xl-5 pr-xl-5">
    <div class="col-sm mt-3 pt-md-0 pl-xl-5 pr-xl-5">
    <div class="col-sm mt-3 pt-md-0 pl-xl-5 pr-xl-5">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/assets/img/posts/2019-09-07-sharedPref/application.gif">
    </div>
    </div>
    </div>
    </div>

</div>
<br>

And here is the code:

{% highlight dart linenos %}
import 'package:flutter/material.dart';
import 'SharedPref.dart';
import 'user.dart';

class Demo extends StatefulWidget {
  @override
  DemoView createState() {
    return DemoView();
  }
}

class DemoView extends State<Demo> {
  SharedPref sharedPref = SharedPref();
  User userSave = User();
  User userLoad = User();

  loadSharedPrefs() async {
    try {
      User user = User.fromJson(await SharedPref.read("user"));
      Scaffold.of(context).showSnackBar(SnackBar(
          content: new Text("Loaded!"),
          duration: const Duration(milliseconds: 500)));
      setState(() {
        userLoad = user;
      });
    } catch (Excepetion) {
      Scaffold.of(context).showSnackBar(SnackBar(
          content: new Text("Nothing found!"),
          duration: const Duration(milliseconds: 500)));
    }
  }

  @override
  Widget build(BuildContext context) {
    return ListView(
      children: <Widget>[
        Container(
          height: 200.0,
          child: Column(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              Container(
                  height: 50.0,
                  width: 300.0,
                  child: TextField(
                    decoration: InputDecoration(hintText: "Name"),
                    onChanged: (value) {
                      setState(() {
                        userSave.name = value;
                      });
                    },
                  )),
              Container(
                  height: 50.0,
                  width: 300.0,
                  child: TextField(
                    decoration: InputDecoration(hintText: "Age"),
                    onChanged: (value) {
                      setState(() {
                        userSave.age = value;
                      });
                    },
                  )),
              Container(
                  height: 50.0,
                  width: 300.0,
                  child: TextField(
                    decoration: InputDecoration(hintText: "Location"),
                    onChanged: (value) {
                      setState(() {
                        userSave.location = value;
                      });
                    },
                  )),
            ],
          ),
        ),
        Container(
          height: 80.0,
          child: Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              RaisedButton(
                onPressed: () {
                  SharedPref.save("user", userSave);
                  Scaffold.of(context).showSnackBar(SnackBar(
                      content: new Text("Saved!"),
                      duration: const Duration(milliseconds: 500)));
                },
                child: Text('Save', style: TextStyle(fontSize: 20)),
              ),
              RaisedButton(
                onPressed: () {
                  loadSharedPrefs();
                },
                child: Text('Load', style: TextStyle(fontSize: 20)),
              ),
              RaisedButton(
                onPressed: () {
                  SharedPref.remove("user");
                  Scaffold.of(context).showSnackBar(SnackBar(
                      content: new Text("Cleared!"),
                      duration: const Duration(milliseconds: 500)));
                  setState(() {
                    userLoad = User();
                  });
                },
                child: Text('Clear', style: TextStyle(fontSize: 20)),
              ),
            ],
          ),
        ),
        SizedBox(
          height: 300.0,
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.center,
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              Text("Name: " + (userLoad.name ?? ""),
                  style: TextStyle(fontSize: 16)),
              Text("Age: " + (userLoad.age ?? ""),
                  style: TextStyle(fontSize: 16)),
              Text("Location: " + (userLoad.location ?? ""),
                  style: TextStyle(fontSize: 16)),
            ],
          ),
        ),
      ],
    );
  }
}
{% endhighlight %}
