---
title: Adding a SSL certificate for secure HTTPS communication
filename: source/_docs/adding-a-ssl-certificate-for-secure-https-communication.md
---
## SSL on Pantheon

Pantheon provides a self-service interface to load your SSL certificate into the system. This will provide you with a **dedicated IP address** for your site, which you will need to use in all your DNS configuration to use HTTPS.

However, you are still responsible for obtaining your cert. There are many different SSL certificate providers you can use. We don't have a specific recommendation, but the steps are generally common.

## Open SSL Certificate

Pantheon will use a certificate generated by any valid OpenSSL implementation. Most linux and MacOS environments can generate keys and CSRs directly from the command-line. You can generate a CSR via this  [digicert wizard tool](https://www.digicert.com/easy-csr/openssl.htm) also.

If you are in a Windows environment then you will need to have Cygwin installed. In the event that you do not have it running you can take a look at our documentation to get [Cygwin running on Windows](/documentation/advanced-topics/installing-cygwin-on-windows/-generating-a-csr-certificate-on-windows).

## Generating a Key and CSR

First you will generate a RSA Key (.key) and CSR (certificate signing request). This will require you to set up the information for your certificate. The most important part of this is the "Common Name", which is the domain you are seeking to secure. You can read the hints on the Wizard as you enter your information for more details.  
  
  
 ![Generate CSR command](https://pantheon-systems.desk.com/customer/portal/attachments/40331)

**Note:** Do not place a password on your key. This will leave us unable to work with it.  


  
  


    openssl req -new -newkey rsa:2048 -nodes -out www_pantheon_com.csr
    -keyout www_pantheon_com.key -subj "/C=US/ST=California/L=San Francisco/O=Pantheon 
    Systems/OU=Support/CN=www.pantheon.com";

The output of this should be two files, in our example:  
  
  
`www_pantheon_com.csr`  
`www_pantheon_com.key`

**Note:** Trying to use the above command or .key and .csr files in this example will NOT work for you. You must generate a valid .key and .csr file for your own domain and paste the command from the Wizard into your terminal or console.

## Multiple Domains with Subject Alternative Names (SAN's)

A Subject Alternative Name is a public key certificate that is permitted to identify more than one entity or device. SAN's may be the right choice for a user looking to manage multiple domains with a single certificate.

For more information on SAN's see [here](http://www.digicert.com/subject-alternative-name.htm).

## Getting the SSL Certificate

You will then provide the CSR to the SSL provider, and they will supply you with one or more certificate (.crt or .pem) files. You will then need to load these certificates and the original .key into Pantheon. The CSR is not used after you get your certificates.  
  
  


## Handling Intermediary Certs

Often you will get more than one certificate back from your SSL provider. There will be one certificate that is especially for you, and another "intermediary" cert. These are used to establish the chain of trust between your provider and you.

If you get more than one intermediary cert file, you will need to stick them together into one. This is rare though. Your provider may also send you a "certificate authority" or "CA" cert. This does not need to be included.

## Loading Certs into Pantheon

If you would like to validate your own files before submitting them to Pantheon, there is a handy collection of OpenSSL validation commands [here](http://www.sslshopper.com/article-most-common-openssl-commands.html).

You will need to have a [Pro level plan](https://www.getpantheon.com/pricing) or higher in order to take advantage of SSL on Pantheon. Go to the "Domains" tool on the environment you want to add the SSL cert to. Once there you can click on the "SSL" tab in main workspace.

If you have not added a card to your site then you will get a message in the main workspace letting you know you need to select a plan.

![](https://pantheon-systems.desk.com/customer/portal/attachments/259878)

Finally, from here you can paste in your .key that you generated earlier as well as your SSL certificate that you have purchased from your provider, these two are required.

If your provider also issued an Intermediary Certificate you should copy and paste this into the SSL Cert window too.

![Site dashboard add SSL certificate step 2](https://pantheon-systems.desk.com/customer/portal/attachments/259882)​

After submitting your certificates, your new static I.P. address should show up under the "Domain Setup" section. Due to the nature of obtaining fresh IPs, this may be delayed by up to 120 seconds. If you are experiencing problems getting your I.P. address, contact support.  


## Adding a Wildcard Certificate

You can upload the same wildcard SSL certificate for multiple sites that share a domain, and add the correct subdomain to each site's domain tab in the live environment.

## Verifying your SSL IP Address

Before you point your DNS to the custom IP address that you get after entering your SSL you can do so by visiting the HTTPS IP address. This will allow you to inspect the certificate is correct.

![Site dashboard domain setup section](https://pantheon-systems.desk.com/customer/portal/attachments/259889)​

**Note: Do not attempt to use https://\*.mysite.gotpantheon.com domain as a means to test your SSL, this will not work.**

#### IPv6 support for SSL

There are two options for configuring your DNS when you are using SSL. The platform has support for IPv4 (A records) and IPv6 (AAAA record) when a cert is initially installed on the dashboard.

At the moment the IPv4 address should be selected if you are unsure but if you are comfortable and understand IPv6 then this option is available.

## Check SSL with Chrome

Before you point your DNS to the custom I.P. address that you get after entering your SSL you can do so by visiting the HTTPS version of the I.P. This will allow you to inspect the certificate is correct.

**Important!** This a good way to verify your SSL certificate matches what you have uploaded. Do not expect to view the contents of the site as we use HTTP headers to route your domain correctly.


![](https://pantheon-systems.desk.com/customer/portal/attachments/66611)  
  
  

  
  
 ![](https://pantheon-systems.desk.com/customer/portal/attachments/66609)

## Checking 'end-to-end' routing

Once you verify the certificate is set up, visiting your SSL IP will show a 404 Site Not Found page when you visit the SSL IP directly.  Pantheon's routing uses the hostname to route traffic to particular backends; without a hostname, the router cannot determine which site was requested.    
  
  

  
  


    $: curl -I -v https://192.168.0.1 --header "Host: mywebsite.com"

## SSL Providers

All you need to do now is choose a certificate authority that will meet your needs. Once are ready to purchase an SSL certificate you can use the guide to [upload a SSL certificate to your site](/documentation/howto/adding-a-ssl-certificate-for-secure-https-communication/).

- [Comodo](http://www.comodo.com/ "Comodo Group")
- [DigiCert](http://www.digicert.com/ "DigiCert")
- [Entrust](http://www.entrust.com/ "Entrust")
- [GeoTrust](http://www.geotrust.com/ "GeoTrust")
- [Go Daddy](http://www.godaddy.com/ "Go Daddy") 
- [Thawte](http://www.thawte.com/ "Thawte")
- [VeriSign](http://www.verisigninc.com/ "VeriSign")
- [TrustWave](https://ssl.trustwave.com/support/install-certificate-cpanel.php "TrustWave")

## Verifying the SSL cert authority

If you receive SSL chain errors on a mobile device such as mobile Chrome, please make sure that the cert that you have purchased has a certificate authority that will support your use case.

There are a number of ways to check the certificate has broad support by using a tool such as an [SSL checker](http://www.sslshopper.com/ssl-checker.html).  

  
  


![](https://pantheon-systems.desk.com/customer/portal/attachments/150678)

If you notice that the SSL chain is broken or you are experiencing issues with mobile versions of the site, we recommend getting an SSL certificate from a different provider.

## HTTPS vs HTTP Redirection

We recommend choosing a single protocol and redirecting traffic to either HTTPS or HTTP rather than in a mixed mode. You can read more about HTTP and HTTPS redirection [o](/documentation/howto/redirect-incoming-requests/-redirecting-incoming-requests) [n this documentation page](/documentation/howto/redirect-incoming-requests/-redirecting-incoming-requests).  
  
  



## Frequently Asked Questions

### What should I do with a GoDaddy gd\_bundle.crt?

Use it as the Intermediary Certificate.

### What combination do I use for a basic Network Solutions SSL pack?

    MYSITE_COM.crt-------------------Cert
    UTNAddTrustServer_CA----------Intermediate
    mysite_com.key---------------------Private Key

### What combination do I use for a Comodo SSL pack?

Comodo lists the different combinations [here](https://support.comodo.com/index.php?_m=knowledgebase&_a=viewarticle&kbarticleid=1182).

### I am receiving the following error: "400: Error the cert at line 1 of the Chain file does not sign the main cert,Signing key mismatch". What do I do?

This error indicates that some part of the chain is out of order. Check that you have the main & intermediary in the right places, and if you have multiple intermediaries check they’re in the right order.

