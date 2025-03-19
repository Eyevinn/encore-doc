##### Audio

The [audioencode](#audioencode) type is, as the name implies, for audio encoding.

The AudioEncode type can exist at two levels in a Profile configuration:

1. As a field, audioEncode, in _encodes:_ [VideoEncode](#videoencode) - the audio stream will be embedded in the output video container.
2. As a separate Encode type in the _encodes_ list - the audio stream will be written to a separate output filestream.

TODO: Describe the new mapping logic

---

**AudioEncode**

| Field | Description | Constraint | Default |
| --- | --- | --- | --- |
| codec | [FFmpeg audio codec library](https://ffmpeg.org/ffmpeg-codecs.html#Audio-Decoders) |  | [libfdk_aac](https://wiki.hydrogenaud.io/index.php?title=Fraunhofer_FDK_AAC) |
| bitrate |  | optional |  |
| samplerate | The audio sample rate in hz |  | 48000 |
| channels | number of channels |  | 2 |
| suffix | suffix for the audio output file |  | {codec]_{nrofchannels}.ch |
| params | map of FFmpeg parameters to the given codec - cutoff etc |  |  |
| filters | any optional filters to FFmpeg. |  |  |
| audioMixPreset | the name of the default [audiomixpreset](#audiomixpreset) to use, if any |  | "default" |
| skipIfNoAudioMixPreset | don’t encode audio if no mix was found | boolean | false |
| format | output file extension |  | mp4 |

---

##### Video

The [videoencode](#videoencode) type is, as the name implies, for video encoding.

VideoEncode is a base type, for building concrete (n)Encode type implementations on. Existing examples are [x264encode](#x264encode) and [x265encode](#x265encode).
It is not intended for direct usage.

**VideoEncode**

| Field | Description | Constraint | Default |
| --- | --- | --- | --- |
| width | adds the [scale filter](https://ffmpeg.org/ffmpeg-filters.html#scale) (if scaling enabled) |  |  null, or -2 if only height is given |
| height | adds the [scale filter](https://ffmpeg.org/ffmpeg-filters.html#scale) (if scaling enabled) |  |  null, or -2 if only height is given |
| twoPass | true, false | If FFmpeg transcoding should be [twoPass](https://en.wikipedia.org/wiki/Variable_bitrate#Multi-pass_encoding_and_single-pass_encoding) | true |
| params | general FFmpeg Encoding parameters (see [examples](#profiles), vsync etc) |  |  |
| filters | for adding extra [FFmpeg Filters](https://ffmpeg.org/ffmpeg-filters.html) |  |  |
| audioEncode |  | [audioencode](#audioencode) or null |  |
| suffix | suffix added to the output filename |  |  |
| format | The file output format |  |  |
| codec | codec library to use |  | example: libx264 |

The [x264encode](#x264encode) will encode to AVC (H.264) video using [libx264](https://www.videolan.org/developers/x264.html).

**X264Encode**

| Field | Description | Constraint | Default 4+ |
| --- | --- | --- | --- |
| fields from  [VideoEncode](#videoencode) | x264-params | map of specific x264 codec library parameters |  |
| deblock, keyint, etc, see [Overwriting default preset settings - FFmpeg x264](https://trac.ffmpeg.org/wiki/Encode/H.264) and  [profiles](#profiles) for examples | format |  |  |
| mp4 | codec |  |  |

The [x265encode](#x265encode) will encode to HEVC (H.265) video using [libx265](https://www.videolan.org/developers/x265.html).

**X265Encode**

| Field | Description | Constraint | Default 4+ |
| --- | --- | --- | --- |
| fields from  [VideoEncode](#videoencode) | x265-params | map of specific x265 codec library parameters |  |
| deblock, scenecut, etc, see [Passing Options for FFmpeg x265](https://trac.ffmpeg.org/wiki/Encode/H.265)   and  [profiles](#profiles) for examples | format |  |  |
| mp4 | codec |  |  |

---

##### Image

[thumbnailmapencode](#thumbnailmapencode) generates a thumbnailmap from the input video.

**ThumbnailMapEncode**

| Field | Description | Constraint | Default |
| --- | --- | --- | --- |
| aspectWidth |  |  | 16 |
| aspectHeight |  |  | 9 |
| tileHeight |  |  | 90 |
| cols |  |  | 12 |
| rows |  |  | 20 |

[thumbnailencode](#thumbnailencode) generates an image/images from the video.
The extraction point/points are given as either a specific time, or, a list of percentages.

**ThumbnailEncode**

| Field | Description | Constraint | Default |
| --- | --- | --- | --- |
| percentages |  |  | [25, 50, 75] |
| thumbnailWidth |  |  | -2 |
| thumbnailHeight |  |  | 1080 |
| thumbnailTime |  |  | 1080 |
