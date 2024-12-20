### Explanation of Why the App Didn’t Update After Running eas update --auto

Take a look at the first image:
![image.png](https://s2.loli.net/2024/12/20/Ue8YBPr4I5thWFd.png)

When you ran `eas update --auto` on the `staging branch` of  Git repository, the `--auto` flag automatically sets the branch to the current `Git branch`, which in this case is staging. However, we currently do not have a channel linked to the staging branch:
![image.png](https://s2.loli.net/2024/12/20/gqxoIR82zA4kvbG.png)

At the moment, the branch linked to the `testflight channel` is testflight, as seen in the second image. This connection between channel and branch is automatically created by EAS based on the channel specified in `eas.json`. When you run `eas update --auto`, it’s effectively the same as running `eas update --branch staging`. However, the staging channel is only `a base channel`, and the` testflight` and `googleinternal` channels inherit from it. For app builds, we are using the following command:

`eas build --platform ios --auto-submit --non-interactive --no-wait --profile testflight`

If you feel that the current connection between branches and channels is not logical, you can modify it using the `eas channel:edit` command.

### Key Points About eas update

Here are some key things to keep in mind when working with eas update:
	1.	EAS Update is for JS-Only Changes:
EAS Update can only be used for pure JavaScript/TypeScript changes. If you’ve made changes to app.config.ts, do not use eas update.
	2.	Changes to package.json:
If you’ve made changes to package.json (such as adding, removing, or updating dependencies), do not use eas update.
	3.	Changes to Native Configuration:
Any modifications to native configurations (e.g., Android build.gradle or iOS Info.plist) require a new build. Do not use eas update.
	4.	Changes to Native Code or Libraries:
If any native code or libraries (e.g., custom native modules) have been modified, do not use eas update.
	5.	Ensure versions.json Reflects the App Version:
After making any of the changes above, you’ll need to increment the` appVersion in versions.json`. This is because the `EAS Update strategy is tied to appVersion`. When you run `eas update`, it only updates the JS bundle for the version of the app matching the appVersion specified in `app.config.ts`.
