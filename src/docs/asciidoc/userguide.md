## The User Guide

This part of the documentation describes how to build the project, and/or just use the pre-built jar to run it directly.

**📌 NOTE**\
The [appendix](#running_encore) gives a few other examples of how you can run Encore

### Building the project

_Note: This step is not needed if you are trying out Encore with the_ [Docker Compose example](#example-using-docker-compose).

* Requires:
  * JDK 11 or later
  * An available Redis instance (default configuration: locally installed port 6379)
  * FFmpeg and Mediainfo (we recommend the versions present in [Homebrew-AVTools](https://github.com/svt/homebrew-avtools) repository, which has been tested to work with Encore).

To build ***Encore***, with all tests, do:

```bash
$ ./gradlew clean build
```

```bash
$ ./gradlew clean build -x test
```

### Run Encore as a runnable jar

You can run **Encore** on your local machine by starting up a local developer Application Profile.
Get the [pre-built encore jar](https://github.com/svt/encore/releases), or first [build the project](#building-the-project) and get the jar from ./build/libs/ 

```bash
foobar@foo:~$ SPRING_PROFILES_ACTIVE=local java -jar encore-x.y.z.jar
```

**📌 NOTE**\
This will use the _src/main/resources/application-local.yml_ configuration profile in **Encore**.

If the startup fails, verify that you are fulfilling the [requirements](#requirements-run-local-boot)

Next, Continue to [posting-your-first-encorejob](#posting-your-first-encorejob)

### Posting your first EncoreJob

* This step requires:
  * A running instance of Encore
  * A inputdir - where you put source files
  * A outputdir - where you want the encoded files to end up

1) Use one of the files from the test resources in the Encore project and copy this to your ***inputdir***
(Verify that you have access permissions to the ***inputdir*** and ***outputdir***.)

```console
$ cp src/test/resources/input/test.mp4 /tmp/input
```

2) Fire up a browser and http://&lt;ENCORE_IP>:8080/swagger-ui/index.html?configUrl=/v3/api-docs/swagger-config#/ - i.e directly to the Encore Swagger Interface for Posting an EncoreJob.

3) Finally, Under POST, hit the "Try it out" button and POST this example. 
(Replace the **outputFolder** and **uri** with your ***outputdir*** and ***inputdir***/filename.)

```json

  {
  	"profile": "program",
  	"outputFolder": "/tmp/output",
  	"baseName": "quicktest_",
  	"inputs": [{
  		"uri": "/tmp/input/test.mp4",
  		"useFirstAudioStreams": 2,
  		"type": "AudioVideo"
  	}]
  }
```

You can now see some output in your terminal, and after a while you should see a couple of encoded files in the ***outputdir***. 
Congratulations, you did your first encoding with Encore!

Now, find out more about specific options, see [dynamicapiendpoints](#dynamicapiendpoints), or see the [concepts](#concepts).
