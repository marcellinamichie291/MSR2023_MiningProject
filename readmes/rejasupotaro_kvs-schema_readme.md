KVS Schema
==========

KVS Schema is a library to manage key-value data for Android.
This library generates accessor methods of SharedPreferences from schema class in compile time.

How to use
--------

### Create Schema

Let's say you will store user id, You have to create a schema class like below.

```java
@Table(name = "example")
public class ExamplePrefsSchema {
    @Key(name = "user_id") int userId;
}
```

Class name should be `*Schema`.

KVS Schema generates `ExamplePrefs` from `ExamplePrefsSchema`. You can use generated accessors below.

```java
ExamplePrefs prefs = ExamplePrefs.get(context);
prefs.putUserId(userId);
prefs.putUserId(userId, defaultValue);
prefs.getUserId();
prefs.hasUserId();
prefs.removeUserId();
```

KVS supports `boolean` `String` `float` `int` `long` `String` `set`.

### Default Values

You can specify default values for the case of that a given key doesn't be contained in a SharedPreferences.

```java
prefs.getUserId(defaultValue);
```

You can also specify default values through a schema class. KVS Schema can read right values that become a constant in compile time.

```java
@Table(name = "example")
public abstract class ExamplePrefsSchema {
    @Key(name = "user_id") final int userId = -1;
}
```

Using the mechanism, you don't have to specify default values when you set right values that is *a final field initialized to a compile time constant*.

### Serializer

Sometimes you want to put model classes into SharedPreferences. For that cases, KVS Schema can store data using serializer class.

```java
// Schema class
@Table(name = "example")
public abstract class ExamplePrefsSchema {
    @Key(name = "user", serializer = UserPrefsSerializer.class) String user;
}

// Serializer class
public class UserSerializer implements PrefsSerializer<User, String> {
    @Override public String serialize(User src) { return GSON.toJson(src); }
    @Override public User deserialize(String src) { return GSON.fromJson(src, User.class); }
}

// Usage
prefs.putUser(user);
```

### Builder

You can specify builder class to use custom SharedPreferences.

```java
// Schema class
@Table(name = "example", builder = ExamplePrefsBuilder.class)
public abstract class ExamplePrefsSchema {
    @Key(name = "user_id") int userId;
}

// Builder class
public class ExamplePrefsBuilder implements PrefsBuilder<ExamplePrefs>　{
    @Override
    public ExamplePrefs build(Context context) {
        ...
        return new ExamplePrefs(...); // You can pass your SharedPreferences here
    }
}
```

### Using from Kotlin

KVS Schema generates `set*` method as an alias of `put*` to utilize property syntax of Kotlin. So you can read/write values like below when you use Kotlin.

```java
prefs.userId = "Kotlin"
prefs.userId // => Kotlin
```

Installation
--------

Add dependencies to your build.gradle.

```groovy
annotationProcessor 'com.rejasupotaro:kvs-schema-compiler:5.1.0'
compile 'com.rejasupotaro:kvs-schema:5.1.0'
```

Migration
--------

Even if you have already used SharedPreferences directly in your existing app, migration is easy. KVS Schema simply maps the structure of SharedPreferences.

For example, if you are using default SharedPreferences like below,

```java
prefs = PreferenceManager.getDefaultSharedPreferences(this);
Editor editor = prefs.edit();
editor.putString("user_id", "1");
editor.putString("user_name", "Smith");
editor.apply();
```

your data is saved on `path/to/app/shared_prefs/package_name_preferences.xml`. The schema class becomes like below.

```java
@Table(name = "package_name_preferences")
public abstract class ExamplePrefsSchema {
    @Key(name = "user_id") int userId;
    @Key(name = "user_name") String userName;
}
```
