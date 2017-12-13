# WAF-XSS-Functional-Example
Sample exercise to create a XSS Threat Protection profile, block a message with the enabled protection profile, and review the outcome of the protection operation.

## Step By Step

1. Access your API Gateway via the CLI.

2. Change directory to the threat protection directory. Unzip the Spider Labs rule set (file name may change with version).
\# cd /opt/Axway/APIM-7.5.3/apigateway/system/conf/threat-protection
\# unzip SpiderLabs-owasp-modsecurity-crs-2.2.9-7-g3e6782b.zip

3. Access the threat protection default configuration directory and copy the Spider Labs XSS ruleset from the base rules to default actived rules.
\# /opt/Axway/APIM-7.5.3/apigateway/system/conf/threat-protection/default
\# cp ../SpiderLabs-owasp-modsecurity-crs-3e6782b/base_rules/modsecurity_crs_41_xss_attacks.conf ./activated_rules/

4. Modify opt/Axway/APIM-7.5.3/apigateway/system/conf/threat-protection/default/modsecurity.conf file as follows:

a. Disable the DetectionOnly rule and enable enforcement:

\#SecRuleEngine DetectionOnly
SecRuleEngine On

b. Add the following anywhere in the file to change the default action from block to deny.

\# My Rules
SecDefaultAction "phase:1,deny,log"
SecDefaultAction "phase:2,deny,log"

5. Log into Policy Studio and access your API project. Create a Threat Protection Profile and apply to a port for testing, as documented here: https://docs.axway.com/bundle/APIGateway_753_AdministratorGuide_allOS_en_HTML5/page/Content/AdminGuideTopics/admin_waf.htm

6. Create a basic Set Message Filter and Reflect Message filter policy to set a default 'OK' type message for test purposes. Give it a relative path behind the protected port. For this example, I have deployed a sample 200 OK XML response policy at '/echo'.

![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")

7. Call your policy via a web browser and ensure it responds as expected. Example: https://192.168.159.134:8445/echo

![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")

8. Call your policy via a web browser with a sample script as a URL query parameter. This should fail if properly configured. Example: https://192.168.159.134:8445/echo<script>alert(1)</script>

![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")

9. Log into your API Gateway Management web console (eg https://hostmane:8090), and access the Traffic Monitor. Apply a filter to show messages blocked by the Threat Protection settings. Drill down on the transaction to review the capture.

![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")

![alt text](https://github.com/Axway-API-Management-Plus/MFA_Auth_Directory_Lookup/blob/master/example/src/directoryScreenshot.png "Directory Attributes")

## API Management Version Compatibility
This artefact was successfully tested for the following versions:
- V7.5.3

## Contributing

Please read [Contributing.md](https://github.com/Axway-API-Management/Common/blob/master/Contributing.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Team

![alt text][Axwaylogo] Axway Team

[Axwaylogo]: https://github.com/Axway-API-Management/Common/blob/master/img/AxwayLogoSmall.png  "Axway logo"

Contact - Daniel Wille: dwille@axway.com

## License
[Apache License 2.0](/LICENSE)
