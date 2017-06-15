## Scenario 5. Building a Single Tenant with a Single-Node Graph within the APIC using Service Manager mode via the Northbound API

The purpose of this scenario is to separate the L2-L3 and L4-L7 policy configuration by using a preconfigured Python script to create **one tenant** with **one single-node graph** within the APIC, and then use Citrix NetScaler MAS to configure the application policy.

 > **NOTE**: If you have already run a scenario that created objects in the APIC, you must [remove those objects](../Scenario4) before running this scenario.

 ![Screenshot1](images/Screenshot1.png)

 > **NOTE**: The Python script method calls individual XML scripts to create a series of objects. To pause the script and create an object using an APIC wizard, see the instructions inline. Objects that can be created via wizard using this script are indicated with (*) in the list below.


The Python script performs the following functions:

* Create a Tenant
* Import Citrix Device Package
* Create L4-L7 Device Manager
* Create L4-L7 Device Cluster
	* Create Concrete Device
	* Create Logical Interfaces
	* Connect Interfaces of Logical to Concrete Device
* Create the Security Contract
* Create the Service Graph
* Attach Service Graph to Contract




* Verify deployed L4-L7 parameters in NetScaler MAS and in NetScaler VPX

 ![Screenshot2](images/Screenshot2.png)

### Steps


 	
 	a. Log in with the following credentials: Username: **admin**, Password: **C1sco12345**.
 	



 ![Screenshot3](images/Screenshot3.png)

