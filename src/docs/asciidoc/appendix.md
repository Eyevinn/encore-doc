## Appendix

### Application Properties

Beside the rich configuration possibilities of [Spring Boot and friends](https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html), a few custom properties can be specified inside your application.properties file, inside your application.yml file, or as command line switches.

**Custom Application Properties - **prefix: encore-settings****

| Key | Default | Description |
| --- | --- | --- |
| local-temporary-encode | false | if true, encoding is done in a temporary folder on the Encore instance currently running the job. When done, the results are moved to the where the Encore Job’s output folder path is pointing. |
| audio-mix-presets | a default [audiomixpreset](#audiomixpreset) is created | a list of [audiomixpreset](#audiomixpreset)s to use |
| concurrency | 2 | Max nr of currently running encoding tasks (property starts at 0, so 0 gives 1 task). |
| poll-initial-delay | 10 | initial wait time in seconds before task starts. |
| poll-delay | 5 | wait before polling, in seconds, for next task after compilation of the current task. |
| redis-key-prefix | encore | the prefix to identify your redis queue. Redis keys are named "$redisKeyPrefix-queue-$queueNo" |
| security.enabled | false | enables basic auth, TODO: describe basic security logic |
| security.user-password |  | password for regular user |
| security.admin-password |  | password for the admin user |
| open-api.title | SVT Encore OpenAPI | title shown in the OpenAPI url summary header |
| open-api.description | "Endpoints for Encore" | description shown in the OpenAPI url summary header |
| open-api.contact-name |  | contact name shown in the OpenAPI gui summary header |
| open-api.contact-email |  | contact email shown in the OpenAPI gui summary header |
| open-api.contact-url |  | contact url shown in the OpenAPI gui summary header |

**AudioMixPreset**

| Field | Description | Constraint | Default |
| --- | --- | --- | --- |
| fallback-to-auto | if true, sets the output audio channels to the number given by the [audioencode](#audioencode) field channel. | the following conditions needs to apply for it to take effect:     set to true     nr of audio channels in input file > 0 [audioencode](#audioencode) channels = 2 OR between 1 and the "nr of audio channels in input file" | true |
| default-pan | if configured, maps "autofound input audio channels" to the configured output channel configuration | input file channels: configured [audioencode](#audioencode) channel: the [pan filter](https://ffmpeg.org/ffmpeg-filters.html#pan) configuration to use  See the concrete [profiles](#profiles) for yaml example.  Any configuration found in pan-mapping would take precedence over this configuration |  |
| pan-mapping | if configured, maps "found input audio channels" to the configured matching output channel configurations | input file channels: configured [audioencode](#audioencode) channel: [the pan filter](https://ffmpeg.org/ffmpeg-filters.html#pan) configuration which should be used.   See the concrete [encore-settings configuration example](#configurationexample) for yaml-examples.   Any configuration here takes precedence over configurations in default-pan |  |

_For, clarity, here is how an encore-setting configuration with audio-mix-presets could look like in real life._

<a name="configurationexample"></a>**encore-settings configuration example**

```yaml
```

### Running Encore

As a complement to the UserGuide, here is a few more examples on how you can try out ***Encore***.

* Using Docker-Compose
* Using Spring Boot BootRun
* Building an Docker Image and run Encore in a Docker Container

#### Example: using Docker Compose

* This step requires:
  * [Docker Compose](https://docs.docker.com/compose/install/Docker-compose)
    * Use a docker-compose.yml

```yaml
```

and then

Linux
```console
$ ./docker-compose up
```

Mac
```console
$ ./docker compose up
```

and you should see Encore starting up.

**❗ IMPORTANT**\
By default, there are two folders mapped to your host system by using the example docker-compose file. 
***/tmp/input and /tmp/output****, which we will refer to as the **inputdir*** and the ***outputdir***
_Change these (in the docker-compose file) to something that suits your environment if you are not running on Linux. 
For example /Users/&lt;YOURUSER>/input:/tmp/input should be fine on macos._

##### Get the IP

On Linux:
- To find the IP Docker-Compose creates (so that you can access Encore’s api).

```console
$ docker inspect <nameOfDirectoryYouAreRunningFrom>_encorenet
```

**📌 NOTE**\
By design Docker-compose creates a network called &lt;nameOfDirectoryYouAreRunningFrom>_encorenet

On macos:
Use 'localhost'

Got the IP? - great! Continue to [posting-your-first-encorejob](#posting-your-first-encorejob)

#### Example: run Encore with Spring Boot bootRun

You can run **Encore** on your local machine by starting up a local developer Application Profile:

**In the **Encore** root folder:**

```bash
foobar@foo:~$ SPRING_PROFILES_ACTIVE=local ./gradlew clean bootRun
```

**📌 NOTE**\
This will use the _src/main/resources/application-local.yml_ configuration profile in **Encore**.

If the startup fails, verify that you are fulfilling the [requirements](#requirements-run-local-boot)

Continue to [posting-your-first-encorejob](#posting-your-first-encorejob)

#### Example: run Encore in a Docker Image

* Create or find a base FFmpeg/Mediainfo Docker image.

The given example installs a recent version of FFmpeg, with a few FFmpeg filters, and mediainfo.
Modify as needed, using a tap of [Homebrew](https://docs.brew.sh/Homebrew-on-Linux) as an installation base.

<details>
<summary>A FFmpeg Dockerfile example (click to expand)</summary>

```dockerfile
```

</details>

**With the environment variable DOCKER_BASE_IMAGE pointing to your FFmpeg Docker Image**

```bash
foobar@foo:~$ docker build -t encore-docker --build-arg DOCKER_BASE_IMAGE=<yourdockerbaseimage:youjustbuilt> . 

foobar@foo:~$ docker run --network=host -v /tmp/input:/tmp/input -v /tmp/output:/tmp/output -e SPRING_PROFILES_ACTIVE='local' encore-docker
```

Continue to [posting-your-first-encorejob](#posting-your-first-encorejob)

### Homebrew AVTools

So you noticed that the FFmpeg Docker Example in the quickstart used the Repository Manager Brew?
The creators of **Encore**, have released their Brew Formulas on [GitHub: Homebrew AVTools](https://github.com/svt/homebrew-avtools).

For example, here is the **Encore** FFmpeg Brew Formula that with minor modifications, uses formulas for X264, X265, and SVT’s subtitle filter.

**📌 NOTE**\
To use and install this version of FFmpeg locally, follow the examples given at the [Homebrew AVTools README](https://github.com/svt/homebrew-avtools/blob/main/README.md).

**The SVT **Encore** FFmpeg Brew Formula**

```ruby
```

**🔥 CAUTION**\
If you use Homebrew-AVTools, the version of FFmpeg in use might be updated without **any** notice.

### Profiles

**Profile example, main**

```yaml
```

**Profile example, x264 codec with forced keyframes**

```yaml
```

**Profile example, x264 codec**

```yaml
```

### The Future

We have a few ideas where we would like to go with Encore.
We are focusing on the features we need, but we are always open to suggestion and discussion, if you would like to contribute to the project.
