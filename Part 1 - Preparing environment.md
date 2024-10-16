# Part 1 - Preparing environment

## Briefing about my domain and notes

About the domain `testzone01.asp.theflightsims.eu.org`:

> TheFlightSims is an international company, primary researching and engineering in aerospace industry. Globally, the company has office in North America (Boston and Florida, both in the US) and Asia Pacific (Tokyo - Japan, Can Tho - Vietnam, and Singapore).
>
> In Asia Pacific zone (`asp.theflightsims.eu.org`), the regional administrators decided to open a new sub DNS zone, named "testzone01", as the zone where all the software testing proceed. One of many tests that those engineer perform is increase the level of security of the current setting, by testing which one is effective.
>
> They have chosen Microsoft Advanced Threat Analytics (or ATA in short), as the threat analytical tool for potential attack and virus protection. However, they noticed that ATA only run on port 443. It quickly becomes serious because the chosen server running Windows Server 2022 with IIS as the server and proxy server, already configured port 443 within it.

The domain `testzone01.asp.theflightsims.eu.org` is a Active Directory Domain-configured, where the DNS Server is also running within the Domain, and installed on the Domain Controller.

This domain is not enabled DNSSEC by default.

Notice servers and it services below, unrelated servers and services doesn't count in the graphic.

<p align="center">
 <img src="./img/briefing_domain_connection.png" alt="Noticed servers and related services"
</p>

## Administrator plans

Since the `zone01-NET01` is already the web server (using 80 and 443 ports), it is good to know that the proxy server is also the zone01-NET01. That means, Microsoft ATA should be configured in port 8080 (or any other ports that is available), reserve the 443 for IIS.

After checking the ATA is running successfully in port 8080, the IIS will be used as the proxy server, to control the traffic in-out for the ATA.

> IIS `zone01-NET01` will be configured like this:
>
> - "Default Web Site" will respond for host name `zone01-NET01.testzone01.asp.theflightsims.eu.org`
> - "Microsoft ATA" will respond for host name `defender.testzone01.asp.theflightsims.eu.org`
> - Configure this host name in **Blinding > {Select your type, either http or https} > Host name**. Remember check the box "Require Server Name Indication" when available.
>
> DNS Server in `zone01-DC01` will be configured like this:
> 
> - Update the CNAME of `defender.testzone01.asp.theflightsims.eu.org` address to the IP address of `zone01-NET01.testzone01.asp.theflightsims.eu.org`

## Preparing environment for the ATA setup

First of all, it is the certificate.
