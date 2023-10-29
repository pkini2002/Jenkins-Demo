# Setting Up an Automation Test Using Jenkins and Integrating with VCS like Github

### Prerequisites to install Jenkins

- JDK 1.7 or 1.8
- 2GB Ram
- Set env Variable `JAVA_HOME` to point to JDK base directory Ex: `C:\Program Files\Java\jdk 1.8.0_162`

> You can use the cmd `java -version` or `javac -version` or `echo %JAVA_HOME%` to check your JDK version

### Jenkins Installation on Windows

- Go to the official website of Jenkins and download the relevant release by clicking the name of the platform (in our case you have to choose Windows):
  <a href="https://www.jenkins.io/download/"> Jenkins Download </a>

  <img width="476" alt="Step1"  src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/8bc9dfb3-48b8-4837-bd3e-4f2e69f0e032">

- The jenkins.msi file will be downloaded. Now go to the Downloads folder and double click the downloaded jenkins.msi file and execute it.
  
  <img width="476" alt="Step2" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/c97dec0e-519c-4a06-93d7-115a79b318cb">

- Click on the Next button in the setup wizard.
  
  <img width="476" alt="Step3" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/861acd4d-2445-4101-8027-19fde7c948b7">

- Choose the location where you want to install the Jenkins instance.
  
  <img width="476" alt="Step4" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/5dc94f44-026a-4106-acac-21e3ee759c71">

- Now you have to choose the service logon credentials. In our case, let us choose the first option “Run service as LocalSystem” and click on the Next button.

  <img width="476" alt="Step5" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/b7f6bcab-d5c6-4e35-be62-ec3636794aa2">

- Then you have to select a port to run the service. Better to go with the recommended default port number (8080) and click on the Test Port option. Once verified click on the Next button.

  <img width="476" alt="Step6" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/91cc6a01-20d3-444d-abe7-a9a9ee074d12">

- Next you have to choose the location where your jdk file is residing and click on the Next button.

  <img width="476" alt="Step7" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/29b89888-5cc0-4ae6-8990-3b850833f0c3">

- Then simply click on the Next button without changing anything in the custom setup wizard.

  <img width="476" alt="Step8" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/1d14ae22-a0b2-435e-94ff-ae926a79b8b1">

- Now click on the Install button and wait for the installation process to complete. It will finish within a few seconds.
- Once the installation process is completed, click on the Finish button and exit the setup wizard.

  <img width="476" alt="Step9" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/ff919710-bf4b-43b7-97f8-935e2715799b">


**Unlock Jenkins**

> After the installation now we can proceed to our Jenkins portal and start configuring it. Go to  ` http://localhost:8080`
> *Here I have given 8080 since I have configured that port number in the setup wizard when installing the Jenkins. You should give the relevant port number you gave during the installation.*
> If this leads to the Jenkins UI, then it means our installation is complete. You should see the below screen which will ask you to unlock Jenkins with the administrator password.

   <img width="476" alt="Step10" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/46868f40-050e-4720-928a-dd6a7c3194ca">

- You can find the password in a file named `“initialAdminPassword”` under the `“C:\Program Files (x86)\Jenkins\secrets”` folder. If you cannot find that particular folder (this happens only in some machines), go to `“C:\Program Files (x86)\Jenkins”` and find the `“jenkins.err”` file. There also you can find the initialAdminPassword.

- Open the file, copy the password and paste it in the pop up page “Unlock Jenkins” and click on the Continue button.

- Now you can start customizing the Jenkins by starting with the Plugin installation. You can select whether you want to install the suggested plugins or you want to select and install some plugins that you prefer.

  <img width="476" alt="Step11" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/af970b89-6e13-4353-be26-63ed645b3527">

- Then create the first Admin User account by providing the necessary details. These are the credentials which you will need to login to Jenkins portal in the future.

  <img width="476" alt="Step12" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/717e748c-8fff-40d4-bb62-736091434c4d">

