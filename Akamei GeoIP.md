---


---

<p>Akamai maintains a database of information about IP addresses around the world which is called Edgescape data.</p>
<p>The Content Targeting module uses this Edgescape data. An EdgeServer can add the requesting user’s geographic location, network, connection speed, etc to a request header, and insert this into the forward request to the origin. The information contained in the header will allow us to target users in any way that we would like based on their specific location.</p>
<p>The header is called X-Akamai-Edgescape.  The list of parameter values can be found here (you will need an akamei account to actually see this): <a href="https://control.akamai.com/portal/content/akaServiceSupport/edgescape/shared/ES_datacodes.jsp">https://control.akamai.com/portal/content/akaServiceSupport/edgescape/shared/ES_datacodes.jsp</a></p>
<p>An example header looks like this:</p>
<p>X-Akamai-Edgescape: georegion=263,country_code=US,region_code=MA,city=CAMBRIDGE,dma=50 6,pmsa=1120,areacode=617,county=MIDDLESEX,fips=25017,lat=42.3933,l ong=-71.1333,timezone=EST,zip=02138-02142+02238-02239,continent=NA ,throughput=vhigh,asnum=21399</p>
<h1 id="how-to-enable-in-property-manager"><strong>How to Enable in Property Manager</strong></h1>
<p>In your Default Rule or a custom one, click Add Behavior.</p>
<h1 id="how-to-implement-on-live-site"><strong>How to Implement on Live Site</strong></h1>
<p>We will have to write custom code to handle queries based on the Edgescape header.   We can use a combination of php and javascript to accomplish this.</p>
<h1 id="how-to-test--debug"><strong>How to test / debug</strong></h1>
<p>In my research I came across this script which will be relatively easy to implement into our staging instance.  HTTP-Echo  <a href="https://github.com/shalomc/HTTP-echo">https://github.com/shalomc/HTTP-echo</a></p>
<h2 id="simple-usage">Simple Usage</h2>
<p>Standard usage is simply to invoke from curl or a browser. You will get a JSON response.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring"</code></p>
<h2 id="options">Options</h2>
<p>###Find and output Geo IP data The geoip information is fetched in real time from  <a href="http://www.geoplugin.net/">http://www.geoplugin.net/</a></p>
<hr>
<p>**** Use with caution. ****</p>
<hr>
<p>Set the config file to include this line to see the geo information of the client based on the IP address. It is very handy when debugging CDN connections to the origin.  <code>resolve-geoip=true</code></p>
<p>Alternatively, add the “x-echo-geoip” header to the request.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring" -H "x-echo-geoip: on"</code></p>
<p>Alternatively, add a “x-echo-geoip=on” query string parameter - it is case sensitive.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring&amp;x-echo-geoip=on"</code></p>
<p>###Output as text By default the results are returned as JSON.</p>
<p>You can add this line to the config file to have a text response instead of a json response.</p>
<p><code>response-type=text</code></p>
<p>Alternatively, add the “x-echo-type” header.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring" -H "x-echo-type: text"</code></p>
<p>Alternatively, add a “x-echo-type=text” query string parameter - it is case sensitive.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring&amp;x-echo-type=text"</code></p>
<p>###Support CDN True client IP headers When used behind a CDN, the IP address as seen by the script is not the client IP but the CDN IP.</p>
<p>You can add this line to the config file to use a specific HTTP header with the real client IP. In this case - it is configured for Fastly.</p>
<p><code>override-clientip=Fastly-Client-IP</code></p>
<p>Alternatively, add the “x-override-clientip” header.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring" -H "x-override-clientip: Fastly-Client-IP"</code></p>
<p>Alternatively, add a “x-override-clientip=Fastly-Client-IP” query string parameter.</p>
<p><code>curl "http://yourserver/somepath/somefile?somequerystring&amp;x-override-clientip=Fastly-Client-IP"</code></p>
<h2 id="example-output"><a href="https://github.com/shalomc/HTTP-echo#example-output"></a>Example output</h2>
<p>Debugging Akamai connections to my server</p>
<pre><code>{
    "meta": {
        "description": "HTTP echo service",
        "author": "Shalom Carmel 2016",
        "version": "1.9.1"
    },
    "request": {
        "timestamp": 1453377461,
        "date": "2016-01-21 11:57:41",
        "server": "echo.globaldots.com",
        "port": "80",
        "protocol": "HTTP\/1.1",
        "client_ip": "165.254.92.135",
        "method": "GET",
        "uri": "\/index.html?x=887",
        "query_string": "x=887",
        "https": null,
        "remote_user": null
    },
    "headers": {
        "Accept": "text\/html,application\/xhtml+xml,application\/xml;q=0.9,*\/*;q=0.8",
        "Accept-Encoding": "gzip",
        "Accept-Language": "en-US,en;q=0.5",
        "Akamai-Origin-Hop": "1",
        "Cache-Control": "no-cache, max-age=0",
        "Connection": "TE, keep-alive",
        "Host": "echo.globaldots.com",
        "Pragma": "akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no, no-cache",
        "Te": "chunked;q=1.0",
        "User-Agent": "Mozilla\/5.0 (Windows NT 10.0; WOW64; rv:43.0) Gecko\/20100101 Firefox\/43.0",
        "Via": "1.1 akamai.net(ghost) (AkamaiGHost)",
        "X-Akamai-Config-Log-Detail": "true",
        "X-Akamai-Device-Characteristics": "brand_name=Firefox;device_os=Windows NT;is_mobile=false",
        "X-Akamai-Edgescape": "georegion=175,country_code=PL,city=WARSAW,lat=52.25,long=21.00,timezone=GMT+1,continent=EU,throughput=vhigh,bw=5000,asnum=47273,location_id=0",
        "X-Akamai-Staging": "EdgeSuite",
        "X-Forwarded-For": "185.15.80.161"
    },
    "body": null
}

</code></pre>
<h2 id="installation"><a href="https://github.com/shalomc/HTTP-echo#installation"></a>Installation</h2>
<h3 id="simple-installation"><a href="https://github.com/shalomc/HTTP-echo#simple-installation"></a>Simple installation</h3>
<p>Just drop everything into a folder on your web server. As simple as that.<br>
Use composer to verify that you have the latest Mobile detect library.</p>
<h3 id="installation-from-scratch"><a href="https://github.com/shalomc/HTTP-echo#installation-from-scratch"></a>Installation from scratch</h3>
<p>Assuming you have a new AWS server, you need php 5.4 and Apache to run this tool.</p>
<p>The <a href="http://install.sh">install.sh</a> script in the setup folder installs all of the prerequisites, downloads the repository, and puts everything in place.</p>
<p>Modify it to remove parts that were done manually.</p>
<h3 id="cloudformation-installation"><a href="https://github.com/shalomc/HTTP-echo#cloudformation-installation"></a>CloudFormation Installation</h3>
<p>The HttpEchoService.json file is an AWS CloudFormation template to start an EC2 server and automatically configure it to use the echo service.</p>

