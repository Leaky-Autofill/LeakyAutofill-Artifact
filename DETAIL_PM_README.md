# DETAIL_PM_README.md

This document describes the manual operations of each password manager.

We have identified that the manual operations required for PMs can be divided into the following four categories at a high level, which reduces the uniqueness of each PM's operation:

1) PM account registration or vault creation. Separately-installed PMs require an initial account registration or vault creation, which involves setting a master password. This process typically only needs to be done once.
2) Inputting/Importing Tested Data: To evaluate the autofill functionality, it is necessary to manually input or import the test data into each PM. This process is generally different in each PM but can be done by following the user interface instructions. For browser-based PMs, this step needs to be repeated for each new browser instance initiated by Selenium (without any local profile by default). We have included sample data in the `Sample Data\` directory to facilitate this process. For separately-installed PMs, this process needs to be done only once after logging into the PM.
3) Logging into PMs and unlocking PMs. For separately-installed PMs, logging into the PM or unlocking the password vault is necessary before each experiment. For PMs having desktop applications like 1Password and KeePassXC, synchronization between the desktop application and the browser extension is also required (like `Enpass` in our given instance). Based on our experiments, we have provided the necessary time (in seconds) for a proficient tester to log into the PM under the `require_login_time` field in the `Code\config\pm_config.py` file. This operation needs to be conducted each time we conduct the experiment.
4) Triggering autofill manually. A few PMs require manual intervention to trigger the autofill functionality, such as clicking the PMs' icon on the browser toolbar or clicking the right-click menu. Our code simulates this process by right-clicking the webpage and reserving time for users to complete these actions to trigger the autofill functionality. This behavior is configured in the `whether_right_click` field of the `Code\config\pm_config.py` file. When this field is set to True, the manual operations after the right-click are needed.

As the inputting/importing data and the account registration processes follow the PM's user interface, we have not included them in the documentation. Instead, we focus on the steps needed when the PM account has already been created and data has been set up.

> We run each experiment using the following command in the command line.
> 
> '''cmd
> python autopm.py --pm_name pm_name --form_type form_type --is_input is_input --is_local is_local
> '''
>
> - pm_name: the name of the password manager 
> - form_type: the type of the test form type
> - is_input: whether the concealment technique is applied on the \<input\> element itself (rather than its ancestor element)
>- is_local: whether Selenium will start the local browser instance with a local profile

## LastPass (Extension ID: hdokiejnpimakedhajhdlcegeplioahd)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the LastPass extension, testers need to manually click the LastPass icon in the browser address bar to trigger the login form from the extension. Testers must input their email address and master password to unlock LastPass.
2. **Risk-Based Authentication**: Occasionally, LastPass may trigger a risk-based authentication, sending an email verification to the tester: `To Continue: Check your inbox for an email from LastPass: [masked email address] or Review your login info and try again.` This email verification process needs to be done first, and then testers need to re-input the master password to log into the LastPass extension.
3. **Experiment Execution**: Once logged into LastPass, no further manual operations are needed to run experiments on our test cases' three web forms.

## Avira (Extension ID: caljgklbbfbcjjanaijlacgncafpegll)

> This PM provides autofill functionality only for login forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Avira extension, testers need to manually click the extension icon in the address bar. This will navigate the browser to the Avira password manager homepage. Testers must click the `Sign in` button at the top of the webpage and input their email address and password for authentication. After this, testers may also need to enter the master password for the Avira password manager.
2. **Initialization Delay**: After entering credentials, testers may need to wait approximately 10 seconds for the Avira PM to complete the initialization process.
3. **Experiment Execution**: Avira only provides autofill functionality for login forms and automatically fills these forms upon page loading. Once logged in, no further manual operations are required to run our experiments.

## Norton (Extension ID: admmjipmmciaobhojoghlmleefbicajg)

> This PM provides autofill functionality for login forms and credit card forms.

For detailed instructions on using the Norton password manager, please refer to the `README.md` file in the same directory.

## 1Password (Extension ID: aeblfdkhhhdcdjpifhhbdiojplfjncoa)

> This PM provides autofill functionality for all three web forms.

1. **Unlock Desktop Application**: Testers are recommended to first unlock the 1Password desktop application before starting any experiments.
2. **Browser Extension and Desktop Application Synchronization**: When Selenium starts a Chrome browser instance with the 1Password extension, the browser will navigate to the 1Password extension welcome page. Testers should wait for synchronization between the desktop application and the browser extension, and pin the 1Password extension to the browser toolbar if needed.
3. **Experiment Execution**: No further manual operations are required to run our experiments after synchronization. Note that for credit card forms, under specific concealment techniques, our tool may throw exceptions because the PM does not recognize forms with concealed credit card number fields as valid credit card forms.

## Bitwarden (Extension ID: nngceckbapebfimnlniiiahkandclblb)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Bitwarden extension, testers need to click the extension icon in the address bar manually. The Bitwarden login form will appear, where testers must input their email address and master password in a two-step authentication form, choosing the `master password` login method. Sometimes, Bitwarden may trigger a CAPTCHA, which testers need to solve to proceed.
2. **Autofill for Login Forms**: For login forms, Bitwarden will automatically autofill the email address and password upon page loading. No further manual operations are required for login forms during the experiment.
3. **Autofill for Personal Information and Credit Card Forms**: For personal information and credit card forms, Bitwarden requires manual intervention. Testers need to right-click the webpage, and then testers choose `Bitwarden` -> `Auto-fill identity` or `Auto-fill card` to fill these forms. To accommodate this, we have used Selenium to simulate the right-click action and reserved time for testers to complete the autofill process manually.

## Kaspersky (Extension ID: dhnkblpjbkfklfloegejegedcafpliaa)

> This PM provides autofill functionality for all three web forms.

1. **Install and Unlock Desktop Application**: Testers must install the Kaspersky password manager desktop application to use its autofill functionality. It is recommended that testers first unlock the desktop application before proceeding.
2. **Pin Browser Extension**: When Selenium starts a Chrome browser instance with the Kaspersky extension, testers are recommended to pin the Kaspersky extension icon to the browser toolbar for easier access.
3. **Autofill for Login Forms**: For login forms, Kaspersky will automatically autofill the email address and password upon page loading, requiring no further manual operations during the experiment.
4. **Autofill for Personal Information and Credit Card Forms**: Kaspersky triggers the autofill functionality through sensitive data fields like telephone, address, and credit card numbers. If these fields are hidden, Kaspersky will not autofill the forms. As a result, no manual operations are required for personal information and credit card forms during the experiments.

## Dashlane (Extension ID: fdjamakpfbbddfjaooikfcpapjohcfmg)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Dashlane extension, testers need to manually input their email address. Dashlane will then send a verification code to the tester's email, which must be entered to verify the account. After that, testers need to input their master password to unlock the Dashlane password manager.
2. **Autofill for Login and Personal Information Forms**: Once unlocked, Dashlane will automatically autofill login and personal information forms, with no further manual operations required for these types of forms during the experiment.
3. **Autofill for Credit Card Forms**: For credit card forms, Dashlane requires users to input their master password to `Unlock secure items for 5 minutes`. This requires manual intervention to input the master password when the timeout occurs.

## iCloud Passwords (Extension ID: pejdijmoenmkgeppbflobdenhhabjlaj)

> This PM provides autofill functionality only for login forms.

1. **Unlock iCloud Desktop Application**: Testers need to first unlock the iCloud desktop application before starting the experiment.
2. **Synchronize Browser Extensions with Desktop Application**: When Selenium starts a Chrome browser instance with the iCloud Passwords extension, testers must manually click the iCloud extension icon and input the same verification code shown in the desktop application. This step synchronizes the state between the browser extension and the desktop application. This experiment could be conducted on macOS, and the iCloud Passwords extension will be synchronized with the iCloud of macOS.
3. **Autofill for Login Forms**: iCloud Passwords extension provides autofill functionality only for login forms. After synchronization, no further manual operations are required during the experiment.

## Keeper (Extension ID: bfogiafebfohielmmehodmfbbebbbpei)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Keeper extension, testers need to manually input their email address. Testers can then choose one of the following methods to proceed: `Email,` `Keeper Push,` or `Two-Factor Method.` After selecting a method, they must input the master password to unlock the Keeper password manager.
2. **Autofill for Login Forms**: Once Keeper is unlocked, no further manual operations are required for login forms, as they will autofill automatically.
3. **Autofill for Personal Information and Credit Card Forms**: For personal information and credit card forms, testers must manually right-click on the webpage, select `Keeper` from the pop-up menu, and then choose `Addresses` or `Payment Card` to trigger the autofill functionality. Additionally, a `confirmation dialog` will appear before autofilling these forms. To facilitate this, we have used Selenium to simulate the right-click action and reserved time for testers to complete the autofill process manually.

## MultiPassword (Extension ID: cnlhokffphohmfcddnibpohmkdfafdli) 

> This PM provides autofill functionality only for login forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the MultiPassword extension, the browser navigates to the MultiPassword welcome page. Testers need to manually click the `Log in` button and input their `email,` `master password,` and `private key` in the form.
2. **Experiment Execution**: Once MultiPassword is unlocked, no further manual operations are required to complete the experiments.

## True Key (Extension ID: cpaibbcbodhimfnjnakiidgbpiehfgci) 

> This PM provides autofill functionality only for login forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the True Key extension, the browser navigates to the True Key dashboard page. Testers need to manually input their email address and master password. True Key will then send an email to the tester for device confirmation. After clicking the link in the email, the True Key extension will be unlocked.
2. **Experiment Execution**: Once unlocked, no further manual operations are required to complete the experiments.

## RoboForm (Extension ID: pnlccmojcmeohlpggmfnbbiapkmbliob) 

> This PM provides autofill functionality for all three web forms.

1. **Install and Unlock Desktop Application**: Testers need to install the RoboForm desktop application to use the autofill functionality of the RoboForm password manager. The RoboForm password vault needs to be unlocked before starting the experiment.
2. **Experiment Execution**: Once the RoboForm desktop application is installed and the vault is unlocked, when Selenium starts a Chrome browser instance with the RoboForm extension, no further manual operations are required to complete the experiments.

## DualSafe (Extension ID: lgbjhdkjmpgjgcbcdlhkokkckpjmedgc) 

> This PM provides autofill functionality only for login forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the DualSafe Password Manager extension, the browser automatically navigates to the login page. Testers need to manually click the `Sign In` button to log into the password manager.
2. **Autofill for Login Forms and Experiment Execution**: DualSafe only provides autofill functionality for login forms, and credentials are automatically filled in on page loading. After logging into the password manager, no further manual operations are required to complete the experiments.

## NordPass desktop version (Extension ID: fooolghllnmhmmndgjiamiiodkpenpbb)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: Testers are recommended to unlock the NordPass desktop application first by inputting their email address and master password. After unlocking the password vault, testers need to confirm that the 4-digit pairing code displayed on the extension matches the one shown in the desktop application. Note that this process may not be conducted smoothly during experiments, and multiple attempts may be required to successfully log into the NordPass desktop application. 
2. **Autofill for Login and Personal Information Forms**: Once logged in, no further manual operations are needed for login forms and personal information forms during the experiments.
3. **Autofill for Credit Card Forms**: NordPass’s autofill functionality for credit card forms is triggered only on the credit card number field, which is considered a sensitive field and is hidden in our experiments. If this field is hidden, NordPass will not autofill the form. Consequently, no manual operations are required for credit card forms during experiments.

> The tested extension version in our experiments is old now and may not work. The desktop version seems no longer updated.

## ExpressVPN Keys (Extension ID: blgcbajigpdfohpgcmbbfnphcgifjopc) 

> This PM provides autofill functionality for login forms and credit card forms.

1. **Unlock Desktop Application**: Testers need to first unlock the ExpressVPN Keys desktop application by inputting the master password.
2. **Re-Authentication in Browser**: When Selenium starts a Chrome browser instance with the ExpressVPN Keys extension, testers may need to re-enter the master password to unlock the password manager in the browser.
3. **Experiment Execution**: Once unlocked, no further manual operations are required to complete the experiments.

## Dropbox Passwords (Extension ID: bmhejbnmpamgfnomlahkonpanlkcfabg) 

> This PM provides autofill functionality for login forms and credit card forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Dropbox Passwords extension, testers need to input their email address and master password to unlock the Dropbox Passwords extension.
2. **Experiment Execution**: After the extension is unlocked, no further manual operations are required during the experiments.
3. **Free Version Limitation**: Note that the free version of Dropbox Passwords can only be linked to three devices (a Selenium-initiated Chrome instance is regarded as a new device). We recommend testers start a local profile with the Dropbox Passwords extension by running the command with the `--is_local 1` parameter.

## KeePassXC (Extension ID: oboonakemofpalcgghocfoadofidjkkk)

> This PM provides autofill functionality only for login forms.

1. **Unlock the KeePassXC Desktop Application**: Testers need first to unlock the KeePassXC PM and open the password vault in KeePassXC.
2. **Link Browser Extensions with Desktop Application**: Testers need first to open the KeePassXC desktop application and configure the browser integration. Then, when Selenium starts a Chrome browser instance with the KeePassXC extension, testers click the KeePassXC extension to connect to the database of the desktop application and name this connection.
3. **Autofill Confirmation**: During the first autofill attempt, testers need to click the pop-up dialog to allow KeePassXC to fill in credentials on the website. After this initial confirmation, no further manual operations are required for subsequent autofill actions.

## NordPass (Extension ID: eiaeiblijfjekdanodkjadfinkhbfgcd)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the NordPass extension, the browser will navigate to the homepage of NordPass. Testers need to click the `Log in` button and input their email address and master password in a two-step authentication process to unlock the NordPass extension.
2. **Experiment Execution for Login and Personal Information Forms**: After unlocking the NordPass extension, no further manual operations are needed for login forms and personal information forms during the experiments.
3. **Credit Card Forms**: For credit card forms, NordPass’s autofill functionality is only triggered by the credit card number field, which is a sensitive field that we hide in our experiments. If this field is hidden, NordPass will not autofill the forms. As a result, no manual operations are required for credit card forms during the experiments.

> The tested extension version in our experiments is old now and may not work. Testers could use the latest version of NordPass.

## Passbolt (Extension ID: didegimhafipceonhjepacocaffmoppf)

> This PM provides autofill functionality only for login forms.

1. **Login Process**: Passbolt requires testers to click the verification link sent via email after account registration to log into the password manager (`I already have a passbolt account`). Testers can also choose to re-register a new account (`I don't have an account yet`), log in, and manually add sample data (Passbolt only supports autofill functionality for login forms). 
2. **Master Password Input**: After logging into Passbolt, users are required to re-enter the master password before autofilling credentials by default. For ease during experiments, testers can disable the master password requirement in the settings.
3. **Selenium Compatibility**: Passbolt is generally difficult to log into on a Selenium-initiated Chrome instance. We recommend testers start a local profile with the Passbolt extension by using the `--is_local 1` parameter.

