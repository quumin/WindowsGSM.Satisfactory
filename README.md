
# Satisfactory Dedicated Server Plugin for WindowsGSM

This repository contains the updated plugin for running the **Satisfactory Dedicated Server** via **WindowsGSM**. The original plugin was created by [werewolf2150 and caindrac](https://github.com/werewolf2150/WindowsGSM.Satisfactory), and this updated version includes several fixes and improvements to address issues with the server freezing and improve compatibility with WindowsGSM.

## Original Plugin

The original plugin, developed by **werewolf2150** and **caindrac**, provided support for running the **Satisfactory Dedicated Server** in early access mode. While it worked for earlier versions of Satisfactory, recent updates in WindowsGSM and changes in the game server required adjustments to the plugin code.

Original repository: [https://github.com/werewolf2150/WindowsGSM.Satisfactory](https://github.com/werewolf2150/WindowsGSM.Satisfactory)

## Updates and Improvements

In this updated version of the plugin, several important changes have been made to address issues where WindowsGSM would freeze when starting the Satisfactory server. Below are the key changes and improvements made:

### 1. **Process Window Handling**
   - **Old Version**: The server window started with `ProcessWindowStyle.Normal`, which could cause the WindowsGSM UI to become unresponsive.
   - **New Version**: The window is now started with `ProcessWindowStyle.Minimized` to prevent UI freezes. This keeps the server running smoothly without affecting WindowsGSM.

   ```csharp
   WindowStyle = ProcessWindowStyle.Minimized, // Minimized to avoid hanging UI
   ```

### 2. **Standard Output and Error Redirection**
   - **Old Version**: Output was redirected if `AllowsEmbedConsole` was enabled, but the handling could cause issues when the process was not fully attached to the console.
   - **New Version**: Added `RedirectStandardInput`, `RedirectStandardOutput`, and `RedirectStandardError` when `AllowsEmbedConsole` is enabled to ensure proper handling of console output without freezing.

   ```csharp
   RedirectStandardInput = true,   // Redirect output if needed
   RedirectStandardOutput = true,  // Redirect output if needed
   RedirectStandardError = true    // Redirect output if needed
   ```

### 3. **Embed Console Handling**
   - **Old Version**: The plugin redirected console output but had incomplete error handling during startup.
   - **New Version**: The updated version ensures that console output is fully handled and startup exceptions are properly caught and logged, preventing the plugin from freezing during initialization.

   ```csharp
   try
   {
       p.Start();
       p.BeginOutputReadLine();
       p.BeginErrorReadLine();
   }
   catch (Exception e)
   {
       Error = e.Message;
       return null; // return null if fail to start
   }
   ```

### 4. **Graceful Shutdown**
   - **Old Version**: The server shutdown mechanism was functional but could be improved with better process timing and control.
   - **New Version**: Added a delay during shutdown to allow the process to close properly. This prevents the server from being forcibly killed prematurely.

   ```csharp
   await Task.Delay(20000); // Give time to shut down properly
   ```

### 5. **Credits to the Original Authors**
   - This updated version builds on the excellent work done by **werewolf2150** and **caindrac**, and credit is given for their original contributions to the plugin.

---

## Full List of Changes

1. Changed the window style from `Normal` to `Minimized` to prevent WindowsGSM from freezing.
2. Added output redirection for standard input, output, and error streams when embedding the console.
3. Added better error handling during the startup process.
4. Implemented a 20-second delay during shutdown to ensure proper server closure.
5. Cleaned up the code to align with best practices in WindowsGSM plugin development.

## How to Use

1. Clone this repository or download the updated `Satisfactory.cs` file.
2. Replace the old `Satisfactory.cs` file in your WindowsGSM plugins folder.
3. Restart WindowsGSM and configure the server as usual.

## Credits

- **Original Authors**: [werewolf2150](https://github.com/werewolf2150) and **caindrac**
- **Updated By**: AimiSayo

---

By following these improvements, the new plugin ensures that the **Satisfactory Dedicated Server** runs smoothly within **WindowsGSM** without causing the UI to hang or the server to freeze during operation.
