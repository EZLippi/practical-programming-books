 <p>In this post you will learn how to use a micro framework called <a href="http://www.sparkjava.com/">Spark</a> to build a RESTful backend. The RESTful backend is consumed by a single page web application using AngularJS and MongoDB for data storage. I&#8217;ll also show you how to run Java 8 on OpenShift.</p>
<h2>Application Use Case</h2>
<p>You will develop a todo application which allows users to create and list todo items. The application will do the following:</p>
<ul>
<li>When a user goes to the &#8216;/&#8217; url of the application, they will see a list of all todos stored in the application database. Behind the curtain, AngularJS makes a REST(/api/v1/todos) call to fetch all the todo items.</li>
</ul>
<p><img style="display: block; margin-left: auto; margin-right: auto;" src="/wp-content/uploads/imported/spark-blog-list-all-todos.png" alt="TodoApp List All Todos" width="600" /></p>
<ul>
<li>When a user clicks on the checkbox then a todo is marked done. AngularJS makes a PUT request and update the todo item.</li>
</ul>
<p><img style="display: block; margin-left: auto; margin-right: auto;" src="/wp-content/uploads/imported/spark-blog-mark-todo-done.png" alt="TodoApp" width="600" /></p>
<ul>
<li>Finally, a user can add a new todo by navigating to http://todoapp-shekharblogs.rhcloud.com/#/create. This makes a POST call to the RESTful backend and saves the todo in the MongoDB datastore.</li>
</ul>
<p><img style="display: block; margin-left: auto; margin-right: auto;" src="/wp-content/uploads/imported/spark-blog-create-new-todo.png" alt="TodoApp Add New Todo" width="600" /></p>
<h2>What is Spark?</h2>
<p>Spark is a Java based microframework for building web applications with minimum fuss. It is inspired by a framework written in Ruby called Sinatra. It has a minimalist core providing all the essential features required to build a web application quickly with little code.</p>
<h2>Prerequisite</h2>
<p>To build the sample application developed in this blog, you would need the following on your machine.</p>
<ol>
<li>Download and install <a href="https://jdk8.java.net/download.html">JDK 8</a></li>
<li>Download and install Eclipse or any other IDE. Eclipse users make sure you have <a href="https://www.eclipse.org/downloads/index-java8.php">Eclipse with Java 8 support</a>.</li>
<li>Download and install <a href="http://maven.apache.org/download.cgi">Apache Maven</a></li>
<li>Download and install <a href="http://www.mongodb.org/downloads">MongoDB</a></li>
</ol>
<h2>Github Repository</h2>
<p>The code for today&#8217;s demo application is available on <a href="https://github.com/shekhargulati/todoapp-spark">github: todoapp-spark</a>.</p>
<h2>Step 1: Create a Maven project</h2>
<p>Open a new command-line terminal and navigate to the location where you want to create the project. Execute the command shown below to generate the template application.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">$ mvn archetype:generate -DgroupId=com.todoapp -DartifactId=todoapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false</pre>
</div>
<h2>Step 2: Update the maven project to use Java 8</h2>
<p>Import the project into your IDE and replace the pom.xml with the one shown below.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd"&gt;
    &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
    &lt;groupId&gt;com.todoapp&lt;/groupId&gt;
    &lt;artifactId&gt;todoapp&lt;/artifactId&gt;
    &lt;packaging&gt;jar&lt;/packaging&gt;
    &lt;version&gt;1.0-SNAPSHOT&lt;/version&gt;
    &lt;name&gt;todoapp&lt;/name&gt;
    &lt;url&gt;http://maven.apache.org&lt;/url&gt;
 
    &lt;properties&gt;
        &lt;maven.compiler.source&gt;1.8&lt;/maven.compiler.source&gt;
        &lt;maven.compiler.target&gt;1.8&lt;/maven.compiler.target&gt;
    &lt;/properties&gt;
 
    &lt;dependencies&gt;
 
    &lt;/dependencies&gt;
