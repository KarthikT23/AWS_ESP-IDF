# AWS_ESP-IDF
AWS IoT Core using ESP - IDF

# Software requirements
1. ESP - IDF: Install and setup ESP-IDF from https://docs.espressif.com/projects/esp-idf/en/stable/esp32/get-started/windows-setup.html
   
2. Amazon Web Services Account: Create an AWS account from https://aws.amazon.com/iot-core/


# Hardware requirements
1. ESP32 Dev Board
2. Micro USB Cable

# Steps to configure AWS IoT Core
1. Create AWS IoT Thing:

a. Log in to the AWS IoT Core Console.

b. In the navigation pane, click Manage > All devices > Things.

c. Click Create Things and select Create a single thing.

d. Enter the name for your IoT Thing.

e. Skip the Thing Type and Group for now and click Next.

2. Create Certificates:

a. Select Auto-generate a new certificate.

b. After the certificate is created, download the certificate, public key, private key, and the Amazon root CA. You will need these later.

3. Attach a policy to the certificate:

a. Click on Create policy.

b. This will take you to a new tab.

c. Under Policy properties, enter the policy name.

d. Under Policy document:
        1. Policy effect: Select Allow
        2. Policy action: Select *
        3. Policy resource: Enter *

e. Click on Create.

f. Go back to the previous tab where you have the list of policies, and select the policy you have created. And then press Create thing.


4. Download the Device Certificate, Public key file, Private Key file, and Root CA Certificate (Amazon trust services endpoint, RSA 2048 bit key: Amazon Root CA 1). Note that this is the only time you can download these files! And then click on Done.


5. In case you did not create a policy as suggested in Step 3, no worries. In the navigation pane to the left, click Security > Policies and then follow Step 3.


# Steps to configure ESP - IDF and run an example
1. Download the latest release of esp-aws-iot from https://github.com/espressif/esp-aws-iot, extract into C:\Espressif\frameworks\esp-idf-v5.2.2\components

2. Copy the tls_mutual_auth folder from C:\Espressif\frameworks\esp-idf-v5.2.2\components\esp-aws-iot-master\examples\mqtt into your local project directory on your system.

3. Inside the tls_mutual_auth folder (in your local project directory), navigate to the main folder and create a new folder called certs.

4. Store all the downloaded certificates and keys into the certs folder. 

5. Rename the Root CA Certificate as root_cert_auth.crt, Device certificate as client.crt, and Private key as client.key. (Public key is not needed but AWS forces you to download it)

6. Inside the tls_mutual_auth folder, open CMakeLists.txt and edit the EXTRA_COMPONENT_DIRS with the path to where the libraries are stored. For example, coreMQTT library is stored in "C:/Espressif/frameworks/esp-idf-v5.2.2/components/esp-aws-iot-master/libraries/coreMQTT"

7. It is possible that the libraries are not extracted when downloading and extracting the latest release of esp-aws-iot. Make sure to download and extract the necessary libraries from Github into "C:/Espressif/frameworks/esp-idf-v5.2.2/components/esp-aws-iot-master/libraries”

For example, coreMQTT library is present in https://github.com/FreeRTOS/coreMQTT/tree/2beef04725328923e05e576b884212d53ec97af7 and must be downloaded, extracted into C:\Espressif\frameworks\esp-idf-v5.2.2\components\esp-aws-iot-master\libraries\coreMQTT.

8. Open ESP-IDF CMD , navigate to your project directory using the command:
   
cd <project_directory>

9. Configure the project by running the command:
    
idf.py menuconfig

Navigate to Example Connection Configuration and enter the Wi-Fi SSID and Password.
Go to Example Configuration, enter the Thing Name (which is the MQTT client identifier used in this example) and the Endpoint of the MQTT broker to connect to (this can be found in AWS IoT Core > Connect > Connect one device > Step 1: Prepare your device, point 4. It should look something like: xxxxxxxxxxxxxxxxx.iot.region.amazonaws.com)

10. Build your project by the command:
    
idf.py build

11. Flash and Monitor the Built project by the command:
    
idf.py -p <port_number> flash monitor

12. Open AWS IoT Core, navigate to AWS IoT > Manage > Things > Thing Name > Activity > MQTT Test Client

13. Enter the topic name which should be Thing Name/example/topic and click Subscribe to view the MQTT communication.