- Then you will be prompted to instance configuration UI. Here it is always suggested to go with the recommended port configuration.

 <img width="476" alt="Step13" src="https://github.com/pkini2002/NMAMIT-CSE-Labs-2020-24/assets/84091455/5ee1c8b4-ecab-43d8-95da-ece64cf81457">

 ### Creating Automation Test

 Once you are in the dashboard of Jenkins 

 - Click on New Item
 - Enter the Item Name
 - Select FreeStyle Project / Pipeline
 - Once Done Click OK
 - Now the configuration page of your Jenkins instance opens
 - Scroll down to find Source code management
 - Tick Git and add the repo URL `https://github.com/pkini2002/Jenkins-Demo.git`
 - In the branches to build type `*/main`
 - Scroll down to find the build triggers
 - Tick the build periodically checkbox
 - Specify the duration after which you want to automatically build your code.
 - In my case I have specified it as 3 min (After every 3 min it will pull my code from repo, build the code and test it)
 - To do so, specify `*/3 * * * *` in the textbox provided (Click on the que mark for more info about cron jobs)
 - Scroll down and select `Exceute Windows Batch Command` in Build Steps
 - I have written a very simple line of command for the demonstration. Write the script which you want to get automated

  ```
    Javac Hello.java
    java Hello
  ```

*Execute windows batch command*

```
javac Hello.java

if %errorlevel% EQU 0 (
    java Hello
) else (
    echo Build unsuccessful due to compilation errors
    exit /b %errorlevel%
)
```

  - Click on Save
  - Then Click on Build now from the left pane
  - You will get a Success/Failure console output after the build is complete

  ```
      Started by timer
      Running as SYSTEM
      Building in workspace C:\ProgramData\Jenkins\.jenkins\workspace\JavaTesting
      The recommended git tool is: NONE
      No credentials specified
       > git.exe rev-parse --resolve-git-dir C:\ProgramData\Jenkins\.jenkins\workspace\JavaTesting\.git # timeout=10
      Fetching changes from the remote Git repository
       > git.exe config remote.origin.url https://github.com/pkini2002/Jenkins-Demo.git # timeout=10
      Fetching upstream changes from https://github.com/pkini2002/Jenkins-Demo.git
       > git.exe --version # timeout=10
       > git --version # 'git version 2.35.1.windows.2'
       > git.exe fetch --tags --force --progress -- https://github.com/pkini2002/Jenkins-Demo.git +refs/heads/*:refs/remotes/origin/* # timeout=10
       > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10
      Checking out Revision cc9c5f3d279b44f09a3f151e35024c567a3e82a9 (refs/remotes/origin/main)
       > git.exe config core.sparsecheckout # timeout=10
       > git.exe checkout -f cc9c5f3d279b44f09a3f151e35024c567a3e82a9 # timeout=10
      Commit message: "Update README.md"
       > git.exe rev-list --no-walk cc9c5f3d279b44f09a3f151e35024c567a3e82a9 # timeout=10
      [JavaTesting] $ cmd /c call C:\WINDOWS\TEMP\jenkins8228152341912714143.bat
      
      C:\ProgramData\Jenkins\.jenkins\workspace\JavaTesting>Javac Hello.java 
      
      C:\ProgramData\Jenkins\.jenkins\workspace\JavaTesting>java Hello 
      Hello World
      I am testing my code using Jenkins
      I am testing my code using Jenkins again
      
      C:\ProgramData\Jenkins\.jenkins\workspace\JavaTesting>exit 0 
      Finished: SUCCESS
  ```

  - You can make changes and commit the code any number of times and the pipeline will automatically pull the code every 3 min from the main branch build the code and will produce the console output with a success or failure message

  <img width="476"  alt="Screenshot 2023-10-29 130642" src="https://github.com/pkini2002/Jenkins-Demo/assets/84091455/db0ce5cd-0490-4b56-9a9c-004ba6f1e4a0">
