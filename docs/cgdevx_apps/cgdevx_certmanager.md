# Secure Your Service Endpoints Seamlessly with Cert-Manager in CGDevX

In CGDevX, we leverage the power of Cert-Manager to automate the creation and rotation of TLS certificates for your service endpoints. This essential feature enhances the security of your applications without added hassle. Whether you prefer bringing your own Certificate Authority (CA) or having CGDecX create one for you, we recommend utilizing [Let's Encrypt](https://letsencrypt.org/) for production certificates.

## Embrace Let's Encrypt for Production Certificates

To optimize security and streamline certificate management, we strongly advise opting for [Let's Encrypt](https://letsencrypt.org/) when setting up Cert-Manager. Utilizing Let's Encrypt requires DNS availability for the requesting domains, necessitating the installation of ExternalDNS in CGDevX. Please note that due to the interconnectedness of various CGDevX contexts with DNS settings, all DNS configurations can be found in one centralized location.

## Configuration Made Easy

CGDevX empowers you to configure Cert-Manager effortlessly, tailoring it to suit your specific needs. The following essential values can be easily customized:

 1. customRootCA - Create or Automatically Generate CA
   Specify your custom Root CA to be used for creating and verifying self-signed certificates. Alternatively, you can leave it empty, and CGDevX will generate one for you automatically.

 2. customRootCAKey - Issuing Certificates Simplified
   Define the private key for your custom Root CA, which is used for issuing certificates. You have the option to let CGDevX generate it for you automatically by leaving this field empty.

 3. email - Mandatory for Let's Encrypt Issuer
   When opting for the Let's Encrypt Issuer, provide the email address to ensure smooth certificate issuance and management.

 4. issuer - Choose Between Let's Encrypt or Custom CA
   Select your desired certificate issuer from the available options: "letsencrypt" or "custom-ca."

 5. stage - Mandatory for Let's Encrypt Issuer
   When utilizing the Let's Encrypt Issuer, specify whether you want to operate in "staging" or "production" mode.

 6. resources - Configure Request and Limits
   Fine-tune the Request and Limits for cert-manager to meet the resource requirements of your system.

## Secure Your Service Endpoints with Confidence

Cert-Manager in CGDevX streamlines the process of managing TLS certificates, bringing you enhanced security without compromising on simplicity. Leverage the power of Cert-Manager, embrace Let's Encrypt for production certificates, and ensure your service endpoints are fortified against threats.

Join CGDevX today and embark on a journey towards robust security and effortless certificate management!