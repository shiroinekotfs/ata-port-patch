# Microsoft ATA Port Patch

Guide to patch Microsoft ATA to another port, not 443

## About Microsoft ATA

According to Microsoft Documentation:

> Microsoft Advanced Threat Analytics (ATA) is an on-premises platform that helps protect your enterprise from multiple advanced targeted cyber attacks and insider threats.

Microsoft ATA works as the Microsoft Defender gateway, delivering security notifications and threat-resolving guidance for system administrators.

However, due to the release of Microsoft Defender XDR, ATA has reached its final release.

> ATA Mainstream Support ended on January 12, 2021. Extended Support will continue until January 2026. For more information, [read this blog from Microsoft](https://techcommunity.microsoft.com/t5/security-compliance-and-identity/end-of-mainstream-support-for-advanced-threat-analytics-january/ba-p/1539181).
>
> Do not click on the link highlighted by the word "try" - it is a logout button. 😂

## Perquisites

1. The primary target is running the ATA gateway with IIS Installed on Windows Server without removing or stopping the "Default Web Site" web service.
2. Security. You can move to another port that is hard to detect or is too far from any normal range of ports, preventing it from being a target of a potential attack.
