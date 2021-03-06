===
# AIP Scanner Classification, Labeling, and Protection
[:arrow_left: Home](#azure-information-protection)

The Azure Information Protection scanner allows you to  classify and protect sensitive information stored in on-premises CIFS file shares and SharePoint sites.  

In this exercise, you will configure conditions for automatic classification.  After that, we will run the AIP Scanner in enforce mode to classify and protect the identified sensitive data. This Exercise will walk you through the items below.

- [Defining Automatic Conditions](#defining-automatic-conditions)
- [Enforcing Configured Rules](#enforcing-configured-rules)
- [Reviewing Protected Documents](#reviewing-protected-documents)

> [!Alert] This exercise requires completion of the previous [AIP Scanner Discovery](#aip-scanner-discovery) exercise.  If you did not already complete that exercise, please do so prior to continuing.

---

## Defining Automatic Conditions
[:arrow_up: Top](#aip-scanner-classification-labeling-and-protection)

The Azure Information Protection Scanner uses Automatic conditions to identify sensitive content to classify, label, and protect.  In this exercise, we will configure some of these conditions.  If you completed the Base Configuration exercise, you have already done some of these, but we will go through the process for those that may have skipped that exercise.

1. [] On @lab.VirtualMachine(Client01).SelectLink, log in with the password +++@lab.VirtualMachine(Client01).Password+++.
2. [] Open the browser window with the Azure Portal (AIP Blade).

	> [!HINT] If necessary, open an InPrivate browsing session and navigate to ```https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/globalBlade``` and login with the credentials below. 
	>
	> ```@lab.CloudCredential(17).Username```
	>
	> ```@lab.CloudCredential(17).Password```

3. [] Under **Dashboards** on the left, click on **Data discovery (Preview)** to view the results of the discovery scan we performed previously.

	!IMAGE[Dashboard.png](\Media\Dashboard.png)

	> [!KNOWLEDGE] Notice that there are no labeled or protected files shown at this time.  This uses the AIP P1 discovery functionality available with the AIP Scanner. Only the predefined Office 365 Sensitive Information Types are available with AIP P1 as Custom Sensitive Information Types require automatic conditions to be defined, which is an AIP P2 feature.

	> [!NOTE] Now that we know the sensitive information types that are most common in this environment, we can use that information to create **Recommended** conditions that will help guide user behavior when they encounter this type of data.

	> [!ALERT] If no data is shown, it may still be processing. Continue with the lab and come back to see the results later.

1. [] Under **Classifications** on the left, click **Labels** then expand **Confidential**, and click on **All Employees**.

	^IMAGE[Open Screenshot](\Media\jyw5vrit.jpg)
1. [] In the Label: All Employees blade, scroll down to the **Configure conditions for automatically applying this label** section, and click on **+ Add a new condition**.

	!IMAGE[cws1ptfd.jpg](\Media\cws1ptfd.jpg)
1. [] In the Condition blade, in the **Select information types** search box, type ```EU``` and check the boxes next to the **items shown below**.

	!IMAGE[xaj5hupc.jpg](\Media\xaj5hupc.jpg)

1. [] Click **Save** in the Condition blade and **OK** to the Save settings prompt.

	^IMAGE[Open Screenshot](\Media\41o5ql2y.jpg)
1. [] In the Labels: All Employees blade, in the **Configure conditions for automatically applying this label** section, click **Automatic**.

1. [] Click **Save** in the Label: All Employees blade and **OK** to the Save settings prompt.

	^IMAGE[Open Screenshot](\Media\rimezmh1.jpg)
1. [] Press the **X** in the upper right-hand corner to close the Label: All Employees blade.

	^IMAGE[Open Screenshot](\Media\em124f66.jpg)
1. [] Next, expand **Highly Confidential** and click on the **All Employees** sub-label.

	^IMAGE[Open Screenshot](\Media\2eh6ifj5.jpg)
1. [] In the Label: All Employees blade, scroll down to the **Configure conditions for automatically applying this label** section, and click on **+ Add a new condition**.

	^IMAGE[Open Screenshot](\Media\8cdmltcj.jpg)
1. [] In the Condition blade, in the search bar type ```credit``` and check the box next to **Credit Card Number**.

	^IMAGE[Open Screenshot](\Media\9rozp61b.jpg)
1. [] Click **Save** in the Condition blade and **OK** to the Save settings prompt.

	^IMAGE[Open Screenshot](\Media\ie6g5kta.jpg)
15. [] In the Labels: All Employees blade, in the **Configure conditions for automatically applying this label** section, click **Automatic**.

   !IMAGE[245lpjvk.jpg](\Media\245lpjvk.jpg)

1. [] Click **Save** in the Label: All Employees blade and **OK** to the Save settings prompt.

	^IMAGE[Open Screenshot](\Media\gek63ks8.jpg)

1. [] Press the **X** in the upper right-hand corner to close the Label: All Employees blade.

	^IMAGE[Open Screenshot](\Media\wzwfc1l4.jpg)

---

## Enforcing Configured Rules
[:arrow_up: Top](#aip-scanner-classification-labeling-and-protection)

In this task, we will set the AIP scanner to enforce the conditions we set up in the previous task and have it rerun on all files using the Start-AIPScan command.

1. [] Switch to @lab.VirtualMachine(Scanner01).SelectLink and log in with the password +++@lab.VirtualMachine(Scanner01).Password+++.
1. [] In an **Administrative PowerShell** window, type ```C:\Users\LabUser\Desktop\StartEnforce.ps1``` and press **Enter**. 

    > [!KNOWLEDGE] The script runs the code below. This script is available online at https://aka.ms/labscripts
	>
	> Set-AIPScannerConfiguration -Enforce On -DiscoverInformationTypes PolicyOnly
	>
	>Start-AIPScan
    
	> [!HINT] Note that this time we used the DiscoverInformationTypes -PolicyOnly switch before starting the scan. This will have the scanner only evaluate the conditions we have explicitly defined in conditions.  This increases the effeciency of the scanner and thus is much faster.  After reviewing the event log we will see the result of the enforced scan.
	>
	>!IMAGE[k3rox8ew.jpg](\Media\k3rox8ew.jpg)
		

1. [] Switch back to @lab.VirtualMachine(Client01).SelectLink and log in using the password +++@lab.VirtualMachine(Client01).Password+++.
1. [] Look in the reports directory we opened previously at ```\\Scanner01.contoso.azure\c$\users\aipscanner\AppData\Local\Microsoft\MSIP\Scanner\Reports```, you will notice that the old scan reports are zipped in the directory and only the most recent results are showing.  
	
	> If needed, use the credentials below:
	>
	>```Contoso\LabUser```
	>
	>```Pa$$w0rd```

	>[!NOTE] The DetailedReport.csv now shows the files that were protected.
	>
	>
	>!IMAGE[6waou5x3.jpg](\Media\6waou5x3.jpg)


---

## Reviewing Protected Documents
[:arrow_up: Top](#aip-scanner-classification-labeling-and-protection)

Now that we have Classified and Protected documents using the scanner, we can review the documents to see their change in status.

1. [] Switch to @lab.VirtualMachine(Client01).SelectLink and log in with the password +++@lab.VirtualMachine(Client01).Password+++.

2. [] Navigate to ```\\Scanner01.contoso.azure\documents```. 

	> If needed, use the credentials below:
	>
	>```Contoso\LabUser```
	>
    >```Pa$$w0rd```

	^IMAGE[Open Screenshot](\Media\hipavcx6.jpg)
3. [] Open one of the Contoso Purchasing Permissions documents.

	> [!NOTE] If asked to log in, use the credentials below.
	>
	> ```AdamS@@lab.CloudCredential(17).TenantName```
	>
	> ```pass@word1```

    > [!NOTE] Observe that the document is classified as Highly Confidential \ All Employees. 
    >
    > !IMAGE[s1okfpwu.jpg](\Media\HCAE.jpg)

4. [] Next, in the same documents folder, open one of the pdf files.
5. [] When prompted by Adobe, enter ```AdamS@@lab.CloudCredential(17).TenantName``` and press OK.
6. [] Check the box to save credentials and press OK.

	> [!NOTE] The PDF will now open and display the sensitivity across the top of the document.

	> [!Knowledge] The latest version of Acrobat Reader DC and the MIP Plugin have been installed on this system prior to the lab. Additionally, the sensitivity does not display by default in Adobe Acrobat Reader DC.  You must make the modifications below to the registry to make this bar display.
	>
	> In **HKEY_CURRENT_USER\Software\Adobe\Acrobat Reader\DC\MicrosoftAIP**, create a new **DWORD** value of **bShowDMB** and set the **Value** to **1**.
	>
	> !IMAGE[1547416250228](\Media\1547416250228.png)
---

===
# AIP Scanner CLP Exercise Complete

In this exercise, we configured the AIP scanner to use automatic conditions to classify, label, and protect documents in our defined repositories.  Choose one of the exercises below or click the Next button to continue sequentially.

- [AIP Scanner Discovery](#aip-scanner-discovery)
- [Base Configuration](#configuring-azure-information-protection-policy)
- [Bulk Classification](#bulk-classification)
- [Security and Compliance Center](#security-and-compliance-center)
- [AIP Analytics Dashboards](#aip-analytics-dashboards)
- [Exchange IRM](#exchange-online-irm-capabilities)

---