&lt;/project&gt;</pre>
</div>
<p>In the pom.xml shown above, we changed the following:</p>
<ol>
<li>You updated Maven to use Java 8 by defining maven.compiler.source and maven.compiler.target properties.</li>
<li>You removed the JUnit dependency from the original pom.xml file.</li>
</ol>
<p>Delete App.java and AppTest.java files as we don&#8217;t need them.</p>
<h2>Step 3: Add Spark and other dependencies</h2>
<p>The application uses the Spark framework and a MongoDB database. So, update the dependencies section of pom.xml with the one shown below.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">&lt;dependencies&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.sparkjava&lt;/groupId&gt;
            &lt;artifactId&gt;spark-core&lt;/artifactId&gt;
            &lt;version&gt;2.0.0&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.slf4j&lt;/groupId&gt;
            &lt;artifactId&gt;slf4j-simple&lt;/artifactId&gt;
            &lt;version&gt;1.7.5&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;org.mongodb&lt;/groupId&gt;
            &lt;artifactId&gt;mongo-java-driver&lt;/artifactId&gt;
            &lt;version&gt;2.11.3&lt;/version&gt;
        &lt;/dependency&gt;
        &lt;dependency&gt;
            &lt;groupId&gt;com.google.code.gson&lt;/groupId&gt;
            &lt;artifactId&gt;gson&lt;/artifactId&gt;
            &lt;version&gt;2.2.4&lt;/version&gt;
        &lt;/dependency&gt;
&lt;/dependencies&gt;</pre>
</div>
<p>Spark uses SLF4J for logging, so we added slf4j-simple binding in the dependencies. This is required to view the log and error messages. Also, we added the gson library as its used to convert objects to and from JSON.</p>
<h2>Step 4: Make Spark Say Hello World</h2>
<p>Create a new class called Bootstrap and add the following code to it.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import spark.Request;
import spark.Response;
import spark.Route;
 
import static spark.Spark.*;
 
public class Bootstrap {
 
    public static void main(String[] args) {
        get("/", new Route() {
            @Override
            public Object handle(Request request, Response response) {
                return "Hello World!!";
            }
        });
    }
}</pre>
</div>
<p>This code:</p>
<ol>
<li>Imports the required classes from the Spark library.</li>
<li>Creates a new class called Bootstrap and defines a main method.</li>
<li>Defines a route that tells Spark that when an HTTP GET request is made to &#8216;/&#8217;, return &#8220;Hello World&#8221;. You use Spark&#8217;s get() method to define the mapping from the URL to the callback.</li>
</ol>
<p>To see the application in action, run the main program using your IDE. The application will start the embedded Jetty server at http://0.0.0.0:4567. When you open this link in your web browser, you will see &#8220;Hello World!!&#8221;.</p>
<p>Take advantage of Java 8 lambda expressions to make your code more concise and clean. Spark is a modern Java web framework that takes advantage of Java 8 features.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import spark.Request;
import spark.Response;
import spark.Route;
 
import static spark.Spark.*;
 
public class Bootstrap {
 
    public static void main(String[] args) {
        get("/", (request, response) -&gt; "Hello World");
    }
}</pre>
</div>
<h2>Step 5: Defining the Domain Model and Service class</h2>
<p>Our goal is an application that stores and manages ToDo items. Our simple ToDo class is shown below.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import com.mongodb.BasicDBObject;
import com.mongodb.DBObject;
import org.bson.types.ObjectId;
 
import java.util.Date;
 
public class Todo {
 
    private String id;
    private String title;
    private boolean done;
    private Date createdOn = new Date();
 
    public Todo(BasicDBObject dbObject) {
        this.id = ((ObjectId) dbObject.get("_id")).toString();
        this.title = dbObject.getString("title");
        this.done = dbObject.getBoolean("done");
        this.createdOn = dbObject.getDate("createdOn");
    }
 
    public String getTitle() {
        return title;
    }
 
    public boolean isDone() {
        return done;
    }
 
    public Date getCreatedOn() {
        return createdOn;
    }
}</pre>
</div>
<p>Our TodoService class implement methods that use CRUD operations on the Todo object. It basically Creates, Reads, and Updates Todo documents stored in MongoDB.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import com.google.gson.Gson;
import com.mongodb.*;
import org.bson.types.ObjectId;
 
