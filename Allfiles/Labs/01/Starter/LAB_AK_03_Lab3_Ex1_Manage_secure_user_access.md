# Learning Path 3 - Lab 3 - Exercise 1 - Manage secure user access 

Holly has then been asked by Adatum’s CTO to deploy Microsoft Entra Multifactor Authentication (MFA), Pass-through Authentication (PTA), and Microsoft Entra Smart Lockout. These three features will help strengthen password management throughout the organization in preparation for Copilot for Microsoft 365. For PTA, you will deploy it using Microsoft Entra Connect Sync. And for Smart Lockout, you will deploy it using Group Policy Management. 

For MFA, you will create a Conditional Access policy to deploy MFA for all of Adatum's users. You will then modify it to exclude Holly and the selected members of her Microsoft 365 pilot project team. That will save you from having to use MFA when signing in with them, as well as provide you with experience on how to exclude users in a Conditional Access policy. 

**Note:** Excluding specific users from using MFA is not something you would normally do in a real-world scenario. However, for the purpose of saving time in this classroom training lab, we will disable MFA for the test users. 

### Task 1: Create a Conditional Access policy to implement MFA

As your training indicated, there are three ways to implement MFA - with Conditional Access policies, with security defaults, and with legacy per-user MFA (not recommended for larger organizations). In this exercise, you'll enable MFA through a Conditional Access policy, which is the method that Microsoft recommends. 

Adatum has directed Holly to enable MFA for all its Microsoft 365 users - both internal and external. However, for the purpose of testing Adatum's Microsoft 365 pilot project implementation, Holly wants to exclude the members of the M365 pilot project group from having to use MFA to sign in. Once the pilot project is complete, Holly plans to update the policy by removing the exclusion of this group from the MFA requirement. The policy will also include two other requirements. It will require MFA for all cloud apps, and it will require MFA even if a user signs in from a trusted location. 

1. In the prior lab exercise, you were working on LON-DC1. In this task, you'll be working back on your Client 1 machine. <br/>

	Switch to **LON-CL1**.

2. On the LON-CL1 VM, the **Microsoft 365 admin center** should still be open in your Microsoft Edge browser from an earlier task. You should be signed into Microsoft 365 as **Holly Dickson**.
   
3. In the **Microsoft 365 admin center**, under the **Admin centers** section in the navigation pane, select **Identity**. Doing so opens the Microsoft Entra admin center in a new browser tab. If a **Pick an account** window appears, select **Holly Dickson's account**.

4. In the **Microsoft Entra admin center**, select **Protection** in the navigation pane, and then select **Conditional Access**.

5. On the **Conditional Access | Overview** page, select **Policies** in the middle navigation pane.

6. On the **Conditional Access | Policies** page, on the menu bar at the top of the page, select **+Create new policy**.

7. On the **New Conditional Access policy** window, enter **MFA for all Microsoft 365 users** in the **Name** field.

8. You will begin by defining the MFA requirement for users. Under the **Users** group, select **0 users and groups selected**. Doing so displays two tabs - **Include** and **Exclude**.

9. Under the **Include** tab, select **All users**. Note the warning message that appears. You will address this in the next two steps.

10. Select the **Exclude** tab. To avoid system lockout, as the prior warning message indicated, you want to exclude your Global administrators - in this case, Holly. Holly also wants to exclude the other Microsoft 365 pilot project group members for the sake of expediency when testing. Once Microsoft 365 goes live at Adatum, Holly will remove the pilot project group from the Exclude list in this Conditional Access policy and simply exclude herself, the MOD Administrator, and a few other Global admins. But for now, Holly wants to exclude the entire pilot project group. <br/>

	To do so, select the **Users and groups** check box. 

11. In the **Select excluded users and groups** window that appears, you want to select the Microsoft 365 pilot project group. The **All** tab is displayed by default. To quickly find the pilot project group, select the **Groups** tab. In the list of active groups, select the check box next to the **M365 pilot project** group, and then select the **Select** button at the bottom of the window. Back on the **New Conditional Access policy** window, note the message that appears under the **Users** section. 

12. You will now define the MFA requirement for all cloud apps. Under the **Target resources** section, select **No target resources selected**. Doing so displays two tabs - **Include** and **Exclude**.

13. Select the **Select what this policy applies to** drop-down field to see the various options in the drop-down menu. Select **Resources (formerly cloud apps)**. 

14. Under the **Include** tab, note that the default setting is **None**. If you did not change this setting, then no cloud apps would require MFA - and that includes Microsoft 365. So even if you created this policy and selected the option to require MFA for all users, but you left this **Target resources** setting to **None**, then any user signing into Microsoft 365 would not have to use MFA. <br/>

	Under the **Include** tab, select the **Select resources** option. Doing so displays two sections - **Edit filter** and **Select**. Under the **Select** section, select **None**. 