## Proton (Extension ID: ghmbeldphafepmbegfdlkpapadhbakde)

> This PM provides autofill functionality only for login forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Proton extension, testers should pin the Proton extension to the browser toolbar. Testers can then click the `Sign in with Proton` button to navigate the browser to the login webpage. Testers will need to input their email address and password to sign in.
2. **CAPTCHA Verification**: During the login process, a CAPTCHA for human verification may appear. Testers will need to solve it manually.
3. **Post-Login**:  After successfully logging in and unlocking the Proton password manager, no further manual operations are required for the experiment.

## Microsoft Autofill (Extension ID: fiedbfgcleddlbcmgdigjgdfcggjcion)

> This PM provides autofill functionality for all three web forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Microsoft Autofill extension, testers should pin the extension to the browser toolbar. Testers need to sign in to their Microsoft account in order to use the password manager extension.
2. **Adding Sample Data**: If there is no sample data stored in the Microsoft account, testers must manually add sample data for the three web forms (login, personal information, and credit card forms) before proceeding with the experiment.
3. **Experiment Execution**: After adding the necessary data, no further manual operations are required during the experiment.

## Zoho Vault (Extension ID: igkpcodhieompeloncfnbekccinhapdb) 

> This PM provides autofill functionality for login forms and credit card forms.