import java.util.ArrayList;
import java.util.Collections;
import java.util.Date;
import java.util.List;
 
public class TodoService {
 
    private final DB db;
    private final DBCollection collection;
 
    public TodoService(DB db) {
        this.db = db;
        this.collection = db.getCollection("todos");
    }
 
    public List&lt;Todo&gt; findAll() {
        List&lt;Todo&gt; todos = new ArrayList&lt;&gt;();
        DBCursor dbObjects = collection.find();
        while (dbObjects.hasNext()) {
            DBObject dbObject = dbObjects.next();
            todos.add(new Todo((BasicDBObject) dbObject));
        }
        return todos;
    }
 
    public void createNewTodo(String body) {
        Todo todo = new Gson().fromJson(body, Todo.class);
        collection.insert(new BasicDBObject("title", todo.getTitle()).append("done", todo.isDone()).append("createdOn", new Date()));
    }
 
    public Todo find(String id) {
        return new Todo((BasicDBObject) collection.findOne(new BasicDBObject("_id", new ObjectId(id))));
    }
 
    public Todo update(String todoId, String body) {
        Todo todo = new Gson().fromJson(body, Todo.class);
        collection.update(new BasicDBObject("_id", new ObjectId(todoId)), new BasicDBObject("$set", new BasicDBObject("done", todo.isDone())));
        return this.find(todoId);
    }
}</pre>
</div>
<p>This code does the following:</p>
<ol>
<li>The TodoService class constructor receives the MongoDB database object and stores that in the instance variable. You use the db.getCollection() method to fetch the todos collection. All of the operation are done on the todos collection.</li>
<li>The findAll() method fetches all of the todo documents from the MongoDB database. The documents fetched from MongoDB are of the DBObject type. You iterated over the DBCursor object and converted the individual documents to Todo objects and then added them to a List. Finally, you returned the list of todos.</li>
<li>The createNewTodo() method receives a JSON string representing the Todo item. You used GSON to convert the JSON to a Todo object. Finally, you inserted the BasicDBObject into the todos collection.</li>
<li>The find() method finds the Todo item corresponding to a given id.</li>
<li>The update() method updates the Todo document for the given todo Id. It also updates the done field of the document.</li>
</ol>
<h2>Step 6: Creating the Resource class</h2>
<p>It is generally not a good idea to add all of the code to one class so we will move the application REST endpoints to another class. This new class is called TodoResource and exposes CRUD operations over Todo objects.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import com.google.gson.Gson;
import spark.Request;
import spark.Response;
import spark.Route;
 
import java.util.HashMap;
 
import static spark.Spark.get;
import static spark.Spark.post;
import static spark.Spark.put;
 
public class TodoResource {
 
    private static final String API_CONTEXT = "/api/v1";
 
    private final TodoService todoService;
 
    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
        setupEndpoints();
    }
 
    private void setupEndpoints() {
        post(API_CONTEXT + "/todos", "application/json", (request, response) -&gt; {
            todoService.createNewTodo(request.body());
            response.status(201);
            return response;
        }, new JsonTransformer());
 
        get(API_CONTEXT + "/todos/:id", "application/json", (request, response)
 
                -&gt; todoService.find(request.params(":id")), new JsonTransformer());
 
        get(API_CONTEXT + "/todos", "application/json", (request, response)
 
                -&gt; todoService.findAll(), new JsonTransformer());
 
        put(API_CONTEXT + "/todos/:id", "application/json", (request, response)
 
                -&gt; todoService.update(request.params(":id"), request.body()), new JsonTransformer());
    }
 
 
}</pre>
</div>
<p>The code shown above exposes the TodoService CRUD methods as REST API&#8217;s.</p>
<p>JsonTransformer in the code shown above is an implementation of Spark&#8217;s ResponseTransformer interface. It allows you to convert response objects to other formats like JSON.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import com.google.gson.Gson;
import spark.Response;
import spark.ResponseTransformer;
 
import java.util.HashMap;
 
public class JsonTransformer implements ResponseTransformer {
 
