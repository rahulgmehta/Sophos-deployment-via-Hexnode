You can deploy Endpoint on your Macs using Hexnode. This means you can install Sophos protection software remotely.
Sophos provide a Configuration Profile. The profile sets appropriate authorizations for settings. These include the following:
Full Disk Access
system extensions
notifications
These settings are required for Endpoint to work correctly.
These instructions are for Hexnode, however, the MDM profile and script should work in other MDM solutions.

Download the macOS configuration profiles
You must download the macOS configuration profiles before you download your installer.
To do this, do as follows:
Sign in to Sophos Fusion.
Go to My Environment > Installers.
In Endpoint, under Deployment Tools, click the Download the macOS Deployment Tools (includes MDM profiles) download link.
Extract the contents of the SophosMacDeploymentTools.zip file. The extracted Deployment Tools folder contains the Sophos Endpoint and Sophos Endpoint and ZTNA folders. Each folder contains configuration files for each macOS version. Next, download the installer. See the next section.

Download installer
You need the macOS Endpoint installer from Sophos Fusion. You also need the SophosInstall URL. You need this to use with the installation script.
To do this, do as follows:
Sign in to Sophos Fusion.
Go to My Environment > Installers.
In Endpoint, choose your installer.
Click Download Complete macOS Installer to download an installer with all endpoint products your license covers.
Click Choose Components… to choose which products will be included in the installer. For more help on downloading the installer see Endpoint.
Save the download URL. To do this, do as follows:
Right click the SophosInstall.zip folder and click Get Info.
Under More Info, copy the URL shown in Where from. If the URL isn't shown in Where from, do as follows:
Right-click the SophosInstall.zip folder in your browser Downloads.
Click Copy address. This gives you the URL of the downloaded installer.
Save the copied URL. You need this to use with the installation script in Hexnode.

Add and assign profiles
Now, you need to add and assign your configuration profile. This is the Sophos Endpoint.mobileconfig file you saved from the installer zip file, SophosInstall.zip.

Add profile
To add your profile, do as follows:

In Hexnode, click Policies, New Policy, Help me create a New Policy> select macOS>NextEnterprise>Next.
Give Policy Name: Sophos Configuration 
Scroll down to Configuration> Deploy Custom Configuration> Choose file> Pop will open 
Choose the file you want to upload (shophos Endpoint mobile config you have downloaded) and click upload button.
Once upload is completed click ok.

Assign Profile
Click on policy Target > Select the Demo Group or Test Group to test in a Test devices.
And click save.

Create and configure a script policy
Next, you need to create and assign the Sophos installation script to your target groups. You will use the Install Sophos Script.txt file you downloaded earlier. You will also need the installer download URL you copied earlier.

Create Sophos installation script
To create the script, do as follows:
In Hexnode go to content > Library> Add Content> upload file> Select Generate Script with Hexnode Genie. In script Editor Place paste the Install Sophos Script.txt content

#!/bin/bash
SOPHOS_DIR=$(mktemp -d -t Sophos_Install)
trap 'rm -rf ${SOPHOS_DIR}' EXIT
cd $SOPHOS_DIR
# Installing Sophos
curl -L -O "put installer URL in these quotes.” (Replace with the URL with Sophos download url copied from above steps)
unzip SophosInstall.zip
chmod a+x $SOPHOS_DIR/Sophos\ Installer.app/Contents/MacOS/Sophos\ Installer
chmod a+x $SOPHOS_DIR/Sophos\ Installer.app/Contents/MacOS/tools/com.sophos.bootstrap.helper
$SOPHOS_DIR/Sophos\ Installer.app/Contents/MacOS/Sophos\ Installer --quiet
rm -rf $SOPHOS_DIR
exit 0

And click save> give file Name, and verify file format as .sh
For more information on the command-line options, see Installer command-line options for Mac.

Installing the script to device
Click on Automate> Active Automation> New Automation > select macOS >Quick> Name the Automation as Sophos install script and click Next.
Select> Execute Custom Script> Hexnode Repository> select the Sophos install script and click execute and the click Next.
In Assignments select the Demo device Group or test user Group click Next. Review the setting and click save.

Check that Endpoint is installed
You can can check in Hexnode policies status, Automation Reports
Check that your managed Macs have Sophos Endpoint installed on them. On each Mac, check the following:
In System Preferences check Profiles. You should see the name of the configuration profile you set up in Hexnode.
In Sophos Endpoint, check the Endpoint Self Help tool. Any issues with installation or configuration are shown here.
For help on fixing permission issues, see Security permissions on macOS
