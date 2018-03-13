# Custom Java source layout
```
build.gradle

sourceSets {
    main {
        java {
            srcDirs = ['src/java']
        }
        resources {
            srcDirs = ['src/resources']
        }
    }
}
```

# Accessing a source set

```
build.gradle

// Various ways to access the main source set
println sourceSets.main.output.classesDirs
println sourceSets['main'].output.classesDirs
sourceSets {
    println main.output.classesDirs
}
sourceSets {
    main {
        println output.classesDirs
    }
}

// Iterate over the source sets
sourceSets.all {
    println name
}
```


# Create project structure of Java
```
build.gradle

task "create-dirs" << {
   sourceSets*.java.srcDirs*.each { it.mkdirs() }
   sourceSets*.resources.srcDirs*.each { it.mkdirs() }
}
```