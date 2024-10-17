# Part 1 - Preparing environment

## Briefing about the testing domain. Administrative notes

About the domain `testzone01.asp.theflightsims.eu.org`:

> TheFlightSims is an international company primarily researching and engineering in the aerospace industry. Globally, the company has offices in North America (Boston and Florida, both in the US) and Asia Pacific (Tokyo - Japan, Can Tho - Vietnam, and Singapore).
>
> In the Asia Pacific zone (`asp.theflightsims.eu.org`), the regional administrators decided to open a new DNS sub-zone, named "testzone01", as the zone where all the software testing proceeded. One of many tests that those engineers perform is to increase the level of security of the current setting by testing which one is effective.
>
> They have chosen Microsoft Advanced Threat Analytics (or ATA in short) as the threat analytical tool for potential attack and virus protection for all domain controllers, functional servers, and essential computers. However, they noticed that ATA only runs on port 443. It quickly becomes serious because the chosen server runs Windows Server 2022 with IIS installed as the web server and proxy server, which is already configured with port 443.

The domain `testzone01.asp.theflightsims.eu.org` is an Active Directory Domain-configured, where the DNS Server is also running within the domain and installed on the Domain Controller. DNSSEC is not enabled on this sub-zone.

Take note of the servers and their services below. Unrelated servers and services don't count in the graphic.

<p align="center">
 <img src="./img/briefing_domain_connection.png" alt="Noticed servers and related services"
</p>

## Administrator plans

Since the `zone01-NET01` is already the web server (using 80 and 443 ports), it is good to know that the proxy server is also the zone01-NET01. That means Microsoft ATA should be configured in port 8080 (or any other available port) and reserve the 443 for IIS.

After checking the ATA is running successfully in port 8080, the IIS will be used as the proxy server to control the traffic in and out for the ATA.

> IIS `zone01-NET01` will be configured like this:
>
> - "Default Web Site" will respond for hostname `zone01-NET01.testzone01.asp.theflightsims.eu.org`
> - "Microsoft ATA" will respond for hostname `defender.testzone01.asp.theflightsims.eu.org`
> - Configure this host name in **Blinding > {Select your type, either http or https} > Host name**. Remember to check the box "Require Server Name Indication" when available.
>
> DNS Server in `zone01-DC01` will be configured like this:
>
> - Update the CNAME of `defender.testzone01.asp.theflightsims.eu.org` address to the IP address of `zone01-NET01.testzone01.asp.theflightsims.eu.org`

## Preparing environment for the ATA setup

1. First of all, it is the certificate. The new DNS will be `defender.testzone01.asp.theflightsims.eu.org`. Make sure the DNS' certificate is `defender.testzone01.asp.theflightsims.eu.org`

<p align="center">
 <img src="./img/certificate_setup.png" alt="Setup Certificate"/>
</p>

Once the setup is running, select the certificate that has been configured. (In that case, the certificate corresponds to `defender.testzone01.asp.theflightsims.eu.org`)

<p align="center">
 <img src="./img/ata_setup_with_configured_certficate.png" alt="Select the certificate while installing ATA" />
</p>

2. Prevent all running IIS hosts using blinding port 443 but not deleting it. For example, "Default Web Site" by default uses port 443 - which is recommended to temporarily stop because the ATA is not yet configured.

<p align="center">
 <img src="./img/iis_pre_setup.png" alt="IIS before setup ATA" />
</p>

## Next step

Once you follow and understand everything, and you are done with everything, it is a good sign to know that you are ready for the next step.
