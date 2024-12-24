# Leaky Autofill

Research Artifact for our ACSAC 2024 paper: "Leaky Autofill: An Empirical Study on the Privacy Threat of Password Managers' Autofill Functionality."

Authors: Yanduo Fu and Ding Wang (Supervisor)

> We recommend reading the `README.md` file in our GitHub repository rather than the `README.md` file contained in the virtual machine image.

In this paper, we develop a semi-automated tool to test the autofill functionality of password managers (PMs). This tool leverages [Selenium](https://www.selenium.dev/) with Python to reduce the manual effort of clicking, inputting, and recording data during testing. It simulates user interactions by triggering the autofill functionality of PMs, fill in data into web forms, and recording the filled results (i.e., whether PM-stored data is filled into web forms).

In our work, we utilize this tool to examine whether PMs fill data into hidden fields concealed by different techniques (e.g., CSS properties). Our [tested website](https://leakyautofill.github.io) is modified from 1Password's autofill functionality test website: https://fill.dev.

Our testing pipeline includes the following steps:

1. **Register a PM Account**: Create an account for the PM.
2. **Import/Add Test Data**: Add test data into the PM.
3. **Log into the PM Account**: Access the PM account using credentials.
4. **Access the website and trigger Autofill using Selenium**: Access the testing website and use Selenium to trigger the autofill functionality to fill the data into web forms.
5. **Record Results**: Use Selenium to log which data gets auto-filled.

In some processes, we have reserved some steps where testers need to intervene with manual operations to complete tasks that Selenium is hard to perform. Readers could refer to the examples provided below (`E1-E3`) and the videos in [`Videos\`] to learn more about the process of manual operations. With the assistance of manual operations, we obtain the filled results of 30 PMs in three forms with hidden field concealment by various techniques (as shown in Tables 2 and 3 in our paper).

Our artifact includes:

- Source code of our semi-automated tools [[Link]](https://github.com/Leaky-Autofill/LeakyAutofill-Artifact)
- Source code of the testing websites modified using https://fill.dev/ [[Link]](https://github.com/LeakyAutofill/leakyautofill.github.io) (and hosted in [GitHub](https://leakyautofill.github.io))
- 24 PM extensions used in our experiments (the other six are built-in-browser PMs) [[Link]](https://zenodo.org/records/13380735)

To facilitate the usage of this artifact, we provide a [`Virtual Machine image`](https://zenodo.org/records/13380735/files/leakyautofill.ova) of Windows 11 with the necessary components to execute the artifact. Besides, all the above codes and browser extensions are archived in [Zenodo](https://zenodo.org/records/13380735).

## Prerequisites

### Security, privacy, ethical concerns

If AE reviewers wish to use their own created PM accounts for testing, we suggest adding fake data to the password manager (e.g., provided in `[Sample Data\]`). We can ensure that the test website we deploy will not maliciously collect users' private information. However, storing personal sensitive information in an infrequently used PM could potentially lead to security risks.

### Hardware dependencies

Most of our experiments are conducted on a machine with the following specifications:

- RAM: 16GB
- Storage: 512 GB Hard Disk 
- Operation System: Windows 11 

Our source code and extensions occupy approximately one GB. To ease the AE committee's review, we provide a testing environment on Windows 11 in the [`Virtual Machine image`](https://zenodo.org/records/13380735/files/leakyautofill.ova) of [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

These tests require users to register PM accounts and import test data, and human operations are necessary to trigger several PMs' autofill functionality. Besides, as PMs contain users' passwords and other sensitive information, some PMs need a subscription and require two-step authentication (e.g., verification code from email and short message service) or risk-based authentication. As a result, the complete testing process requires nearly 16 hours (both human and machine effort) to complete. Thus, we provide testing samples for three PMs with pre-registered accounts, imported data, and detailed instructions. These three PMs generally do not require two-step authentication and can be tested with only the passwords of their PMs and completed for around 40~50 minutes.

> Safari does not support loading user profiles with pre-entered sample data or allowing subsequent manual interactions for testing purposes. Due to this limitation, experiments involving Safari are conducted manually on macOS Monterey 12.7.4. [[Link1]](https://stackoverflow.com/questions/62246240/disable-automation-warning-in-safari-when-using-selenium) [[Link2]](https://github.com/SeleniumHQ/selenium/issues/6198)

> Testers may need to change the path in the `config\common_config.py` as we have only provided the path format on Windows.

### Software dependencies

```
- Chrome browser extensions of password managers with our tested versions (available in [Zenodo](https://zenodo.org/records/13380735)).
- python3 (3.11)
	- selenium==4.18.1
	- pywin32==306
    - loguru==0.7.2
    - psutil==5.9.8
- Chrome browser (latest version) and Chromedriver with compatible versions
- Firefox browser (latest version) and Geckodriver with compatible versions
- Edge browser (latest version) and Msedgedriver with compatible versions
- Opera browser (latest version) and Chromedriver with compatible versions
- Brave browser (latest version) and Chromedriver with compatible versions
```

To reduce the workload of AE reviewers, we have packed all the required environment and software dependencies into the [`Virtual Machine image.`](https://zenodo.org/records/13380735/files/leakyautofill.ova) At least a Windows 11 system with [`Virtualbox`](https://www.virtualbox.org/wiki/Downloads) software is required.

If AE reviewers prefer running our tool on their machine, python3 (with the required dependencies listed above) and Chrome browser are at least needed for completing our provided three samples. Moreover, we have uploaded the `requirements.txt` file in the [`Code\`] directory, which lists the Python dependencies and their exact versions. AE reviews could install all Python dependencies by running the command `pip install -r requirements.txt` under the [`Code\`] directory.

> Notes: Due to the need for manual interaction, we can only use `headful` Selenium. The Selenium instances are also set to a resolution of 1920 * 1080. To prevent layout changes in web forms that could cause test failures, we need to ensure that the screen can accommodate this resolution and that the screen's resolution (DPR) is ideally 1:1.

### Network Requirements 

We have deployed our tested websites on [GitHub](https://leakyautofill.github.io). Thus, our testing requires testers to be able to access `github.io`. Besides, testers should maintain a stable network connection to interact with PMs to complete the testing.

## Artifact Evaluation / Running the tools

### Installation: Import VirtualBox image

Download the packed [`Virtual Machine image`](https://zenodo.org/records/13380735/files/leakyautofill.ova), then import the image into the `VirtualBox` [[Reference Link]](https://docs.oracle.com/cd/E26217_01/E26796/html/qs-import-vm.html), and start the virtual machine. This process may take 5~8 minutes.

Testers are recommended to wait for some time (~5 minutes) for the virtual machine to work correctly.

Enter the `Artifact\` directory on the `Desktop`.

```cmd
cd C:\Users\artifact\Desktop\Artifact\
```

We provide testing samples for three PMs in our Virtual Machine image. Testers could create their password account, configure the password manager's properties, and conduct the same pipelines. The results are presented in Table 2 and Table 3 in our paper.

### (E1) Chrome browser-based Password Manager [time required 12 minutes]

This experiment examines whether Chrome browser-based password manager will autofill sensitive data into hidden sensitive fields in web forms. In this case, the concealment techniques directly apply to the `<input>` elements. We have added some tested data in the Chrome browser, including identity information, credentials of the tested account, and credit card. We start Selenium with this profile to facilitate the testing. 

However, in this case, Selenium opens the webpage and initially focuses on the address bar, which hinders the activation of the password manager [[Reference Link]](https://issues.chromium.org/issues/42320236). Therefore, testers need to click one blank part on the webpage after landing on the [home page](https://leakyautofill.github.io), effectively removing the highlights in the address bar. This action allows for observing the autofill process, mainly checking if the information is auto-filled in the first form. At this juncture, we recommend moving the mouse cursor outside the virtual machine, enabling our tool to function as expected.

We need to run the following scripts, with the manual process repeated for each iteration, taking approximately three minutes per run. Specific steps are as follows:

Under the above directories: `C:\Users\artifact\Desktop\Artifact\,` open `windows command line,` and run the following instructions, respectively.

**[Read before running the following scripts]**

When running each command, testers need to click the webpage to make Selenium's focus back to the webpage (not the address bar). We suggest the testers move the mouse cursor outside the browser or virtual machine. After around three minutes for each test, we could obtain the results in `[Artifact\Code\results\chrome\]`, corresponding to the results in Table 2 (row Chrome). The expected result is that the produced results are the same as the results in Table 2.

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./chrome_login.ps1"
```

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./chrome_pii.ps1"
```

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./chrome_cvv.ps1"
```

Here is a video (in `Video\Chrome\Human Operation for Chrome.mp4`) for the above human operation (about 10 seconds). The videos for all Chrome tests are shown in [`Video\Chrome\.`]

Here is an example of the results for Chrome and the personal information form. 0 means not filled, corresponding to a green cross, and 1 corresponds to a red checkmark.

```
original,1
display-none,0
visibility-hidden,0
visibility-collapse,0
opacity-0,1
cover-overlay,1
non-effective-size,1
off-screen-placement,1
ancestor-overflow,1
hidden-property,0
clip-property,1
clip-path-property,1
scale-property,1
font-size-zero,1
content-visibility-property,1
tiny-size,1 
```

### (E2) Enpass Password Manager [time required 15 minutes]

This experiment examines whether the Enpass password manager will autofill sensitive data into hidden sensitive fields in web forms. In this case, the concealment techniques directly apply to the `<input>` elements. We have installed the Enpass desktop application in the virtual machine, registered a test account, and stored login credentials for tested websites and credit card information (using data in `Sample Data`). Generally, we recommend the testers first unlock the desktop application using credentials stored in `[PM Sample Data\Enpass.json]`. Then, testers could run the following script.

We need to run the following scripts, with the manual process repeated for each iteration, taking approximately seven minutes per run. Specific steps are as follows:

Under the above directories: `C:\Users\artifact\Desktop\Artifact\,` open `windows command line,` and run the following instructions, respectively.

**[Read before running the following scripts]**

When running each command, testers need first to click the extension icon in the address bar (following the instructions in the pop-up webpage). Then, they could unlock the Enpass extension using the stored credentials in `[PM Sample Data\Enpass.json]` (if locked) and link the browser extension and the desktop application using a six-digit verification code shown on the webpage. As configured in [`Code\Config\PMConfig.py`], we have 20 seconds to do the above operations to ensure the Enpass extension is unlocked and ready to work.

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./enpass_login.ps1"
```

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./enpass_cvv.ps1"
```

After around *seven* minutes for each test, we could obtain the results in `[Artifact\Code\results\enpass\],` corresponding to the results in Table 2 (row Enpass). The expected result is that the produced results are the same as the results in Table 2.

Here is a video (in `Video\Enpass\Human Operation for Enpass.mp4`) for the above human operation (about 40 seconds). The videos for all Enpass tests are shown in `Video\Enpass\.`

### (E3) Norton Password Manager [time required 15 minutes]

This experiment examines whether Norton password manager will autofill sensitive data into hidden sensitive fields in web forms. In this case, the concealment techniques apply to **the ancestor elements of `<input>` elements**. We have registered a test account and stored login credentials for tested websites and credit card information (using data in `Sample Data`). The registered credential is stored in `[PM Sample Data\Norton.json]`, including the account name, account password, and vault key for unlocking the password vault. Then, testers could run the following script.

We need to run the following scripts, with the manual process repeated for each iteration, taking approximately seven minutes per run. Specific steps are as follows:

Under the above directories: `C:\Users\artifact\Desktop\Artifact\,` open `windows command line,` and run the following instructions, respectively.

**[Read before running the following scripts]**

When running each command, testers need to log in to the Norton account using the stored credentials in `[PM Sample Data\Norton.json]`. There are two steps to unlock the Norton extension.

1. Click the Norton Icon in the browser address bar if no pop-up windows appear. Input the `account name` and `password` into the pop-up form, and submit the login form. 
2. After the verification, the PM is expected to show a webpage for users to unlock the password vault. Testers should input the `vault_key` and continue to complete the setup to unlock the Norton extension. 

As configured in [`Code\Config\PMConfig.py`], we have 80 seconds to do the above operations to ensure the Norton extension is unlocked and ready to work.

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./norton_login.ps1"
```

```cmd
powershell.exe -ExecutionPolicy Bypass -File "./norton_cvv.ps1"
```

After around *seven* minutes for each test, we could obtain the results in `[Artifact\Code\results\norton\],` corresponding to the results in Table 3 (row Norton). The expected result is that the produced results are the same as the results in Table 3.

Here is a video (in `Video\Norton\Human Operation for Norton.mp4`) for the above human operation (about 100 seconds).
The videos for all Norton tests are shown in [`Video\Norton\`].

> The manual operations that may be involved with other PMs are listed in the `DETAIL_PM_README.md` file.

## Notes

Our Selenium tool uses `headful` mode and needs to access the website, which is sensitive to network fluctuations and unexpected user interactions that could potentially lead to test failures. For instance, the autofill feature might fail to fill any information. In the event of such an occurrence, we strongly recommend the following steps:

1. **Check Network Stability:** Ensure the network connection is stable throughout the testing process to avoid any disruptions that could affect the test outcomes.

2. **Review User Interactions:** Be mindful of any unexpected user interactions that might interfere with the test execution.

3. **Retry the Test:** If a failure is detected, it is advisable to rerun the program after addressing any potential issues identified in the previous steps.

## Directory Structure in the VirtualBox image

```
>>> Desktop/Artifact/ 
├── Code/                                   // Our main source code
├──├── config/                              // Config used in our testing
├──├── utils/                               // Util package
├──├── logs/                                // Logs during testing
├──├── results/                             // Generated results for each test
├──├── screenshots/                         // Screenshots during testing
├──├── autopm.py                            // Main logic of our semi-automated tool
├── Driver/                                 // Storing webdrivers for Selenium
├── Video/                                  // Sample videos for Chrome, Enpass, and Norton
├── Extension/                              // Storing extensions of password managers used in our experiments
├── Sample Data/                            // Sample data for login, personal information form, and credit card form using fake data
├── PM Sample Data/                         // Available credentials for Enpass password manager and Norton password manager
├── chrome_login.ps1			    // Powershell scripts for testing chrome in login forms
├── chrome_pii.ps1			    // Powershell scripts for testing chrome in personal information forms
├── chrome_cvv.ps1			    // Powershell scripts for testing chrome in credit card forms
├── norton_login.ps1			    // Powershell scripts for testing norton in login forms
├── norton_cvv.ps1			    // Powershell scripts for testing norton in credit card forms
├── enpass_login.ps1			    // Powershell scripts for testing enpass in login forms
├── enpass_cvv.ps1			    // Powershell scripts for testing enpass in credit card forms
├── README.md                               // README.md
```
