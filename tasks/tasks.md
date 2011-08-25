!SLIDE

## Tasks

+ Generate Java source files
+ Clean generated java source

!SLIDE

# Generate Java source files

Declaration

```scala
val generate = TaskKey[Seq[File]]("generate")

generate in protobufConfig <<= sourceGeneratorTask
```

&nbsp;  

Register as source generator

```scala
sourceGenerators in Compile <+= (generate in protobufConfig).identity
```

!SLIDE 

# Generate Java source files #2
```scala
def sourceGeneratorTask = 
  (streams,
  sourceDirectory in protobufConfig,
  javaSource in protobufConfig,
  includePaths in protobufConfig,
  cacheDirectory) map {	
  
    (out, srcDir, targetDir, includePaths, cache) => ...
}
```

!SLIDE

# Generate Java source files #3
```scala
(out, srcDir, targetDir, includePaths, cache) =>
  val cachedCompile = FileFunction.cached(cache / "protobuf",
                                          inStyle = FilesInfo.lastModified,
                                          outStyle = FilesInfo.exists) { 
    (in: Set[File]) => compile(srcDir, targetDir, includePaths, out.log)
  }
  cachedCompile((srcDir ** "*.proto").get.toSet).toSeq
```


!SLIDE

# Clean up

No need to create a clean task.

```scala

cleanFiles <+= (javaSource in protobufConfig).identity,

```
&nbsp;  
_WARNING:_ Don't set it to `src/main/java` if you value its content!