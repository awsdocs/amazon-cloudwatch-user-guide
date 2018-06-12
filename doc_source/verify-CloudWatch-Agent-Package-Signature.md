# Verify the Signature of the CloudWatch Agent Package<a name="verify-CloudWatch-Agent-Package-Signature"></a>

GPG signature files are included for CloudWatch Agent assets compressed in ZIP archives\. The public key is here: [ https://s3.amazonaws.com/amazoncloudwatch-agent/assets/amazon-cloudwatch-agent.gpg]( https://s3.amazonaws.com/amazoncloudwatch-agent/assets/amazon-cloudwatch-agent.gpg)\. You can use the public key to verify that the agent's ZIP archive is original and unmodified\. First, import the public key with [https://gnupg.org/index.html](https://gnupg.org/index.html)\.

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

   If the fingerprint string does not match, do not install the agent, and contact Amazon Web Services\.

   After you have verified the fingerprint, you can use it to verify the signature of the CloudWatch agent package\.

1. Download the package signature file\.

   ```
   wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.sig
   ```

1. To verify the signature, run `gpg --verify`\.

   ```
   shell$ gpg --verify AmazonCloudWatchAgent.sig AmazonCloudWatchAgent.zip
   gpg: Signature made Wed 29 Nov 2017 03:00:59 PM PST using RSA key ID 3B789C72
   gpg: Good signature from "Amazon CloudWatch Agent"
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   ```

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact Amazon Web Services and avoid unzipping the downloaded agent archive\.

   Note the warning about trust\. A key is only trusted if you or someone that you trust has signed it\. This does not mean that the signature is invalid, only that you have not verified the public key\.

**To verify the CloudWatch agent package on a server running Windows Server**

1. Download and install GnuPG for Windows from [https://gnupg.org/download/](https://gnupg.org/download/)\. When installing, include the **Shell Extension \(GpgEx\)** option\.

   The remaining steps can be performed in Windows PowerShell\.

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

   Make a note of the key value, as you need it in the next step\. In the preceding example, the key value is `3B789C72`\.

1. Verify the fingerprint by running the following command, replacing *key\-value* with the value from the preceding step:

   ```
   PS>  gpg --fingerprint key-value
   pub   rsa2048 2017-11-14 [SC]
         9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   uid           [ unknown] Amazon CloudWatch Agent
   ```

   The fingerprint string should be equal to the following:

   `9376 16F3 450B 7D80 6CBD 9725 D581 6730 3B78 9C72`

   If the fingerprint string does not match, do not install the agent, and contact Amazon Web Services\.

   After you have verified the fingerprint, you can use it to verify the signature of the CloudWatch agent package\.

1. Download the package signature file\.

   ```
   wget https://s3.amazonaws.com/amazoncloudwatch-agent/windows/amd64/latest/AmazonCloudWatchAgent.sig
   ```

1. To verify the signature, run `gpg --verify`\.

   ```
   PS> gpg --verify AmazonCloudWatchAgent.sig AmazonCloudWatchAgent.zip
   gpg: Signature made 11/29/17 23:00:45 Coordinated Universal Time
   gpg:                using RSA key D58167303B789C72
   gpg: Good signature from "Amazon CloudWatch Agent" [unknown]
   gpg: WARNING: This key is not certified with a trusted signature!
   gpg:          There is no indication that the signature belongs to the owner.
   Primary key fingerprint: 9376 16F3 450B 7D80 6CBD  9725 D581 6730 3B78 9C72
   ```

   If the output includes the phrase `BAD signature`, check whether you performed the procedure correctly\. If you continue to get this response, contact Amazon Web Services and avoid unzipping the downloaded agent archive\.

   Note the warning about trust\. A key is only trusted if you or someone that you trust has signed it\. This does not mean that the signature is invalid, only that you have not verified the public key\.