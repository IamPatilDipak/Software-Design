## S — Single responsibility principle

Single Responsibility Principle was defined by Robert C. Martin as – A class should have only one reason to change.
    
The Single Responsibility Principle states that every module or class should have responsibility over a single part of the functionality  provided by the software.

You may have heard the quote: “Do one thing and do it well”.

In the article Principles of Object Oriented Design, Robert C. Martin defines a responsibility as a *__‘reason to change’__, and concludes that a class or module should have one, and only one, reason to be changed.*

A class should have a single responsibility, where a responsibility is nothing but a reason to change.

Let’s take an example of how to write a piece of code that violates this principle.

```
class User {
    void CreatePost(Database db, string postMessage) {
        try {
            db.Add(postMessage);
        }catch (Exception e) {
            db.LogError("An error occured: ", e.ToString());
            File.WriteAllText("LocalErrors.txt", e.ToString());
        }
    }
}
```

We notice that how the `CreatePost()` method has too much responsibility, given that it can both create a new post, log an error in the database, and log an error in a local file. This violates the single responsibility principle.

Let’s try to correct it.

```
class Post {
    private ErrorLogger errorLogger = new ErrorLogger();

    void CreatePost(Database db, string postMessage) {
        try {
            db.Add(postMessage);
        }catch (Exception e)
        {
            errorLogger.log(e.ToString())
        }
    }
}

class ErrorLogger {
    void log(string error) {
      db.LogError("An error occured: ", error);
      File.WriteAllText("LocalErrors.txt", error);
    }
}
```

By abstracting the functionality that handles the error logging, we no longer violate the single responsibility principle.
Now we have two classes that each has one responsibility; to create a post and to log an error, respectively.