    private Gson gson = new Gson();
 
    @Override
    public String render(Object model) {
        return gson.toJson(model);
    }
 
}</pre>
</div>
<h2>Step 7: Bootstrap the application</h2>
<p>Now we will write the entry point for our application. The Bootstrap class shown below configures all of the components. When you run this class as a Java application, it starts the Jetty server and start listening to requests.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">package com.todoapp;
 
import com.mongodb.*;
 
import static spark.Spark.setIpAddress;
import static spark.Spark.setPort;
import static spark.SparkBase.staticFileLocation;
 
public class Bootstrap {
    private static final String IP_ADDRESS = System.getenv("OPENSHIFT_DIY_IP") != null ? System.getenv("OPENSHIFT_DIY_IP") : "localhost";
    private static final int PORT = System.getenv("OPENSHIFT_DIY_PORT") != null ? Integer.parseInt(System.getenv("OPENSHIFT_DIY_PORT")) : 8080;
 
    public static void main(String[] args) throws Exception {
        setIpAddress(IP_ADDRESS);
        setPort(PORT);
        staticFileLocation("/public");
        new TodoResource(new TodoService(mongo()));
    }
 
    private static DB mongo() throws Exception {
        String host = System.getenv("OPENSHIFT_MONGODB_DB_HOST");
        if (host == null) {
            MongoClient mongoClient = new MongoClient("localhost");
            return mongoClient.getDB("todoapp");
        }
        int port = Integer.parseInt(System.getenv("OPENSHIFT_MONGODB_DB_PORT"));
        String dbname = System.getenv("OPENSHIFT_APP_NAME");
        String username = System.getenv("OPENSHIFT_MONGODB_DB_USERNAME");
        String password = System.getenv("OPENSHIFT_MONGODB_DB_PASSWORD");
        MongoClientOptions mongoClientOptions = MongoClientOptions.builder().build();
        MongoClient mongoClient = new MongoClient(new ServerAddress(host, port), mongoClientOptions);
        mongoClient.setWriteConcern(WriteConcern.SAFE);
        DB db = mongoClient.getDB(dbname);
        if (db.authenticate(username, password.toCharArray())) {
            return db;
        } else {
            throw new RuntimeException("Not able to authenticate with MongoDB");
        }
    }
}</pre>
</div>
<h2>Step 8 : Setup AngularJS and Twitter Bootstrap</h2>
<p>Create a new directory with name <em>public</em> and place the javascript and css files in it. You can checkout the required directory structure from the <a href="https://github.com/shekhargulati/todoapp-spark/tree/master/src/main/resources">Github repository</a>. Download the latest copy of <a href="https://angularjs.org/">AngularJS</a> and <a href="http://getbootstrap.com/">Bootstrap</a> from their respective official websites, or you can copy the resources from this project <a href="https://github.com/shekhargulati/todoapp-spark/tree/master/src/main/resources/public">github repository</a>.</p>
<h2>Step 9: Create index.html</h2>
<p>Create a new file called index.html inside of the src/main/resources/public directory and place the following content into it.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">&lt;!DOCTYPE html&gt;
&lt;html ng-app="todoapp"&gt;
&lt;head&gt;
    &lt;title&gt;Todo App&lt;/title&gt;
    &lt;link rel="stylesheet" href="/css/bootstrap.css"&gt;
    &lt;link rel="stylesheet" href="/css/main.css"&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div class="navbar navbar-inverse"&gt;
    &lt;div class="container-fluid"&gt;
        &lt;div class="navbar-header"&gt;
            &lt;button type="button" class="navbar-toggle"&gt;
                &lt;span class="sr-only"&gt;Toggle navigation&lt;/span&gt;
                &lt;span class="icon-bar"&gt;&lt;/span&gt;
                &lt;span class="icon-bar"&gt;&lt;/span&gt;
                &lt;span class="icon-bar"&gt;&lt;/span&gt;
            &lt;/button&gt;
            &lt;a class="navbar-brand" href="/"&gt;TodoApp&lt;/a&gt;
        &lt;/div&gt;
    &lt;/div&gt;
&lt;/div&gt;
 
