# BB3-API

Currently this is where I'll be engaging in some research to get a workable BB3 API up that lets others gain access to publicly-available Blood Bowl 3 data without having to read a bunch of network packets themselves. 

## Phases of Development
- Beginning Research <-- Currently Here
- Research Complete
- Architecting
- Active Development
- Testing & Validation
- Ready for Production

## Research Steps
1. Download [Wireshark](www.wireshark.org).
2. Start capturing packets.
3. Open BB3, log in, and do things in the menus.
4. Close BB3 and stop capturing.
5. Look through the Wireshark data for a TCP stream between your computer's IP and a server IP that makes sense. For me it has been a server based in the UK (msg me on Discord if you'd like to compare the IP to what I've found). Then right-click > Follow > TCP Stream.
6. The connection to BB3 should start with something like:
```<Header><Data type="textxml" size="24" zipped="false" MessageName="NotificationKeepAlive" MessageToken="1" sizeBeforeCompression="24"/></Header><NotificationKeepAlive/>....<Header><Data type="protocol" size="96" zipped="no" MessageName="KeepAliveAdvice" MessageToken="0" sizeBeforeCompression="96" /></Header><KeepAliveAdvice><Params KeepAliveInterval="15000" KeepAliveTimeout="100000"/></KeepAliveAdvice>```
7. Look through the data and find the xml requests for things you did! If you clicked on your team page, you can search for "RequestGetTeamsOfGamer", which is the request your client makes to the server to check the teams you have created. The response to that should have fields like `<JerseyPattern>`, `<PrimaryColor>`, `<Name>`, etc. If you decode the weird-looking string between those brackets using Base64, that should output something more human-readable. Example: The data I have is `<Name>SXQncyBUaW1lIGZvciBDaGVmIExpZmU=</Name>`, and decoded from base64, that string is `It's Time for Chef Life`, the name of one of my teams.
8. Keep digging! I'll create a list of useful `MessageNames` I find, and upload them here periodically. 

## Next Steps
* Once we know the kind of data that we can obtain through these requests, the next step is to start figuring out how to communicate with Cyanide's server. This means that we need to send specific headers and data with our requests, and format it correctly. 
* After we've figured out how to consistently make requests programatically, we can create an API that will allow others to make requests without all this work. 
* After an API is created, bots can use this API to get the data they need to function and inform the player base.

## Contributing
I welcome any and all Pull Requests! You can fork this repo and work on your own, then open a PR later. Or, if you contact me, I can add you as a contributor to this repo, and you can make PRs here. More contribution details/instructions to be written later.

## Notes
There will probably be a lot of Python notebook usage during the first iterations of research, because it's the language I currently work fastest in. I would like to eventually make this in Rust though, as I need practice.
