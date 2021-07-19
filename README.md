# HostF2A Image Host

HostF2A is a image host for all. With multiple settings

> HostF2A Is a free to use image host that can be deployed in the web with no effort. This uses local storage as for storing the files. This is not only a image host you can upload any kind of file that is under 50mb each you can also change the upload limit later.

##### Setup and Running 
```scala
Download the source and extract it 
Go to ./server
Then open cmd in that directory 
Then 'npm install'
After installing is done type 'node server.js'
Then open your browser and go to 'http://127.0.0.1'
Boom! Your host is now running in your local computer.
```
##### Config
```shell
Server configuration is done through the server.conf
Client configuration is done through the  config.js
```
##### Understanding the configs

`listen is an address:port-formatted string, where either one are optional. Some examples include ":9000" to listen on any interface, port 9000; "127.0.0.1" to listen on localhost port 80; "1.1.1.1:8080" to listen on 1.1.1.1 port 8080; or even "" to listen on any interface, port 80.`

`api_key is a very basic security measure, requiring any client making an upload to know this key. This doesn't seem very useful and should be revamped; replace it with HTTP auth maybe?`

`delete_key is a key used to secure the deletion keys. Set this to something that only the server knows.`

`maximum_file_size is the largest file, in bytes, that's allowed to be uploaded to the server. The default here is a decimal 50MB.`

`There are three additional sections in the configuration file: http, https and cloudflare-cache-invalidate. The first two are fairly self-explanitory (and at least one must be enabled).`

`cloudflare-cache-invalidate is disabled by default and only useful if you choose to run the Up1 server behind Cloudflare. When this section is enabled, it ensures that when an upload is deleted, Cloudflare doesn't hold on to copies of the upload on its edge servers by sending an API call to invalidate it.`

`For the web application configuration, a config.js file is provided. Make sure the api_key here matches the one in server.conf.`

##### How it works?
```shell
Before an image is uploaded, a "seed" is generated. This seed can be of any length (because really, the server will never be able to tell), but has a length of 25 characters by default. The seed is then run through SHA512, giving the AES key in bytes 0-256, the CCM IV in bytes 256-384, and the server's file identifier in bytes 384-512. Using this output, the image data is then encrypted using said AES key and IV using SJCL's AES-CCM methods, and sent to the server with an identifier. Within the encryption, there is also a prepended JSON object that contains metadata (currently just the filename and mime-type). The (decrypted) blob format starts with 2 bytes denoting the JSON character length, the JSON data itself, and then the file data at the end.

Image deletion functionality is also available. When an image is uploaded, a delete token is returned. Sending this delete token back to the server will delete the image. On the server side, HMAC-SHA256(static_delete_key, identifier) is used, where the key is a secret on the server.
```

[![logo](https://avatars2.githubusercontent.com/u/12774718?s=150 "logo")](https://avatars2.githubusercontent.com/u/12774718?s=150 "logo")

###### Please help this project by subscribing to my youtube channel -Please help this project by subscribing to my youtube channel - [click here to subscribe](https://www.youtube.com/channel/UCDnNlPz2Wa2EW3gi-zOIsPA "click here to subscribe")
