
# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.


# Azure Advanced Threat Protection Sizing Tool - Version 1.3.0.0

The sizing tool automates collection of the amount of traffic Azure ATP would need to monitor and automatically provides supportability and resource recommendations for both the ATA (Center and Gateway) and Azure ATP (Sensor).
It is recommended that you run the Azure ATP sizing tool as follows:

- With domain admin credentials
- From a domain-joined workstation that has network access to all the domain controllers on the following ports: TCP 135, TCP 389 ,TCP 445 and TCP RPC Dynamic Ports. If these ports are blocked, you may experience an error message "Remote Registry Query Failed" in the OS Server Level field
- Make sure you have .Net 4.5.2 or later installed 
- Make sure that the EPPlus dll, that is included in the zip, is in the same folder that the sizing tool is being used from

The sizing tool uses the RPC protocol, specifically Remote WMI and Remote PerfMon over RPC.  When the tool launches, it enumerates all domain controllers in the domain (by default) using LDAP.  Then it contacts each DC and performs an initial remote performance counter query and a couple of WMI queries.  After these queries are done, it uses the remote performance counters to repeatedly ask each DC for certain perf data (CPU, Mem, packets) roughly every 5 seconds (by default) for the duration of its execution. The tool generates an Excel file summarizing its findings and recommendations.

By default, the tool runs for 24 hours (this is the recommended amount of time to run it) and gathers data including the packets/sec counter, operating system version, compute utilization, and memory utilization.

In the Excel file you will find two sheets, one for ATA sizing, and the second for Azure ATP.

If you choose to [deploy Standalone Sensors](https://docs.microsoft.com/en-us/azure-advanced-threat-protection/atp-capacity-planning#azure-atp-sensor-and-standalone-sensor-sizing-a-namesizinga), in the Excel results file, under Azure ATP summary tab, use the following fields to determine the AATP Sensor specifications needed: 

Match the "Busy Packets/sec" field in the Sensor table of the results file to the **"PACKETS PER SECOND"** field in the [AATP Sensor table or the AATP Standalone Sensor](https://docs.microsoft.com/en-us/azure-advanced-threat-protection/atp-capacity-planning#azure-atp-sensor-and-standalone-sensor-sizing-a-namesizinga) depending on the [Sensor type you choose](https://docs.microsoft.com/en-us/azure-advanced-threat-protection/atp-capacity-planning#choosing-the-right-sensor-type-for-your-deployment). 

To choose which domain controllers the tool evaluates remotely, use one of the following command line parameters:

| Command line parameters(evaluates remotely) | Evaluation |
| :------------------------------------------ | :----------- |
| `-DomainFQDN=<Domain FQDN>` | Evaluates all the domain controllers in the specified domain. |
| `-InputDCListFile=<File path>` |Evaluates all the domain controllers in the specified file (each domain controller is presented on a separate line). |
| `-UseCurrent=UserDomain` | Evaluates all the domain controllers in the domain of the user running the tool. |
| `-UseCurrent=ComputerDomain` | Evaluates all the domain controllers in the domain of the computer running the tool.|
| `-UseCurrent=Forest` | Evaluates all the domain controllers in the entire forest. |

If none of the above are specified, UseCurrent=UserDomain is used.

For more options, run "TriSizingTool -?"

Verified on the following platforms:

| OS  | Is Verified |
| :------------- | :------------- |
| Windows 10  | Yes  |
| Windows Server 2012  | Yes  |
| Windows Server 2012 R2  | Yes  |
| Windows Server 2008 R2  | Yes  |
| Windows Server 2008  | Yes  |
| Windows Server 2003  | No  |
| Windows Server 2016  | No  |
| Windows 8  | Yes  |
| Windows 7  | Yes  |
| Windows Vista  | No  |
| Windows XP  | No  |
| Windows 2000  | No  |
