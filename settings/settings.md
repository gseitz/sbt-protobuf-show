!SLIDE

## Settings

+ Path for source *.proto
+ Path for generated *.java
+ Protobuf version to use
+ Path for additional *.proto

!SLIDE

## Reuse, Reuse, Reuse
```scala
val sourceDirectory	: sbt.SettingKey[java.io.File]

val javaSource 		: sbt.SettingKey[java.io.File]

val version 		: sbt.SettingKey[String]
```

!SLIDE

## Protobuf Source

```scala
sourceDirectory in protobufConfig <<= 

	(sourceDirectory in Compile) { _ / "protobuf" }

	/*      src/main         */
```

!SLIDE

## Java Source

```scala
javaSource in protobufConfig <<=

	(sourceManaged in Compile) { _ / "protobuf" }

	/*  target/scala_x.y.z  */
```

!SLIDE

## Version
```scala
version in protobufConfig := "2.4.1"
```

!SLIDE

## Include Path
```scala
val includePaths = TaskKey[Seq[File]]("include-paths")
```	
	

Add our proto source directory by default:

```scala

includePaths in protobufConfig <<= 

	(sourceDirectory in protobufConfig) map (identity(_) :: Nil),
```

!SLIDE

# What we have so far

```scala
object ProtobufPlugin extends Plugin {
  val protobufConfig = config("protobuf")

  val includePaths = TaskKey[Seq[File]]("include-paths")

  lazy val protobufSettings: Seq[Setting[_]] = inConfig(protobufConfig)(Seq(
    sourceDirectory <<= (sourceDirectory in Compile) { _ / "protobuf" },
    javaSource <<= (sourceManaged in Compile) { _ / "compiled_protobuf" },
    version := "2.4.1",
    includePaths <<= sourceDirectory map (identity(_) :: Nil)
  )) ++ Seq(
    libraryDependencies <+= (version in protobufConfig)("com.google.protobuf" % "protobuf-java" % _),
  )
}
```
