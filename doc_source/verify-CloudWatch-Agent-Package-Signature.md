# Verifying the Signature of the CloudWatch Agent Package<a name="verify-CloudWatch-Agent-Package-Signature"></a>

GPG signature files are included for CloudWatch agent packages on Linux servers\. You can use a public key to verify that the agent download file is original and unmodified\.

For Windows Server, you can use the MSI to verify the signature\.

For macOS computers, the signature is included in the agent download package\.

To find the correct signature file, see the following table\. For each architecture and operating system there is a general link as well as links for each Region\. For example, for Amazon Linux and Amazon Linux 2 and the AMD64 architecture, three of the valid links are:
+ `https://s3.amazonaws.com/amazoncloudwatch-agent/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm.sig`
+ `https://s3.us-east-1.amazonaws.com/amazoncloudwatch-agent-us-east-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm.sig`
+ `https://s3.eu-central-1.amazonaws.com/amazoncloudwatch-agent-eu-central-1/amazon_linux/amd64/latest/amazon-cloudwatch-agent.rpm.sig`


| Architecture | Platform | Download Link | Signature File Link | 
| --- | --- | --- | --- | 
|  AMD64 |  Amazon Linux and Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Centos  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/centos/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  Debian  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/debian/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  AMD64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/amd64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  AMD64 |  Oracle  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/oracle\_linux/amd64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  AMD64 |  macOS  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/darwin/amd64/latest/amazon\-cloudwatch\-agent\.pkg\.sig  | 
|  AMD64 |  Windows  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi  |   https://s3\.amazonaws\.com/amazoncloudwatch\-agent/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/windows/amd64/latest/amazon\-cloudwatch\-agent\.msi\.sig  | 
|  ARM64 |  Amazon Linux 2  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/amazon\_linux/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Redhat  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/redhat/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 
|  ARM64 |  Ubuntu  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/ubuntu/arm64/latest/amazon\-cloudwatch\-agent\.deb\.sig  | 
|  ARM64 |  SUSE  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm  |  https://s3\.amazonaws\.com/amazoncloudwatch\-agent/suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig https://s3\.*region*\.amazonaws\.com/amazoncloudwatch\-agent\-*region*suse/arm64/latest/amazon\-cloudwatch\-agent\.rpm\.sig  | 

**To verify the CloudWatch agent package on a Linux server**

1. Download the public key\.

   ```
   shell$ wget https://s3.amazonaws.com/amazoncloudwatch-agent/assets/amazon-cloudwatch-agent.gpg
   ```

1. Import the public key into your keyring\.

   ```
   shell$  gpg --import amazon-cloudwatch-agent.gpg
   gpg: key 3B789C72: public key "Amazon CloudWatch Agent" imported
   gpg: Total number processed: 1
   gpg: imported: 1 (RSA: 1)
   ```

   Make a note of the key value, as you need it in the next step\. In the preceding example, the key value is `3B789C72`\.

1. Verify the fingerprint by running the following command, replacing *key\-value* with the value from the preceding step:

   ```
   shell$  gpg --fingerprint key-value
   pub   2048R/3B789C72 2017-11-14
         Key fingerprint = 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   uid                  Amazon CloudWatch Agent
   ```

   The fingerprint string should be equal to the following:

   `9376 16F3 450B 7D80 6CBD 9725 D581 6730 3B78 9C72`

   If the fingerprint string doesn't match, don't install the agent\. Contact Amazon Web Services\.

   After you have verified the fingerprint, you can use it to verify the signature of the CloudWatch agent package\.

1. Download the package signature file using wget\. To determine the correct signature file, see the preceding table\.

   ```
   wget Signature File Link
   ```

1. To verify the signature, run gpg \-\-verify\.

   ```
   shell$ gpg --verify signature-filename agent-download-filename
   gpg: Signature made Wed 29 Nov 2017 03:00:59 PM PST using RSA key ID 3B789C72
   gpg: Good signature from "Amazon CloudWatch Agent"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   ```

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact Amazon Web Services and avoid using the downloaded file\.

   Note the warning about trust\. A key is trusted only if you or someone who you trust has signed it\. This doesn't mean that the signature is invalid, only that you have not verified the public key\.

**To verify the CloudWatch agent package on a server running Windows Server**