1. **Login Process**: When Selenium starts a Chrome browser instance with the Zoho Vault extension, testers should pin the extension to the browser toolbar. Then, testers need to click the extension icon, which navigates the browser to the Zoho Vault login page. Testers need to input their email address and account password to authenticate their identity.
2. **One-Time Password (OTP) Verification**: Zoho Vault may send a one-time password (OTP) to verify the email address. Testers must input the OTP into the designated field to continue.
3. **Browser Access Confirmation**: After verifying the OTP, Zoho Vault will prompt users to choose whether to allow the Zoho Vault Browser Extension to access browser information. Testers must confirm this step to proceed.
4. **Unlocking the Vault**: Finally, testers need to re-click the extension icon on the toolbar and input the master password in the pop-up window to unlock Zoho Vault.
5. **Experiment Execution**: After unlocking the vault, no further manual operations are required for the experiments.

## Enpass (Extension ID: kmcfomidfpdkfieipokbalgegidffkal) 

> This PM provides autofill functionality for all three web forms.

For detailed instructions on using the Enpass password manager for login forms and credit card forms, please refer to the `README.md` file in the same directory.

For personal information forms, testers are required to wait for the right-click operation conducted by Selenium, click the Enpass extension icon on the browser toolbar, choose the `identifier` information, and trigger the autofill functionality by double-click the item.

