# Build Instructions

```bash
./genMakefiles linux
make -j4
```

Replace "linux" with your platform, e.g. avr32-linux, cygwin, freebsd, iphoneos, linux, linux-64bit, macosx, openbsd, solaris-64bit, etc (see config.PLATFORM files)

You will find various executables:

 * ./testProgs - contain various programs such as testRTSPClient to receive an RTSP stream
 * ./proxyServer/live555ProxyServer - a great RTSP proxy server
 * ./mediaServer/live555MediaServer - an RTSP media server for serving static files over RTSP

# Changes to Master

### Buffer sizes
OutPacketBuffer::maxSize is increased to 2,000,000 bytes which makes live555 work better with buggy IP cameras.

### Force port re-use
Added -DALLOW_RTSP_SERVER_PORT_REUSE=1 to force reusing existing port (e.g. when restarting the proxy). Please ensure
you never run multiple instances of the proxy on the same port!

### Quit on TCP Errors
liveMedia/RTCP.cpp#431 changed break to exit(1); - this ensures that live555 does not flood the screen and/or log with:
The remote endpoint is using a buggy implementation of RTP/RTCP-over-TCP.  Please upgrade it!

### Add -d option
See Proxyserver_check_interPacketGap_2017.01.26.patch - This allows specifying a number of seconds of inactivity
before timing out the connection.

### Schedule Delayed Task Event
sheduledelayedtask event created: Every 1 sec checks for the update in the .cfg file. Incase of change like Add, Update, Delete of RTSP urls, RTSP proxy server changes.
#### Add
A new servermediasession (sms) is created in case of a new RTSP url is added to the file.
#### Delete
Existing servermediasession (sms) is deleted using deleteservermediasession() method.
#### Update
Update existing rtsp url corresponding to existing rtsp url name. Deleted the previous servermediasession object and created a new object with the updated rtsp url.

