!SLIDE

## Tasks

+ Generate Java source files
+ Clean generated java source

!SLIDE

# Generate Java source files #1

Declaration

```scala
val generate = TaskKey[Seq[File]]("generate")

generate in protobufConfig <<= sourceGeneratorTask()
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
# Generate Java source files #4

```scala
def compile(srcDir: File, target: File, includePaths: Seq[File], log: Logger) =  {

  val schemas = (srcDir ** "*.proto").get
  val incPath = includePaths.map(_.absolutePath).mkString("-I", " -I", "")

  <x>protoc {incPath} --java_out={target.absolutePath} {schemas.map(_.absolutePath).mkString(" ")}</x> ! log
  
  (target ** "*.java").get.toSet
}
```

!SLIDE

# Clean up

No need to create a clean task.

```scala

cleanFiles <+= (javaSource in protobufConfig).identity,

```
&nbsp;  
_WARNING:_ Don't set it to `src/main/java` if you value its content!

