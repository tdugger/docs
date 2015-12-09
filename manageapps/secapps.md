{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:screen: .screen}

#Securing apps
{: #securingapps}

*Last updated: 4 December 2015*

You can secure your applications by uploading SSL certificates and restricting access to the applications.
{:shortdesc}

##Creating certificate signing requests
{: #ssl_csr}

Before you can upload the SSL certificates to which you are entitled with {{site.data.keyword.Bluemix}}, you must create a certificate signing request (CSR) on your server.

A CSR is a message that is sent to a certificate authority to request the signing of a public key and associated information. Most commonly, CSRs are in PKCS #10 format. The CSR includes a public key, as well as a common name, organization, city, state, country, and email. SSL certificate requests are accepted only with a CSR key length of 2048 bits.

For the CSR to be valid, the following information must be entered when generating the CSR:

**Country name**
  
  A two-digit code that represents the country or region. For example, "US" represents the United States. For other countries or regions, check the [list of ISO country codes](https://www.iso.org/obp/ui/#search){:new_window} before you create the CSR.
  
**State or Province**

  The full, unabbreviated name of the state or province.

**Locality**

  The full name of the town or city.
  
**Organization**

  The full name of the business or company, as legally registered in your locality, or personal name. For companies, be sure to include the registration suffix, such as Ltd., Inc., or NV.
  
**Organization unit**

  The branch name of your company that is ordering the certificate, such as Accounting or Marketing.
  
**Common aame**

  The fully qualified domain name (FQDN) for which you are requesting the SSL certificate.
  
The methods for creating a CSR vary depending on your operating system. The following example shows how to create a CSR by using [the OpenSSH command tool](http://www.openssl.org/){:new_window}:

```
openssl req -out CSR.csr -new -newkey rsa:2048 -nodes -keyout
    privatekey.key
```

**Note:** OpenSSL SHA-512 implementation depends on compiler support for 64-bit integer type. You can use the SHA-1 option for applications that have compatibility issues with the SHA-256 certificate.

##Uploading SSL certificates
{: #ssl_certificate}

You can apply a security protocol to provide communication privacy for your application to prevent eavesdropping, tampering, and message forgery.

For every organization in {{site.data.keyword.Bluemix_notm}} with an account owner who has a Pay as you Go or Subscription plan in place, you are allowed four free certificate uploads. For every organization with an account owner who has a free trial account, you are allowed one free certificate upload.

Before you can upload certificates, you must create a certificate signing request. See [Creating certificate signing requests](#ssl_csr).

To properly serve the SSL certificate, you must use the following IP addresses to configure the DNS or a hosts file when creating a custom domain to provide the URL route that is allocated to your organization in {{site.data.keyword.Bluemix_notm}}.

* US-SOUTH: 75.126.81.68
* EU-GB: 5.10.124.142
* AU-SYD: 168.1.35.169

The IP addresses that you use for dedicated environments are different. Contact your IBM representative to get the IP address for a dedicated environment.

For more information about creating a custom domain, see [Creating and using a custom domain](updapps.html#domain).

To upload a certificate for your application:

1. Create a route or edit an existing route by selecting **Edit Routes and App Access** from the application menu.

2. In the Edit Routes and App Access dialog, click **Manage Domains**.

3. For your custom domain, click **Upload Certificate**.

4. Browse to upload a certificate, private key, and optionally an intermediate certificate. You can also select the checkbox to enable request of a client certificate.

  **Certificate**
    
    A digital document that binds a public key to the identity of the certificate owner, thereby enabling the certificate owner to be authenticated. A certificate is issued by a certificate authority and is digitally signed by that authority.
    
    The following types of certificates are supported in {{site.data.keyword.Bluemix_notm}}:
    
      * PEM (pem, .crt, .cer, and .cert)
	  * DER (.der or .cer )
      * PKCS #7 (p7b, p7r, spc)
	  
  **Private key**
  
    An algorithmic pattern used to encrypt messages that only the corresponding public key can decrypt. The private key is also used to decrypt messages that were encrypted by the corresponding public key. The private key is kept on the user system and is protected by a password.
    
    The following types of private keys are supported in {{site.data.keyword.Bluemix_notm}}:
    
    * PEM (pem, .key)
    * PKCS #8 (p8, pk8)
    
    **Limitation:** Private keys that are protected by a passphrase cannot be uploaded.
    
  **Intermediate certificate**
  
    A subordinate certificate that is issued by the trusted root certificate authority (CA) specifically to issue end-entity server certificates. The result is a certificate chain that begins at the trusted root CA, passes through the intermediate certificate, and ends with the SSL certificate issued to the organization.
    
    You should use an intermediate certificate to verify the authenticity of the main certificate. Intermediate certificates are typically obtained from a trusted third-party. You might not require an intermediate certificate when testing your application prior to deploying it to production.
  
  **Enable request of client certificate**
  
    If you enable this option, a user who tries to access an SSL protected domain is requested to provide a client side certificate. For example, in a web browser, when a user tries to access an SSL protected domain, the web browser prompts the user to provide a client certificate for the domain.
  
  **Note:** The custom certificate feature in {{site.data.keyword.Bluemix_notm}} domain management depends on the Server Name Indication (SNI) extension of the Transport Layer Security (TLS) protocol. Therefore, the client code that accesses {{site.data.keyword.Bluemix_notm}} applications protected by custom certificates must support the SNI extension in the TLS implementation. For more information, see [section 7.4.2 of RFC 4346](http://tools.ietf.org/html/rfc4346#section-7.4.2){:new_window}.

To delete a certificate or replace an existing certificate with a new one, go to **Manage Organizations** > **Domains** > **View Certificate** to manage your certificates.