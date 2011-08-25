!SLIDE
# sbt-protobuf

&nbsp;

a quick walk through

&nbsp;  
[gseitz/sbt-protobuf](http://github.com/gseitz/sbt-protobuf) on github

!SLIDE

Convert these 15 lines...
  
```protobuf
	package contacts;
	
	message Person {
		required string name = 1;
		required int32 age = 2;
		optional Address primary_address = 3;
		repeated Address secondary_addresses = 4;
	}
	
	message Address {
		required string country = 1;
		required string city = 2;
		required string zip = 3;
		required string street = 4;
	}
```

!SLIDE

...into ~1600 lines

![pic](hello/protobuf.png "I don't want to have to write this.")

!SLIDE

## &gt;&gt;NO&lt;&lt; manual labor please!

!SLIDE

## Use youre favourite build tool!

!SLIDE

# SBT 

Simple Build Tool  
&nbsp;  
[http://github.com/harrah/xsbt](http://github.com/harrah/xsbt)
