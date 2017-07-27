# go-livepeer
[Livepeer](https://livepeer.org) is a live video streaming network protocol that is fully decentralized, highly scalable, crypto token incentivized, and results in a solution which is cheaper to an app developer or broadcaster than using traditional centralized live video solutions.  go-livepeer is a golang implementation of the protocol.

Building and running this node allows you to:

* Create a local Livepeer Network, or join the existing Livepeer test network.
* Broadcast a live stream into the network.
* Request that your stream be transcoded into multiple formats.
* Consume a live stream from the network.

For full documentation and a project overview, go to 
[Livepeer Documentation](https://github.com/livepeer/wiki/wiki)

## Installation
The easiest way to install Livepeer is by downloading it from the [release page on Github](https://github.com/livepeer/go-livepeer/releases).  Pick the appropriate platform and the latest version.

## Build
If you have never set up your Go programming environment, do so according to Go's [Getting Started Guide](https://golang.org/doc/install).

You can build Livepeer from scratch.  If you already have the code, you can simply run `go build ./cmd/livepeer/livepeer.go` from the project root directory.

You can also fetch and build the `livepeer` node using `go get github.com/livepeer/go-livepeer/cmd/livepeer`. It should be built and put in your $GOPATH/bin.

## Setup
The current version of Livepeer requires [ffmpeg](https://www.ffmpeg.org/).

On OSX, run
`brew install ffmpeg --with-ffplay`

or on Debian based Linux
`apt-get install ffmpeg`

## Usage

### Short version
- Make sure you have the `livepeer` executable.  It can be downloaded from the [releases page](https://github.com/livepeer/go-livepeer/releases).

- `./livepeer` to start Livepeer.  This will be our broadcasting node.

- `./livepeer -p=15001 -rtmp=1936 -http=8936 -datadir=./data1` to start another Livepeer node. This will be our subscribing node.

- `./livepeer broadcast` to start broadcasting. You should see a streamID in the log. Copy it.

- `./livepeer stream -http=8936 -id={streamID}`, replacing the {streamID} with the streamID we just got.

You should see a video stream broadcasted from your webcam.  It may feel a little delayed - that's normal. Video live streaming typically has latency from 15 seconds to a few minutes. We are working on solutions to lower this latency, using techniques like WebRTC, peer-to-peer streaming, and crypto-incentives.

### Broadcasting

Sometimes the `./livepeer broadcast` command doesn't work - especially if you are running the software on Windows or Linux. Livepeer can take any RTMP stream as input, so you can use other popular streaming software to create the video stream. e recommend [OBS](https://obsproject.com/download) or [ffmpeg](https://www.ffmpeg.org/).

By default, the RTMP port is 1935.  For example, if you are using OSX with ffmpeg, run 

`ffmpeg -f avfoundation -framerate 30 -pixel_format uyvy422 -i "0:0" -vcodec libx264 -tune zerolatency -b 1000k -x264-params keyint=60:min-keyint=60 -acodec aac -ac 1 -b:a 96k -f flv rtmp://localhost:1935/movie`

Similarly, you can use OBS, and change the setting->stream->URL to `rtmp://localhost:1935/movie`

If the broadcast is successful, you should be able to get a streamID by querying the local node:

`curl http://localhost:8935/streamID`

### Streaming

Sometimes the `./livepeer stream` doesn't work.  You can use tools like `ffplay` to view the stream.

For example, after you get the streamID, you can view the stream by running: 

`ffplay http://localhost:8935/stream/{streamID}.m3u8`

## Contribution
Thank you for your interest in contributing to the core software of Livepeer. 

There are many ways to contribute to the Livepeer community. To see the project overview, head to our [Wiki overview page](https://github.com/livepeer/wiki/wiki/Project-Overview). The best way to get started is to reach out to us directly via our [gitter channel](https://gitter.im/livepeer/Lobby).