15. In the **Select Resources** pane that appears, scroll down through the list of apps to see all the different apps that you could require MFA for. **Do NOT select any of the apps.** We're having you scroll through this list just to get a feel for how granular you can get when requiring MFA should you decide to limit MFA to certain apps in your real-world deployments.  <br/>

	For Adatum, Holly wants to require MFA for all cloud apps, which is typically a more common business scenario than selecting specific apps. Under the **Include** tab, select the **All resources (formerly 'All cloud apps')** option. Adatum will not exclude any cloud apps from MFA authentication. You can select the **Exclude** tab if you want to see the options it provides. It works basically the same as the **Include** tab. You can view this tab, but do NOT select any cloud apps for exclusion. 

16. Finally, you will define the MFA requirement for all user sign-in locations. In some scenarios, organizations may only require MFA if a user signs-in from an untrusted location. However, Adatum wants to require MFA for all included users, regardless of the location from where they sign in. <br/>

	Under **Conditions**, select **0 conditions selected**. Doing so displays a list of potential conditions the policy will check for. For this lab exercise, under the **Locations** condition, select **Not configured**. Doing so displays a **Configure** toggle switch and two tabs - **Include** and **Exclude**. Both tabs are currently disabled.

17. Set the **Configure** toggle switch to **Yes**, which enables the two tabs. 

18. Under the **Include** tab, verify **Any network or location** is selected (select it if necessary). Select the **Exclude** tab. If your organization recognizes specific IP addresses or ranges of addresses as "trusted", you can exclude the MFA requirement if a user signs in from one of those locations. However, Adatum wants to require MFA for all user sign-in attempts, regardless of their location. This will include both internal and external user sign-ins. Verify the **Selected networks and locations** option is selected, and under the **Select** section, verify it says **None**. By not specifying any selected locations, this setting ensures that no locations are excluded from MFA. 

19. Under the **Access controls** section, under the **Grant** group, select **0 controls selected**. Doing so displays a **Grant** pane.

20. In the **Grant** pane that appears, verify the **Grant access** option is selected (select it if necessary). Note all the access controls that are available that can be enabled with this policy. This policy will only require MFA, so select the **Require multifactor authentication** check box. Select the **Select** button at the bottom of the **Grant** pane, which closes the pane. 

21. At the bottom of the **New Conditional Access policy** window, in the **Enable policy** field, select **On**.

22. Note the warning message and options that appear at the bottom of the page that warn you not to lock yourself out. Select the option **I understand that my account will be impacted by this policy. Proceed anyway.** In fact, Holly won't be impacted since she's a member of the M365 pilot project group that is excluded from this policy.

23. Select the **Create** button to create the policy.

24. On the **Conditional Access | Policies** window that appears, verify the **MFA for all Microsoft 365 users** policy appears and that its **State** is set to **On**.

25. Remain logged into LON-CL1 with all your Microsoft Edge browser tabs open for the next task.


### Task 2: Test MFA for both an included and excluded user

To test the Conditional Access policy that you just created, you will sign-out of Microsoft 365 as Holly, and then you'll sign back in as Adele Vance. Adele is not a member of the M365 pilot project group, so Microsoft Entra should require that she use MFA when signing in. Once you sign-in as Adele and verify that MFA works, you will sign-out as Adele and then sign back in as Holly. Since Holly is a member of the M365 pilot project group that was excluded from using MFA in the Conditional Access policy, you should not have to use MFA when signing in as Holly. Similarly, you won't have to use MFA when you sign in as the various members of the M365 pilot project group in the remaining labs in this course.

**Important:** To implement MFA, you must use your mobile phone to receive a verification code so that you can enter it into your tenant as a second form of authentication. If you do not have a phone, you can still test your Conditional Access policy. For students without a phone, when you sign in as Adele Vance, the system will require you to sign in with a second form of authentication. At that point, you can simply cancel out of your sign-in and then sign back in as Holly, who will not require MFA. While you will not complete the MFA sign-in for Adele, you can still verify that the system forces you to use it when attempting to sign-in.

1. On the LON-CL1 VM, the **Microsoft 365 admin center** should still be open in your Microsoft Edge browser from the prior task. You should be signed into Microsoft 365 as **Holly Dickson**. You will begin by signing out of Microsoft 365. On the **Microsoft 365 admin center** tab, select Holly's name in the upper right corner of your browser. In the **Holly Dickson** window that appears, select **Sign out.** 
	
2. Once you are signed out of Microsoft 365 as Holly, close your browser session to clear your cache. Then select the **Edge** icon on your taskbar to open a new browser session. In your browser, go to the **Microsoft 365 Home** page by entering the following URL in the address bar: **https://portal.office.com/** 

3. In the **Pick an account** window that appears, select **Use another account**. 

