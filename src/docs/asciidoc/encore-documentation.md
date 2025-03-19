# SVT Encore Documentation

## License

_This work is licensed under the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) License. To view a copy of this license, visit https://creativecommons.org/licenses/by-sa/4.0/ or send a letter to Creative Commons, PO Box 1866, Mountain View, CA 94042, USA._

Copyright 2021 Sveriges Television AB

## Introduction

**❗ IMPORTANT**\
This documentation is in a work-in-progress state. Not all features are yet described.

**This is a living, breathing guide**. If you’d like to contribute,  [fork us on GitHub!](https://github.com/svt/encore-doc)

**_What is Encore?_**

**Encore** is a scalable video transcodingfootnote:[Encoding = source file is uncompressed, Transcoding = source file is compressed. The distinction might not matter much in practice, but we prefer to use the term Transcoding in the Encore documentation] tool, built on Open Source giants like [FFmpeg](https://www.ffmpeg.org/) and [Redisson](https://github.com/redisson).

**_Why does it exist?_**

**Encore** was created to scale, and abstract the transcoding _power of FFmpeg_, and to offer a simple solution for Transcoding - Transcoding-as-a-Service.

See the [projecthistory](#projecthistory) for more about that.

**_Who is it for?_**

**Encore** is aimed at the advanced technical user that needs a scalable video transcoding tool - for example, as a part of their VOD (Video On Demand) transcoding pipeline.

**_How can you use it?_**

For example, you could deploy a number of **Encore** instances within your existing Kubernetes/Docker/other container solution and let the running instances communicate with a Redis cluster (as a Queue message broker). Every instance can than pick the next job lot, and you can scale your transcoding as needed.

**_Features_**

* Scalable - queuing and concurrency options
* Flexible profile configuration
* Possibility to extend FFmpeg functionality
* Tested and tried in production

**_Encore is not_**

* A live/stream transcoder
* A Video packager (see [faq](#faq))
* A GUI application

**_Built with_**

* Kotlin
* Gradle
* Spring Boot
* FFmpeg
* Redisson
* and many other great projects

**⚠️ WARNING**\
API, Profiles, and other formats will change now and then before we reach version 1.0.0 - **Encore** is currently considered a work-in-progress tool.
It **is** used in daily production though.

_Encore - built on Open Source Giants_

![align="center"](img/svt_encore_at_a_glance.png)

_There are no more guides.
You are now on your own.
Thank you!._

### Asciidoc Debug

**Built-in**

* **asciidoctor-version**\
{asciidoctor-version}
* **safe-mode-name**\
{safe-mode-name}
* **docdir**\
{docdir}
* **docfile**\
{docfile}
* **imagesdir**\
{imagesdir}