## SafeInCloud (Extension ID: lchdigjbcmdgcfeijpfkpadacbijihjl)

> This PM provides autofill functionality for login forms and credit card forms.

1. **Setup and Unlocking**: Testers are recommended to install and unlock the SafeInCloud desktop application. When Selenium starts a Chrome browser instance with the SafeInCloud extension, testers should pin the extension to the browser toolbar. Then, testers need to click the extension icon on the toolbar and unlock SafeInCloud by inputting the master password.
2. **Login Form Autofill**: For login forms, testers need to click the extension icon on the toolbar to trigger the autofill functionality. We utilize Selenium to right-click the webpage to inform testers to click the extension during the reserved time.
3. **Credit Card Form Autofill**: For credit card forms, testers need to click the extension icon on the toolbar and choose the credit card item to trigger the autofill functionality. The same method is used with login forms, using Selenium to assist testers in triggering the extension.

## Google Chrome

> This PM provides autofill functionality for all three web forms.

For detailed instructions on using the built-in-browser password manager in Chrome browser, please refer to the `README.md` file in the same directory.

## Microsoft Edge

> This PM provides autofill functionality for all three web forms.

The main process is the same as that of the Chrome browser. If the tester does not use the local profile, they must add sample data into the built-in-browser PM. However, after this addition process, there is no further manual operation during the experiment.

