Prototype "thin" jar launcher for java apps.

## Getting Started

Build this project locally:

```
$ ./mvnw clean install
```

Then run the "app" jar:

```
$ java -jar ./app/target/*.jar
```

(It starts an empty Spring Boot app with Tomcat.)

## How does it Work?

Inspect the "app" jar. It is just a regular jar file with the app
classes in it and two extra features:

1. The `ThinJarWrapper` class has been added.
2. There is a `META-INF/lib.properties` which lists the dependencies of the app.

When the app runs the main method is in the `ThinJarWrapper`. It's job
is to download another jar file that you just built (the "launcher"),
or locate it in your local Maven repo if it can. The wrapper downloads
the launcher (if it needs to), or else uses the cached version in your
local Maven repository.

The launcher then takes over and reads the `lib.properties`,
downloading the dependencies (and all transitives) as necessary, and
setting up a new class loader with them all on the classpath. It then
runs the application's own main method with that class loader.

## Caching

All jar files are cached in the local Maven repository, so if you are
building and running the same app repeatedly, it should be faster
after the first time, or if the local repo is already warm.

## Upgrades

You can upgrade all the libraries by changing the `lib.properties`.
