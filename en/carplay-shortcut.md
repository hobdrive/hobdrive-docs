# Automatically Launch hobDrive on CarPlay Connection Using iOS Shortcuts

This guide will help you set up automatic launching of the hobDrive app when you connect your iPhone to CarPlay using the Apple Shortcuts app.

## Table of Contents
{:.no_toc}
* TOC
{:toc}

## Overview

The iOS Shortcuts app allows you to create automation that triggers when specific events occur on your device. By setting up a CarPlay connection automation, you can ensure that hobDrive automatically starts whenever you connect to your car's CarPlay system, making your driving experience seamless and convenient.

### Important Notes About iOS Behavior

- **Phone must be unlocked**: iOS starts the application only if your phone is unlocked
- **CarPlay and locked phone**: iOS may start the application if CarPlay is enabled, even with a locked phone
- **Bluetooth/ELM connection**: Connection to Bluetooth/ELM adapter starts only if the phone screen is active

These limitations are part of iOS security and privacy features. For the best experience, ensure your phone is unlocked when connecting to CarPlay.

### Requirements

- iPhone running iOS 13.1 or later
- CarPlay-compatible vehicle or aftermarket CarPlay unit
- hobDrive app installed on your iPhone
- Apple Shortcuts app (pre-installed on iOS 13+)

## Setting Up the Automation

Follow these step-by-step instructions to create the automation:

### Step 1: Open the Shortcuts App

Locate and tap the **Shortcuts** app on your iPhone home screen. The icon looks like two overlapping squares with rounded corners.

If you can't find the Shortcuts app, you can download it from the App Store (it's free and made by Apple).

### Step 2: Navigate to Automation

1. Tap on the **Automation** tab at the bottom of the screen
2. If this is your first automation, you'll see a screen explaining what automations can do
3. Tap the **Create Personal Automation** button (or the **+** button if you already have other automations)

### Step 3: Select CarPlay Trigger

Scroll through the list of automation triggers and select **CarPlay**.

This trigger will activate whenever your iPhone connects to or disconnects from a CarPlay system.

### Step 4: Configure When to Run

You'll see two options:

- **When Connecting** - Run the automation when CarPlay connects
- **When Disconnecting** - Run the automation when CarPlay disconnects

Select **When Connecting** by tapping it. A checkmark will appear next to your selection.

Tap **Next** in the top right corner to continue.

### Step 5: Add the Open App Action

Now you need to tell the automation what to do when CarPlay connects.

1. Tap **Add Action** or use the search field
2. In the search field, type "Open App"
3. Select the **Open App** action from the results

### Step 6: Select hobDrive App

1. Tap on the word **App** in the action block (it will appear as a blue link)
2. Scroll through your installed apps or use the search field to find **hobDrive**
3. Tap **hobDrive** to select it

The action block should now read "Open hobDrive".

### Step 7: Disable Ask Before Running

By default, iOS will ask for your confirmation before running the automation. To make it truly automatic:

1. Tap **Next** in the top right corner
2. You'll see a summary screen with a toggle for **Ask Before Running**
3. Turn **OFF** the "Ask Before Running" toggle

A dialog will appear warning you that the automation will run without your confirmation. Tap **Don't Ask** to confirm.

### Step 8: Complete the Setup

Tap **Done** in the top right corner to save your automation.

Your automation is now active and will automatically launch hobDrive whenever you connect to CarPlay!

## Testing the Automation

To verify that your automation works correctly:

1. Make sure the hobDrive app is installed and configured
2. If already connected to CarPlay, disconnect your iPhone
3. Close the hobDrive app if it's currently running
4. Connect your iPhone to CarPlay
5. The hobDrive app should automatically launch within a few seconds

## Troubleshooting

### The automation doesn't run

- **Check the automation is enabled**: Open Shortcuts > Automation tab, and verify that the toggle next to your CarPlay automation is ON (green)
- **Verify "Ask Before Running" is disabled**: Edit the automation and make sure this toggle is OFF
- **Check iOS version**: Ensure you're running iOS 13.1 or later
- **Restart your iPhone**: Sometimes a restart helps resolve automation issues

### hobDrive doesn't open

- **Verify app name**: Make sure you selected the correct hobDrive app in the action
- **Check app is installed**: Ensure hobDrive is properly installed and can be opened manually
- **Re-create the automation**: Delete the automation and create it again following the steps above

### The automation asks for confirmation

- **Disable "Ask Before Running"**: Edit the automation and turn OFF the "Ask Before Running" toggle
- **Confirm the warning**: When disabling, make sure to tap "Don't Ask" in the confirmation dialog

## Additional Tips

### Adding a Delay

If you experience issues with the app launching too quickly (before CarPlay is fully connected), you can add a short delay:

1. Edit your automation
2. Before the "Open App" action, tap the **+** button
3. Search for and add a "Wait" action
4. Set the wait time to 2-5 seconds
5. The automation will now wait before opening hobDrive

### Multiple Actions

You can add more actions to your automation, such as:

- Setting volume level
- Enabling Do Not Disturb while driving
- Starting a specific playlist
- Adjusting screen brightness

Simply tap the **+** button between actions and search for what you want to add.

### Editing or Deleting the Automation

To modify or remove the automation:

1. Open the Shortcuts app
2. Go to the **Automation** tab
3. Tap on your CarPlay automation
4. To edit: Make your changes and tap **Done**
5. To delete: Scroll down and tap **Delete Automation**

## Conclusion

Setting up this automation makes using hobDrive with CarPlay effortless. Once configured, you'll never need to manually launch the app when connecting to your car—it will be ready and waiting for you automatically.

For additional help with hobDrive features and settings, please refer to the [User Guide](user-manual.md) or contact [support@hobdrive.com](mailto:support@hobdrive.com).