1. Download and install GnuPG for Windows from [https://gnupg.org/download/](https://gnupg.org/download/)\. When installing, include the **Shell Extension \(GpgEx\)** option\.

   You can perform the remaining steps in Windows PowerShell\.

1. Download the public key\.

   ```
   PS> wget https://s3.amazonaws.com/amazoncloudwatch-agent/assets/amazon-cloudwatch-agent.gpg -OutFile amazon-cloudwatch-agent.gpg
   ```

1. Import the public key into your keyring\.

   ```
   PS>  gpg --import amazon-cloudwatch-agent.gpg
   gpg: key 3B789C72: public key "Amazon CloudWatch Agent" imported
   gpg: Total number processed: 1
   gpg: imported: 1 (RSA: 1)
   ```

   Make a note of the key value because you need it in the next step\. In the preceding example, the key value is `3B789C72`\.

1. Verify the fingerprint by running the following command, replacing *key\-value* with the value from the preceding step:

   ```
   PS>  gpg --fingerprint key-value
   pub   rsa2048 2017-11-14 [SC]
         9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   uid           [ unknown] Amazon CloudWatch Agent
   ```

   The fingerprint string should be equal to the following:

   `9376 16F3 450B 7D80 6CBD 9725 D581 6730 3B78 9C72`

   If the fingerprint string doesn't match, don't install the agent\. Contact Amazon Web Services\.

   After you have verified the fingerprint, you can use it to verify the signature of the CloudWatch agent package\.

1. Download the package signature file using wget\. To determine the correct signature file, see [CloudWatch Agent Download Links](download-cloudwatch-agent-commandline.md#agent-download-link-table)\.

1. To verify the signature, run gpg \-\-verify\.

   ```
   PS> gpg --verify sig-filename agent-download-filename
   gpg: Signature made 11/29/17 23:00:45 Coordinated Universal Time
   gpg:                using RSA key D58167303B789C72
   gpg: Good signature from "Amazon CloudWatch Agent" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   ```

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact Amazon Web Services and avoid using the downloaded file\.

   Note the warning about trust\. A key is trusted only if you or someone who you trust has signed it\. This doesn't mean that the signature is invalid, only that you have not verified the public key\.

**To verify the CloudWatch agent package on a macOS computer**
+ There are two methods for signature verification on macOS\.
  + Verify the fingerprint by running the following command\.

    ```
    pkgutil --check-signature amazon-cloudwatch-agent.pkg
    ```

    You should see a result similar to the following\.

    ```
    Package "amazon-cloudwatch-agent.pkg":
            Status: signed by a developer certificate issued by Apple for distribution
            Signed with a trusted timestamp on: 2020-10-02 18:13:24 +0000
            Certificate Chain:
            1. Developer ID Installer: AMZN Mobile LLC (94KV3E626L)
            Expires: 2024-10-18 22:31:30 +0000
            SHA256 Fingerprint:
            81 B4 6F AF 1C CA E1 E8 3C 6F FB 9E 52 5E 84 02 6E 7F 17 21 8E FB
            0C 40 79 13 66 8D 9F 1F 10 1C
            ------------------------------------------------------------------------
            2. Developer ID Certification Authority
            Expires: 2027-02-01 22:12:15 +0000
            SHA256 Fingerprint:
            7A FC 9D 01 A6 2F 03 A2 DE 96 37 93 6D 4A FE 68 09 0D 2D E1 8D 03
            F2 9C 88 CF B0 B1 BA 63 58 7F
            ------------------------------------------------------------------------
            3. Apple Root CA
            Expires: 2035-02-09 21:40:36 +0000
            SHA256 Fingerprint:
            B0 B1 73 0E CB C7 FF 45 05 14 2C 49 F1 29 5E 6E DA 6B CA ED 7E 2C
            68 C5 BE 91 B5 A1 10 01 F0 24
    ```
  + Or, download and use the \.sig file To use this method, follow these steps\.

    1. Install the GPG application to your macOS host by entering the following command\.

      ```
      brew install GnuPG
      ```
  + Download the package signature file using curl\. To determine the correct signature file, see [CloudWatch Agent Download Links](download-cloudwatch-agent-commandline.md#agent-download-link-table)\.
  + To verify the signature, run gpg \-\-verify\.

    ```
    PS> gpg --verify sig-filename agent-download-filename
    gpg: Signature made 11/29/17 23:00:45 Coordinated Universal Time
    gpg:                using RSA key D58167303B789C72
    gpg: Good signature from "Amazon CloudWatch Agent" [unknown]
    gpg: WARNING: This key is not certified with a trusted signature!
    gpg:          There is no indication that the signature belongs to the owner.
    Primary key fingerprint: 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
    ```

    If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact Amazon Web Services and avoid using the downloaded file\.

    Note the warning about trust\. A key is trusted only if you or someone who you trust has signed it\. This doesn't mean that the signature is invalid, only that you have not verified the public key\.