5. Login to PuTTy (**user01**/**user01**) and place it so that both the PuTTY window and the APIC directories are visible.

  ![Screenshot4](images/Screenshot4.png)
  
6. From the command line type .**/request.py Citrix_Scripts/Build_Citrix_VPX_MAS.cfg** and hit <**Enter**>.

 >**NOTE**: To show the XML code as the Python script calls each XML script, substitute ./xml_request.py Citrix_Scripts/Build_Citrix_VPX_MAS.cfg for the above command.
 >   ![Screenshot5](images/Screenshot5.png)
 >This is an example of the partial XML output for the Build_Citrix_VPX_MAS.xml script.

 The Build_Citrix_VPX_MAS.cfg script utilizes a series of XML scripts to perform the necessary configuration steps. It will pause between each of the XML scripts, and the user can either press <Enter> to run the script, or type s to skip the script and configure the object via a wizard. While the script is running, a brief description will display what that script is doing, while the APIC window updates in real-time. When a script completes successfully, the success code 200 will appear onscreen.

  ![Screenshot6](images/Screenshot6.png)
  
 > **NOTE**: The following step creates the tenant in APIC. To perform this procedure manually, type **s** and hit <**Enter**> at the prompts for the **1N_Tenant.xml** script.

7. Create the **Tenant**.
	a. In the APIC top menu, select **TENANTS**.
	
  ![Screenshot7](images/Screenshot7.png)

   b. Return to the PuTTY window and hit <**Enter**> at the Hit return to process **Citrix_Scripts/1N_Tenant.xml** or **press‘s**’ and return to **skip this script prompt**.
   
   
  ![Screenshot8](images/Screenshot8.png)
  
  d. Double-click the newly created Visa tenant.
  
  

8. Import the Citrix Device Package as follows:

  
  
  
  
  
   ![Screenshot9](images/Screenshot9.png)

 >**NOTE**: The next four steps create the device cluster, concrete devices and logical interfaces in the APIC. To [create these devices manually](../Appendix/Appendix-D), type s and hit enter at the prompts for the following scripts:
 >
 >* CreatedevMgr.xml
 >* 1N_CreateLDevVip_MAS.xml
 >* 1N_CreateCDev.xml
 >* 1N_CreateLIf_MAS.xml

9. Create the **Device Manager**:
 b. Expand **L4-L7 Services > Device Managers** and show there are no device clusters present.
 c. Return to the PuTTY window and hit <**Enter**> at the Hit return to process Citrix_Scripts/CreatedevMgr.xml.xml or press ‘s’ and return to skip this script prompt.
 d. Verify the creation of the **NetScaler-MAS** Device Manager.
 
10. Create the **Device Cluster**:
 a. Expand the **L4-L7 Services > L4-L7 Devices** folder and show there are no device clusters present.
 b. Return to the PuTTY window and hit <**Enter**> at the Hit return to process Citrix_Scripts/1N_CreateLDevVip_MAS.xml or press‘s’ and return to skip this script prompt.
 c. Verify the creation of the **ADCCluster1** device cluster.
 a. Expand **L4-L7 Services > L4-L7 Devices > ADCCluster1**.
 c. **ADCCluster1** is now populated with the inside, outside, and mgmt Logical Interfaces.
 
 a. Return to the PuTTY window and hit <**Enter**> at the Hit return to process Citrix_Scripts/1N_CreateCDev.xml or press‘s’ and return to skip this script prompt.
 b. The **ADC1** device is created in the **ADCCluster1** device cluster. Expand the **ADC1** device to display the concrete interfaces.
 
   ![Screenshot10](images/Screenshot10.png)

 >**NOTE**:Wait until the device cluster is in a **stable** state before proceeding. It may take up to 30 seconds and you may need to click the icon.
 
13. Create the **Application Profile** as follows:
 a. Expand **Security Policies > Contracts**.
 b. Return to the PuTTY window and hit <**Enter**> at the Hit return to Process Citrix_Scripts/1N_CreateContract.xml or press‘s’ and return to skip this script prompt.
 c. **webCtrct > http** is created in **Contracts**.
 a. Still in the **Tenant Visa** directory, expand **L4-L7 Services > L4-L7 Service Graph Templates**, which is empty.
 b. Return to the PuTTY window and hit <**Enter**> at the Hit return to process Citrix_Scripts/1N_CreateWebGraph_MAS.xml or press ‘s’ and return to skip this script prompt.

 c. **WebGraph** is created in the **L4-L7 Service Graph Templates** folder, with **Function Node – N1** sub-directories. This script also pushes the Port Profiles and Connections.


   ![Screenshot11](images/Screenshot11.png)
   
16. Optional – Click **Function Node – N1** to show the items that have been configured.




   ![Screenshot12](images/Screenshot12.png)

 > **NOTE**: The next step runs the script that attaches the Service Graph to the Contract. To [perform the procedure manually](../Appendix/Appendix-E), type s at the prompts for the Citrix_Scripts/1N_AttachGraphToContract.xml script:

19. Attach the **Service Graphs** to the **Visa** tenant, as follows:

 
 

20. Click the **L4-L7 Services > Deployed Graph Instances** folder – the contract is listed in applied state.

   ![Screenshot13](images/Screenshot13.png)


21. Return to the VMWare Client to see that the new port-profiles have been created.


   ![Screenshot14](images/Screenshot14.png)

22. Refer to the **Recent Tasks** window to see the tasks that attach the new port-profiles to the Virtual machine – vpx1.

   ![Screenshot15](images/Screenshot15.png)

23. Return to the APIC and expand **Tenant Visa > L4-L7 Services > Deployed Graph Instances**. Click **webCtrct-WebGraph-Visactx1** to see the topology of the deployed Service Graph.

   ![Screenshot16](images/Screenshot16.png)

24. Expand **Deployed Devices** and click **ADCCluster1-Visactx1** to review VLANs.

   ![Screenshot17](images/Screenshot17.png)
   
25. Open a new **Chrome** tab. Click the NetScaler VPX shortcut and login with credentials: **nsroot/C1sco12345**.


   ![Screenshot18](images/Screenshot18.png)


27. Obtain the MAC addresses for the network adapters in vSphere:

   ![Screenshot19](images/Screenshot19.png)

 b. Click **vpx1** to display the parameters.

 
   ![Screenshot20](images/Screenshot20.png)

 d. Click Network adapter 2 and Network adapter 3 to show the MAC Addresses.

   ![Screenshot21](images/Screenshot21.png)

28. Return to APIC, and verify that the MAC addresses and the Port-Profiles match those in vSphere:
 a. From the **VM Networking > Inventory** menu, expand **VMware > My-vCenter > Controllers > dCloudDC > Hypervisors > vesx1.dcloud.cisco.com > Virtual Machines** and **vesx2.dcloud.cisco.com > Virtual Machines**. (vpx1 may be on either host.)
 b. Click **vpx1** to display its parameters. See that the newly attached port-profiles show in the **Portgroup** field.

   ![Screenshot22](images/Screenshot22.png)

 a. Open a new **Chrome** tab. In the Chrome Bookmarks Toolbar, click the **NetScaler MAS** shortcut and log in (**nsroot/C1sco12345**).
 b. Navigate to **Orchestration**, on the left menu click **SDN Orchestration**, and then click **Cisco ACI**.
 c. In the work pane, click **APIC-HTTP-LB**.

   ![Screenshot23](images/Screenshot23.png)
   
 d. On the **Deploy Configuration** page, click **Sample Stylebook for APIC Load Balanced Application**.

   ![Screenshot24](images/Screenshot24.png)

 

   ![Screenshot25](images/Screenshot25.png)

 g. In the **Load Balanced App Virtual IP** address field, enter **10.10.10.100**.
 
 
 

   ![Screenshot26](images/Screenshot26.png)
   
 k. On the **Deploy Configuration** page, click **Create**.

   ![Screenshot27](images/Screenshot27.png)

 l. Watch the configuration dialog, then verify the Success messages, shown below.
 
   ![Screenshot28](images/Screenshot28.png)

 m. Click **APIC-HTTP-LB** to review the deployed settings or make changes.
 


   ![Screenshot29](images/Screenshot29.png)

 p. In the **Traffic Management > Load Balancing** menu, click **Service Groups** on the left menu, then click **WebGraph-VIP-80-sg**.

   ![Screenshot30](images/Screenshot30.png)

 q. Click **2 Service Group Members** to display the pool of servers belonging to the Virtual Server created by **NetScaler MAS**.

   ![Screenshot31](images/Screenshot31.png)

 r. Review the list of servers that make up the service group.

30. [Remove the configuration and reset the environment](../Scenario4) prior to starting another scenario.