&lt;div class="container" ng-view=""&gt;
 
 
&lt;/div&gt;
 
&lt;script src="/js/jquery.js"&gt;&lt;/script&gt;
&lt;script src="/js/angular.js"&gt;&lt;/script&gt;
&lt;script src="/js/angular-route.js"&gt;&lt;/script&gt;
&lt;script src="/js/angular-cookies.js"&gt;&lt;/script&gt;
&lt;script src="/js/angular-sanitize.js"&gt;&lt;/script&gt;
&lt;script src="/js/angular-resource.js"&gt;&lt;/script&gt;
 
&lt;script src="/scripts/app.js"&gt;&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
</div>
<p>In the html shown above:</p>
<ol>
<li>You imported all of the required libraries. Our application code is in /scripts/app.js. The app.js file will be created in step 10.</li>
<li>In Angular, you defined the scope of the project using the ng-app directive. You used ng-app on the html element but we can use it with any other element as well. Using the ng-app directive with html element means that AngularJS is available on the whole index.html. The ng-app directive can take a name. This name is the module name. I used todoapp as this application module name.</li>
<li>The last interesting thing in the index.html is the use of the ng-view directive. The ng-view directive renders the template corresponding to the current route inside index.html. So that everytime you navigate to a route only the ng-view portion changes.</li>
</ol>
<h2>Step 10: Write the Angular application</h2>
<p>The app.js houses all of the application specific JavaScript. All of the application routes are defined inside it. In the code shown below, we have defined two routes and each has a corresponding Angular controller.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">/**
 * Created by shekhargulati on 10/06/14.
 */
 
var app = angular.module('todoapp', [
    'ngCookies',
    'ngResource',
    'ngSanitize',
    'ngRoute'
]);
 
app.config(function ($routeProvider) {
    $routeProvider.when('/', {
        templateUrl: 'views/list.html',
        controller: 'ListCtrl'
    }).when('/create', {
        templateUrl: 'views/create.html',
        controller: 'CreateCtrl'
    }).otherwise({
        redirectTo: '/'
    })
});
 
app.controller('ListCtrl', function ($scope, $http) {
    $http.get('/api/v1/todos').success(function (data) {
        $scope.todos = data;
    }).error(function (data, status) {
        console.log('Error ' + data)
    })
 
    $scope.todoStatusChanged = function (todo) {
        console.log(todo);
        $http.put('/api/v1/todos/' + todo.id, todo).success(function (data) {
            console.log('status changed');
        }).error(function (data, status) {
            console.log('Error ' + data)
        })
    }
});
 
