# Build 2024 - GitHub Advanced Security Hands-on Lab
GitHub Advanced Security (GHAS) is a developer-first application security testing solution that brings GitHub's world-class security capabilities to public and private repositories. Most of GitHub Advanced Security features are free for public repositories but require a GitHub Advanced Security license for private repositories. It only takes a few clicks to get started. Right out of the box, you'll benefit from highly curated detection and remediation capabilities crafted by some of the world's best security engineers to ensure your code and software supply chain are as secure as possible. It's fully automated, so once enabled, you don't have to remember to run GHAS tests or wait for a security review before merging.

In this lab, you will use use GHAS as a single developer taking advantage of these professional-grade tools that are available free to anyone using GitHub.com when working on their code in public. In addition, you will use GitHub Codespaces as your development IDE in the cloud so you don't need anything but a modern web-browser.

## Prerequisites
In order to complete this lab, you only need a valid GitHub.com account and a modern web browser (Chromium-based browsers like Microsoft Edge or Google Chrome are [preferred for accessing GitHub Codespaces](https://docs.github.com/en/codespaces/troubleshooting/troubleshooting-github-codespaces-clients#troubleshooting-the-visual-studio-code-web-client)).

## Exercise 1
To get started, you will get the sample code and "[fork it](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)" into a new repo in your personal GitHub.com account.

1. Navigate to [https://github.com](https://github.com) and log in to your personal account.
   
2. Once you've authenticated, navigate to [https://gh.io/build-2024-ghas-lab](https://gh.io/build-2024-ghas-lab).

3. In the upper right of the repo, click the **Fork** button.   

4. On the **Create a new fork** page, verify that you're forking into your *personal account*--you'll be the Owner.
   
5. Remove the check from **Copy the **`main`** branch only**.
   
6. Accept the rest of the defaults, and click **Create fork**.

You now have a copy of the sample code and can continue to configuring GitHub Advanced Security (GHAS).

## Exercise 2
Now, you'll configure GHAS in your newly forked repo.

1. In your forked copy of the sample repo, click the **Settings** button.

2. On the left-hand menu, select **Code security and analysis** under the *Security* section.
   
   > You're going to start with *Dependabot*.

3. First, you need to enable **Dependency graph** by clicking the **Enable** button.
   
    > As you click the **Enable** button you'll see a Repository settings saved message at the top of the Settings page.

4. Click **Enable** next to the three following items:
   1. Dependabot alerts
   2. Dependabot security updates
   3. Dependabot version updates
       
5. Click the **<> Code** button the repo button.

With just a few button clicks, you've turned on some GHAS goodness. Move forward to the next exercise to see the results.

## Exercise 3
You'll examine the results of enabling Dependabot and try a few quick things to make your code more secure using GitHub Codespaces.

1. On **your** repo's home page, click the green **<> Code** button.

2. Select the **Codespaces** tab.

3. Click **Create codespace on main**.

4. Wait for your codespace to start. This could take a minute or two.

   > Examine the user experience (Visual Studio Code) **without clicking on the interface**. In the Terminal window, in the middle of the screen, you'll see messages as your codespace finishes spinning up.

5. Examine the *Get Started* tab and/or README tab if they open, and close them when you're ready.

6. Once the codespace is ready, you'll be able to issue commands at the terminal prompt. The terminal prompt will read like **/workspaces/your-repo-name (main)**.

7. At the Terminal prompt, type **cd src/** and press return.

8. Next type **dotnet build** and press return. This should build the application successfully. Did you notice anything interesting?

9.  At the Terminal prompt, type **cd ReadingTime6.Web.Tests.Unit/** and press return.

10. Type **dotnet test** and press return.

11. You should see **13** tests pass.

13. In the Terminal, and type the following command to add a NuGet package with a known vulnerability (this is one long line of code): 
   
    ``` bash
    dotnet add package Swashbuckle.AspNetCore.SwaggerUI --version 6.2.3
    ```

14. This should add a new line to the `ReadingTime6.Web.Tests.Unit.csproj` file with for the NuGet package.

15. On the *Activity Bar*, click on the **Source Control**. 

16. Create a commit message.

17. Commit your changes and then push back to GitHub.com.

18. Return to your repo and leave your codespace running.

19. Click the **Security** tab which should have a number **1** next to it.

20. Click the **Dependabot** link. You'll see there's an alert related to the new package you just added.

21. Click the **Server side request forgery in SwaggerUI** alert and examine the details.
    
You'll leave this as it is for a bit. Dependabot will suggest a fix and you'll come back to that; for now keep going.

## Exercise 4
You're now going to try out [Push Protection](https://docs.github.com/en/enterprise-cloud@latest/code-security/secret-scanning/push-protection-for-repositories-and-organizations). This feature is turned-on *automatically* for public repos on GitHub.com.

1. Create a simple [GitHub Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) so that you have a secret to that you'll try and "push".
    1.  Click your **profile picture dropdown** in the upper right hand corner.

    2.  Click **Settings**.

    3.  Scroll all the way down on the left hand-side menu and navigate to **<> Developer Settings**.

    4.  Click **Personal Access Tokens** and then choose **Tokens (classic)**.

    5.  Click **Generate new token** and then **Generate new token (classic)**.

    6.  Give the token a name in the **note** field.

    7.  Change the expiration to **7 days** and select the **scope** of the PAT something relatively harmless, like `repo:invite`. 

    8.  Click **Generate Token**.

2. Copy the value of the PAT and save it to a text file.

3. When ready, return to your codespace.

4. Perform a **pull** to ensure you have the latest code.

5. Access the Explorer in your codespace and open the **Startup.cs** file in the **src/ReadingTime6.Web** folder.

   1. Add the following code:
   
   ``` cs
   var pat = "{put your pat here}";
   ```

   2. ... as the last line in the constructor  ...
   
   ``` cs
   public Startup(IConfiguration configuration)
   {
        Configuration = configuration;
   }
   ```

6. Save and commit your change in the codespace.

7. Now try and push.
   
   > It should fail with a warning dialog.

8. Click **Open Git Log**.

9. In the log window, find the URL for *learning more*. Follow the URL and examine doc page and have a quick review. When ready, return to your codespace.
    
    > **ALERT**: for the next few steps, pay close attention and read thoroughly for the best lab experience.

    Back in the output, you'll see a message (see below) with a URL you can click on:

    ```
    (?) To push, remove secret from commit(s) or follow this URL to allow the secret.   
    ```

10. Click the URL and at the details. You will see there's options that you can choose to "resolve" the issue. 
    
    > **Do NOT choose any of the options**.

11. Instead, back in your codespace's Terminal, enter the following:
    
    ``` bash
    git reset HEAD~1
    ```

12. Now undo the local change. 
    
    > You've now cleaned things up. If you want to be really tidy, go delete your PAT.

13. Return to your repo and leave your codespace running.

14. In the repo under the **Pull Requests** tab, you should see one *Dependabot Pull Request*. As was mentioned earlier, Dependabot found you had a vulnerable library and has suggested a fix.

15. Merge the pull request. Normally, you'd run your extensive unit and integrations tests suites, but for this lab you'll just put your faith in Dependabot.

16. Now, access the Security tab and then Dependabot and notice you have no open issues.

At this point you've used three great features of GHAS: Dependabot, Secret Scanning, and Push Protection. Move on to the next exercise to use some more features!

## Exercise 5
In this exercise, you'll enable CodeQL on your repository to perform code analysis.

1.	From your repo, select the **Settings** tab

2.	On the menu on the left, select **Code security and analysis**

3. For the **Code scanning** option, click the **Set up** button.

4. Choose the **Default** option.

5. In the dialog that appears, review the settings and choose **Enable CodeQL**.

    > It will take a couple of minutes for GitHub to setup CodeQL analysis. This is done via a GitHub Actions workflow.

6.  Click the **Actions** tab. You should see a **CodeQL Setup** workflow running.

7.  You can click the running workflow to see the status of the job and review the details.

You can now move on to checking the results.

## Exercise 6
In this exercise, you'll examine the code scan results.

1. Click the **Security** button on the toolbar.

2. On the menu under *Overview*, you will see a **Code scanning** item, click it. There will be a list of results, with the most critical alerts at the top.

    > You can filter these results by Package, Ecosystem, Manifest, or Severity, and sort by priority, date, severity, and more. You can create different views and bookmark the URLs to come back to them.

3. You should see that CodeQL found **eight** vulnerabilities. There will be one for C# (added on purpose for the lab), and the rest are JavaScript related. You'll note that four of the eight are of a *high* severity.
   
4. Examine the various errors.
   
   > The C# remediation will be the easiest because it's in 'our' code. The other ones make the remediation a bit more complex because we need updates from the library providers.
   
You just ran a CodeQL scan using the standard queries which identified vulnerabilities in the app's code as well as libraries used by it.

## Exercise 7
In this last exercise, you'll fix as many errors as possible that were detected by CodeQL.

1. Click on the **<> Code** button.

2. Click the **Switch branches** drop-down and select the **fix/ghas-found-issues** branch.

3. If you click on the **Commits** link you'll see there three commits that appear to fix the security issues.

4. Click on the **Pull Requests** button.

5. Click the green **New pull request** button.
   
    > Because you're using a forked repo, the default PR process is to merge back to the origin repo. You're not going to do that. You're going to create a PR using the provided local fix branch.

6. Change the *base repository* to **{your handle}/build2024ghas**. This will change the UI a bit.

7. Change the *compare* branch to **fix/ghas-found-issues**

8. Click the green **Create pull request** button.

9. Provide a description and click the green **Create pull request** button.
    
    > You'll see that there should be no conflicts, however, CodeQL is checking the PR to see if it introduces any issues.

10. Wait for the CodeQL checks to finish. You should see that no new alerts were introduced.

11. Merge the pull request.

12. Click the **Actions** tab. You should see a **CodeQL Setup** workflow running.

13. You can click the running workflow to see the status of the job and review the details. This ran automatically due to the CodeQL configuration you did earlier. Wait for the job to finish.

14. Click the **Security** button on the toolbar.

15. Review the what issues are left. You and GitHub need to work with the OSS community help report and if possible fix errors as they're found.

GitHub Advanced Security will surface existing vulnerabilities and flag potential new problems before they're merged into your code. Keep your applications secure is a journey, not a destination. Tools like GHAS are an enormous help. But you are the key contributor to your team's and organization's success in building secure solutions.

Thank you for taking the time to run through this lab.
