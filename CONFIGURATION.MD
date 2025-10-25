#### This guide is for Retoswap network only
***********************************************

1. There is a opened PR by atsamd21 https://github.com/retoaccess1/haveno-reto/pull/110 which you need to merge.

2. Then you need to fork the haveno android app https://github.com/atsamd21/Haveno-app
Then open the Manta.csproj file in a text editor to set it up for Retoswap network.


    | Property              | Value                                 
    |-----------------------|---------------------------------------
    | ApplicationTitle      | App name shown to the user: RetoSwap
    | ApplicationId         | Change this to com.retoswap, this is the app id that Android uses and not shown to the user
    | DaemonUrl             | Url of where the daemon.jar files are hosted. If you're using GitHub actions, use the url of the release that was created
    | Network               | Change this to XMR_MAINNET
    | HavenoAppName         | Change this to haveno-reto-mobile, this works the same as the app data directory on desktop
    | MauiIcon              | This is the app icon, you will need to update the SVG icon in Resources/AppIcon, make sure it is named appicon.svg
    | MauiSplashScreen      | This is the splash screen image shown when the app starts, update the SVG in Resources/Splash

***********************************************

![alt text](https://github.com/atsamd21/Haveno-app/blob/master/AppImages/CHANGETHESE1.png "Change these 1")

***********************************************

![alt text](https://github.com/atsamd21/Haveno-app/blob/master/AppImages/CHANGETHESE2.png "Change these 2")


3. Open the AppConstants.cs file in a text editor and change the ApplicationId string so that it matches the one in Manta.csproj

4. TODO - remote Haveno node

#### Updating the app

You will need to create a new release/url if updating the daemon since the app will compare the last installed url with the one set in Manta.csproj. However, you cannot update just because Haveno updates, there may be breaking changes and so the mobile app will need to be updated to support these changes first.

When the code for the app has been updated to support the latest Haveno version you can build the daemon again and create a new release. Update the url value for DaemonUrl in Manta.csproj. Then the app can be built. Once installed on the device it will automatically fetch the new daemon and update.
The app knows where to get this daemon by checking a URL stored in a configuration file called Manta.csproj.

So:


🧩 1. The “Daemon”

The app looks at the last installed daemon URL.

It compares it to the one defined in Manta.csproj.

If it’s different, it’ll try to fetch and install the new daemon automatically.


⚠️ 2. You Can’t Just Update When Haveno Updates

You can’t just rebuild or update the daemon every time Haveno releases a new version.
Why? Because Haveno updates might include breaking API changes — meaning the mobile app might no longer work with the updated daemon until the app’s code itself is updated to handle those changes.

So you must wait until:

The app’s code is updated to support the new Haveno version.

Then you can rebuild and release a new daemon.


🔨 3. When the App Supports the New Version

Once Manta’s code is updated to match the latest Haveno changes:

Rebuild the daemon.

Create a new release (hosted somewhere, e.g., GitHub or a web server).

Update the DaemonUrl value inside Manta.csproj to point to the new release URL.

Rebuild the mobile app.


🔄 4. App Auto-Update Mechanism

When a user installs or updates the app:

It checks the DaemonUrl.

If it’s new, it will automatically download and update the daemon on the device.

So that URL acts like a “trigger” for the app to update its backend.


| Step | Action                                        | Purpose                                     |
| ---- | --------------------------------------------- | ------------------------------------------- |
| 1    | Don’t update daemon blindly                   | Haveno updates can break compatibility      |
| 2    | Update app code to support new Haveno version | Avoid crashes or API mismatches             |
| 3    | Build new daemon & release                    | Publish new backend                         |
| 4    | Update `DaemonUrl` in `Manta.csproj`          | Point app to new daemon                     |
| 5    | Rebuild app                                   | Distribute new version; auto-fetches daemon |