4. In the **Sign in** window, enter **AdeleV@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider) and then select **Next**. In the **Enter password** window, enter the **User Password** provided by your lab hosting provider and select **Sign in**.

5. Because MFA is enabled for all users except for the M365 pilot project group members (of which, Adele is not a member), a **More information required** window appears. Select **Next**. This returns the **Microsoft Authenticator** page, which is the starting point for signing in with MFA. <br/>

	**Important:** If you do not have a phone, then this is as far as you can go when attempting to sign-in as Adele. Even though you can't complete the sign-in, you have verified that the first part of your Conditional Access policy is working, since it requires Adele to sign-in using MFA. At this point, skip to step #18 so that you can sign back in as Holly.

6. On the **Microsoft Authenticator** page that appears, you can download this mobile app or use a different method for MFA verification. For the purposes of this lab, we recommend that you use your mobile phone so that you do not have to take time installing the Microsoft Authenticator app that you may not use again after this training class. Select the **I want to set up a different method** option at the bottom of the page (**Important:** Do NOT confuse this link with the **I want to use a different authenticator app** that appears above it). 

7. In the **Choose a different method** dialog box that appears, select the drop-down arrow in the **Which method would you like to use?** field, select **Phone**, and then select **Confirm**. 

8. In the **Phone** window that appears, under **What phone number would you like to use?** field, select your country or region, and then in the field next to it, enter your phone number (use your country specific formatting). Verify the **Receive a code** option is selected and then select **Next**.

9. Retrieve the verification code from the text message that is sent to your phone.

10. In the **Phone** window, enter the 6-digit verification code in the code field and then select **Next**. When the **Phone** window displays a message indicating your phone was registered successfully, select **Next**.

11. On the **Success!** page, select **Done**.

12. Microsoft has implemented a new security policy in the trial tenants that are used in its training labs. All of the predefined test user accounts are configured so that students must change their initial password at their next sign-in. You must do so now with Adele. <br>

	In the **Update your password** window that appears, enter the **User Password** provided by your lab hosting provider in the **Current password** field. Then in the **New password** and **Confirm password** fields, enter the New User Password that you defined for all test users at the start of the lab. Select **Sign in**.

13. If a **Stay signed in?** dialog box appears, select the **Don’t show this again** check box and then select **Yes.** 

14. If a **Welcome to Microsoft 365** dialog box appears, select the right arrow twice and then select the check mark.

15. If a **Create with Microsoft 365** window appears, select the **X** to close it.

16. On the **Welcome to Microsoft 365** page, select the **App launcher** icon, then select **Word**. This opens **Microsoft Word Online**. Doing so validates that you can access a Microsoft 365 app after signing in using MFA.  <br/>

	**Important:** You have now verified that the first part of the Conditional Access policy that you created works. The policy requires that a user who is not a member of the Microsoft 365 pilot project team must sign-in using MFA. You verified this works when you signed in as Adele. You will now sign out as Adele and sign back in as Holly, during which you will verify that the second part of the Conditional Access policy also works. You should NOT have to use MFA when signing in as Holly, since she's a member of the M365 pilot project group, which is excluded from the MFA requirement in the Conditional Access policy.

17. On the **Microsoft 365 admin center** tab, select the icon for Adele's account in the upper right corner of your browser. In the **Adele Vance** window that appears, select **Sign out.** <br/>
	
18. Close your browser session to clear your cache. Select the **Edge** icon on your taskbar to open a new browser session. In your browser go to the **Microsoft 365 Home** page by entering the following URL in the address bar: **https://portal.office.com/** 

19. In the **Pick an account** window, select **Holly@xxxxxZZZZZZ.onmicrosoft.com** (where xxxxxZZZZZZ is the tenant prefix provided by your lab hosting provider) and then select **Next**. In the **Enter password** window, enter the new Administrative Password that you defined for all test users at the start of the lab and later assigned to Holly's account. Select **Sign in**.

20. Because MFA is required for all users except for the M365 pilot project team members (of which, Holly is a member), MFA will not be required. Since MFA is not required, the system displays the **Microsoft 365 Home** page in the **Home | Microsoft 365** tab. 

21. If a **Welcome to Microsoft 365** dialog box appears, select the right arrow twice and then select the check mark.

22. If a **Create with Microsoft 365** window appears, select the **X** to close it.

23. On the **Welcome to Microsoft 365** page, select the **Admin** icon in the side pane to navigate to the **Microsoft 365 admin center**. <br/>

	**Important:** You have now verified that the second part of the Conditional Access policy that you created works. The policy excludes members of the Microsoft 365 pilot project group from signing-in using MFA. Holly is a member of this group, and she did not have to sign in using MFA.

24. Remain signed into LON-CL1 with the **Microsoft 365 admin center** open in your browser.

# Proceed to Lab 3 - Exercise 2