app.controller('CreateCtrl', function ($scope, $http, $location) {
    $scope.todo = {
        done: false
    };
 
    $scope.createTodo = function () {
        console.log($scope.todo);
        $http.post('/api/v1/todos', $scope.todo).success(function (data) {
            $location.path('/');
        }).error(function (data, status) {
            console.log('Error ' + data)
        })
    }
});</pre>
</div>
<p>The code shown above does the following:</p>
<ol>
<li>First, it configures the Angular module named todoapp and defines all of it&#8217;s dependencies.</li>
<li>It defines the routes that this application will respond to.</li>
<li>When a user makes request to &#8216;/&#8217;, the ListCtrl will be invoked. The ListCtrl will make an HTTP GET request to &#8216;/api/v1/todos&#8217; to fetch all of the todo items. The todo items are put on the scope. The list.html uses the ng-repeat directive to iterate over all of the todo items and generate a table.</li>
<li>When a user clicks on the checkbox next to the item, the todoStatusChanged() function is invoked. This function makes an HTTP PUT request to update the todo item.</li>
<li>When a user makes a GET request to &#8216;/create&#8217;, create.html is rendered. The create.html renders a bootstrap form. The form HTML element uses the ng-submit directive. On form submit the createTodo function is invoked. The createTodo function makes an HTTP POST request to create a new todo item.</li>
</ol>
<p>You can find the respective views for different routes in the application&#8217;s <a href="https://github.com/shekhargulati/todoapp-spark/tree/master/src/main/resources/public/views">Github repository</a>.</p>
<h2>Step 11: Run the application</h2>
<p>You can either run the application using your IDE or package this application as an executable jar and then run it from the command-line. Add the following plugin to your project pom.xml to create an executable JAR. This plugin provides the capability to package the artifact in an uber-jar, including its dependencies and to shade &#8211; i.e. rename &#8211; the packages of some of the dependencies.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">&lt;build&gt;
        &lt;plugins&gt;
            &lt;plugin&gt;
                &lt;groupId&gt;org.apache.maven.plugins&lt;/groupId&gt;
                &lt;artifactId&gt;maven-shade-plugin&lt;/artifactId&gt;
                &lt;version&gt;2.3&lt;/version&gt;
                &lt;configuration&gt;
                    &lt;createDependencyReducedPom&gt;true&lt;/createDependencyReducedPom&gt;
                    &lt;filters&gt;
                        &lt;filter&gt;
                            &lt;artifact&gt;*:*&lt;/artifact&gt;
                            &lt;excludes&gt;
                                &lt;exclude&gt;META-INF/*.SF&lt;/exclude&gt;
                                &lt;exclude&gt;META-INF/*.DSA&lt;/exclude&gt;
                                &lt;exclude&gt;META-INF/*.RSA&lt;/exclude&gt;
                            &lt;/excludes&gt;
                        &lt;/filter&gt;
                    &lt;/filters&gt;
                &lt;/configuration&gt;
                &lt;executions&gt;
                    &lt;execution&gt;
                        &lt;phase&gt;package&lt;/phase&gt;
                        &lt;goals&gt;
                            &lt;goal&gt;shade&lt;/goal&gt;
                        &lt;/goals&gt;
                        &lt;configuration&gt;
                            &lt;transformers&gt;
                                &lt;transformer
                                        implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer"&gt;
                                    &lt;mainClass&gt;com.todoapp.Bootstrap&lt;/mainClass&gt;
                                &lt;/transformer&gt;
                            &lt;/transformers&gt;
                        &lt;/configuration&gt;
                    &lt;/execution&gt;
                &lt;/executions&gt;
            &lt;/plugin&gt;
        &lt;/plugins&gt;
&lt;/build&gt;</pre>
</div>
<p>To create an executable jar, run the mvn clean install command. This creates a todoapp-1.0-SNAPSHOT.jar in the target directory.</p>
<p>To run the application:</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">$ java -jar target/todoapp-1.0-SNAPSHOT.jar</pre>
</div>
<p>This would print following lines in the terminal.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">== Spark has ignited ...
>&gt; Listening on localhost:8080
[Thread-2] INFO org.eclipse.jetty.server.Server - jetty-9.0.z-SNAPSHOT
[Thread-2] INFO org.eclipse.jetty.server.ServerConnector - Started ServerConnector@5bbf3bfb{HTTP/1.1}{localhost:8080}</pre>
</div>
<h2>Step 12: Deploying on OpenShift</h2>
<p>This blog would not be complete if I didn&#8217;t show you how to run this application on OpenShift. Today OpenShift does not support JDK 8 but that doesn&#8217;t mean you can&#8217;t run Java 8 applications. You can use the DIY cartridge and install your own JDK version. The next command creates the todo application you created in the above mentioned steps. It installs JDK 8 on the DIY gear and configures other settings.</p>
<div class="geshifilter">
<pre class="text geshifilter-text" style="font-family: monospace;">$ rhc app create todoapp diy mongodb-2.4 --repo=todoapp-os --from-code=https://github.com/shekhargulati/spark-openshift-quickstart.git</pre>
</div>
<p>After this commands successfully finishes, you will see the todo app running at http://todoapp-{domain-name}.rhcloud.com. Please replace {domain-name} with your OpenShift account domain name.</p>
<h2>Next Steps</h2>
<ul>
<li><a href="https://www.openshift.com/app/account/new">Sign up for OpenShift Online</a> and try this out yourself</li>
<li>Promote and show off your awesome app in the <a href="https://www.openshift.com/application-gallery">OpenShift Application Gallery</a> today.</li>
</ul>