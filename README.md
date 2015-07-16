# mod_cloudflare Railgun for Northern Lights #
This version of the mod_cloudflare extension for Easy Apache has been modified for use on the Northern Lights Network and includes specific modifications to the install script to reflect locations of Northern Lights Network Edge Railguns. This script should not be used in other enviornments.

If you are looking for the mod_cloudflare extension for EasyApache from which this was forked from: https://github.com/cloudflare/mod_cloudflare ane follow the instructions CloudFlare provides to install.

# mod_cloudflare for Apache #
Copyright CloudFlare Inc. 2013
Script modified by the Consortium of the Infinity Star. 2015

## mod_cloudflare.c ##

Based on mod_remoteip.c, this Apache extension will replace the remote_ip variable in user's logs with the correct remote IP sent from CloudFlare. The module only performs the IP substitution for requests originating from CloudFlare IPs by default.

To install, follow the instructions on:
    https://www.cloudflare.com/resources-downloads#mod_cloudflare
    
No further configuration is needed. However, if you wish to override the default values, the following directives are exposed:

### Adding Additional Railguns ###

Since the Northern Lights Network utilizes Cloud Linux primarily, adding new Railguns require direct modification of the mod_cloudflare.c file.

Railguns should be added after the CloudFlare IPv4 Trusted Proxies and prior to the CloudFlare IPv6 Trusted Proxies. All Railguns should be added to the following section:
    /* Northern Lights Railguns */
    
PLEASE NOTE THAT THIS IS THE MASTER BRANCH INTENDED FOR NLMS. Create new branches and modify installation scripts as needed for additional Northern Lights Network Railguns.

INFORMATION BEYOND THIS POINT IS AVAILABLE FROM THE ORIGINAL CLOUDFLARE EASY APACHE PAGE.

### CloudFlareRemoteIPHeader ###

PLEASE NOTE: THE INSTRUCTIONS BELOW AREN'T APPLICABLE TO CLOUDLINUX RAILGUN INSTALLATIONS. SEE THE ABOVE FOR ADDING NEW RAILGUNS TO THE NORTHERN LIGHTS NETWORK.

This specifies the header which contains the original IP. Default:

    CloudFlareRemoteIPHeader CF-Connecting-IP

### CloudFlareRemoteIPTrustedProxy ###

This is the IP range from which we will allow the `CloudFlareRemoteIPHeader` to be used from. See [here][1] for a complete list.

Note that on some systems, you may have to add a `LoadModule` directive manually. This should look like:

    LoadModule cloudflare_module /usr/lib/apache2/modules/mod_cloudflare.so

Replace `/usr/lib/apache2/modules/mod_cloudflare.so` with the path to `mod_cloudflare.so` on your system.


NOTES:

- If mod\_cloudflare and mod\_remoteip are enabled on the same web server, the server will crash if they both try to set the remote IP to a different value.
- Enabling mod\_cloudflare will not effect the performance of Apache in any noticeable manner. AB testing both over LAN and WAN show no equivalent numbers with and without mod\_cloudflare.
- If you like, you may also add the directive `DenyAllButCloudFlare`. This will result in all requests from IPs which are not in the `CloudFlareRemoteIPTrustedProxy` range being denied with a status of 403.

  [1]: https://www.cloudflare.com/ips