## Safari

> This PM provides autofill functionality for all three web forms.

Safari does not support loading user profiles with pre-entered sample data or allowing subsequent manual interactions for testing purposes. Due to this limitation, experiments involving Safari are conducted manually on macOS Monterey 12.7.4.

Specifically, we manually visit all the test websites, trigger the autofill functionality of the Safari browser to fill in the web forms. Then, we utilize JavaScript `document.getElementById(#id).value` command in the DevTools to retrive the vaule in the hidden fields. Finally, we can determine whether Safari will autofill data into hidden fields.

## Mozilla Firefox

> This PM provides autofill functionality for all three web forms.

The main process is the same as that of the Chrome browser. If the tester does not use the local profile, they need to add sample data into the built-in-browser PM. However, after this addition process, there is no further manual operation during the experiment. Note that Firefox by default does not allow autofilling personal information in locations except US. Thus, we recommend that testers could set the country of the personal information as `US`.

## Opera

> This PM provides autofill functionality for all three web forms.

The main process is the same as that of the Chrome browser. If the tester does not use the local profile, they need to add sample data into the built-in-browser PM. However, after this addition process, there is no further manual operation during the experiment.

## Brave 

> This PM provides autofill functionality for all three web forms.

The main process is the same as that of the Chrome browser. If the tester does not use the local profile, they need to add sample data into the built-in-browser PM. However, after this addition process, there is no further manual operation during the experiment.
