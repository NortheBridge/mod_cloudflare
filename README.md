### Providence-Network ###
***You are in the Providence Branch, Phase II in development and deployment of mod_cloudflare to the Northern Lights SoftLayer Footprint. This branch is the branch pushed out to Northern Lights Edge Servers following final code review***

# mod_cloudflare for Northern Lights Railguns #
mod_cloudflare for Northern Lights Railguns is a modified version of the original mod_cloudflare EasyApache extension for use exclusively with the Northern Lights Network Edge Relay Servers and is likely to be incompaitble with other network setups due to Northern Lights' Network's unqiue design. **Please do not use this in your own enviornment. It is recommended that you fork your own mod_cloudflare directly from the CloudFlare team.** *We appreciate contributions to help better the mod_cloudflare extension for the Northern Lights Railguns as with any open sourced project.*

## Get mod_cloudflare ##
If you are looking for the mod_cloudflare extension for EasyApache from which this was forked from please visit: https://github.com/cloudflare/mod_cloudflare ane follow the instructions CloudFlare provides to install.

### mod_cloudflare for Apache ###
Copyright CloudFlare Inc. 2013. *This script has been modified by the Consortium of the Infinity Star for use with the Northern Lights network.*

**Script modifications designed explictly for Network Edge Servers utilizing CloudLinux/RHEL/CentOS.** Functionality on non-RHEL based systems is not guarnteed. Since the Northern Lights Network does not consist of any other Linux distributions we will not pursue further development for non-RHEL. Non-RHEL solutions may however still exist.

## mod_cloudflare.c ##

Based on mod_remoteip.c, this Apache extension will replace the remote_ip variable in user's logs with the correct remote IP sent from CloudFlare. The module only performs the IP substitution for requests originating from CloudFlare IPs by default.

Installation instructions are available at:
    https://www.cloudflare.com/resources-downloads#mod_cloudflare
    
## Defining Development Workflow ##

In order to create an efficent and proven workflow, the following branches exists:

    [1]Alpha-DevBranch
    [2]Beta-TestBranch
    [3]Delta-Release
    [4]Providence-Network
    [5]Washington-Network
    [6]master
    
**master:** is the directly forked source code of mod_cloudflare from CloudFlare. This branch will be where any future mod_cloudflare updates are pulled to first;
**Washington-Network:** is the branch containing the specific configurations for the Northern Lights Washington SoftLayer Network;
**Providence-Network:** is the branch containing the specific configurations for the Northern Lights Network Seattle SoftLayer network;
**Alpha-DevBranch:** is the branch containing the combined configuration of any Network Branches;
**Beta-TestBranch:** is the branch used for testing changes to configurations before final deployment across the Northern Lights SoftLayer footprint;
**Delta-Release:** is the branch for which all approved changes are merged into before being queried and downloaded by the Northern Lights Network during a "Correlative Update."

### Adding Additional Railguns ###

Since the Northern Lights Network utilizes CloudLinux primarily, adding new Railguns **require direct modification of the mod_cloudflare.c file.**

Railguns should be added after the CloudFlare IPv4 Trusted Proxies and prior to the CloudFlare IPv6 Trusted Proxies. All Railguns should be added to the following section:
    /* Northern Lights Railguns */
    
*PLEASE NOTE:* This is the Alpha Development Branch. When adding new Railguns from one of the "Network Branches" as defined below, they should be merged into the Alpha Development Branch to ensure continuity. The following are "Network Branches:"

    [1]Washington-Network (Washington, D.C.)
    [2]Providence-Network (Seattle and Vancouver - Providence Base)
    
Network Branches are labled with their Consortium name and detailed with their SoftLayer location.

*INFORMATION BEYOND THIS POINT IS AVAILABLE FROM THE ORIGINAL CLOUDFLARE EASY APACHE PAGE.*

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
