# Download RDCMan (Remote Desktop Connection Manager)

Updated: July 25, 2025       
**⬇️ [Download RDCMan](https://rdmtool.github.io/RDCMan)**

## Introduction

**Remote Desktop Connection Manager (RDCMan)** is designed to streamline the management of multiple remote desktop sessions. It is an essential tool for environments like server management, automated kiosk systems, and data centers, where consistent access to individual machines is required.

Servers are organized into categorized groups, allowing users to connect or disconnect from all servers in a group simultaneously. A thumbnail view offers live previews of active sessions. Login credentials can be inherited from parent groups or centrally managed, simplifying password management, as updates are needed only once. RDCMan ensures the secure handling of stored credentials through encryption (using either the local user’s credentials with `CryptProtectData` or an X509 certificate).

Users with operating systems older than Windows 7 or Vista must install version 6 of the Terminal Services Client, available from the Microsoft Download Center: XP; Win2003.

**Upgrade notice:** RDG files created in this version are not compatible with previous RDCMan versions. When opening legacy files and saving them, a backup copy with the `.old` extension is automatically generated.

## The Display

The RDCMan interface includes a menu bar, a hierarchical server group tree, a splitter, and the client display area.

### The Menu

RDCMan provides several main menus:

* **File** – for loading, saving, and closing RDCMan group files
* **Edit** – allows adding, deleting, and modifying server or group configurations
* **Session** – provides options to connect, disconnect, or log off from sessions
* **View** – controls the visibility of the server tree, virtual groups, and adjusts the client display area
* **Remote Desktops** – shows a hierarchical view of groups and servers, useful when the Server Tree is hidden
* **Tools** – used to configure application-wide settings
* **Help** – displays information about RDCMan (which you have likely already accessed)

### The Tree

Common actions such as adding, removing, and configuring servers or groups can be done by right-clicking on a tree node. Items can also be rearranged by dragging and dropping them.

**Keyboard shortcuts include:**

* **Enter**: Connect to the selected server
* **Shift+Enter**: Connect using the "Connect As" option
* **Delete**: Delete the selected server or group
* **Shift+Delete**: Delete the selected item without a confirmation prompt
* **Alt+Enter**: Open the properties window for the selected server or group
* **Tab**: When a server is selected, focus shifts to its display

To move the server tree to either side of the window, use the **\[View\.Server tree location]** menu.

The visibility of the server tree can be toggled between docked, auto-hide, or hidden completely using the **\[View\.Server tree visibility]** option. If hidden, servers are still accessible through the Remote Desktops menu. In auto-hide mode, a splitter remains visible on the left side—hovering over it will bring the tree back into view.

### The Client Area

The content displayed in the client area depends on the tree node you select. Choosing a server will show that server’s remote desktop session. Selecting a group will show thumbnail views of its servers. You can adjust the size of this area via the View menu or by resizing the RDCMan window. To prevent resizing from the window edges, use **\[View\.Lock window size]**.

**Note:** When navigating within the thumbnail view using the keyboard, focus may shift to an active session. Be careful, as it may not always be clear which server is focused. This can be managed with the setting: **\[Display Settings.Allow thumbnail session interaction]**.

### Full Screen Mode

To switch to full screen with a server, select it and press **Ctrl+Alt+Break** (this shortcut can be customized in the Shortcut Keys settings). To exit full screen mode, press the same keys again or use the minimize/restore controls in the connection's title bar. Multi-monitor support is available if monitor spanning is enabled.

### Shortcut Keys

A complete list of Terminal Services keyboard shortcuts is available through the interface. Many of these can be customized under the **Hot Keys** tab.

## Files

The core structure in RDCMan is the Remote Desktop file group. A file group contains servers and/or groups in a single physical file. Servers cannot exist outside a group, and groups must always be part of a file.

A file functions like a server group, but its parent cannot be changed.

## Groups

Groups consist of collections of servers and configuration parameters such as logon information. These settings can inherit values from other groups or from the application's default settings. Groups can be nested, but they are structured in a consistent manner: each group contains either servers or other groups, but not both at the same level. All servers within a group can be connected or disconnected simultaneously.

When you select a group in the tree, its servers are displayed as thumbnails. These thumbnails can either show the actual remote desktops or just represent status indicators. Global thumbnail settings are controlled in **\[Tools.Options.Client Area]**, while the display options for specific servers or groups are found in the Display Settings panel.

### Smart Groups

Smart groups are automatically populated based on predefined criteria. Any ancestor of a sibling group qualifies for inclusion in a smart group.

### The Connected Virtual Group

Once a server establishes a connection, it is automatically added to the Connected virtual group. Servers cannot be manually added or removed from this group.

You can control the visibility of the Connected group through the **View** menu.

### The Reconnect Virtual Group

In certain situations, a server may disconnect and remain offline for an extended period, such as during a reboot following an OS update. In these cases, the affected server can be moved to the Reconnect group. RDCMan will continuously attempt to reconnect until the connection is successfully re-established.

The Reconnect group can be shown or hidden via the **View** menu.

### The Favorites Virtual Group

The Favorites virtual group provides a flat list of servers you have marked as favorites. You can add any server from the tree view to this group. This feature is especially useful when you manage many servers but regularly interact with just a few across different groups.

You can toggle the visibility of the Favorites group using the **View** menu.

### The Connect To Virtual Group

This virtual group contains servers that have not been assigned to any manually created groups. For further details, refer to the **Ad Hoc Connections** section.

The Connect To group appears when there are active ad hoc connections and disappears when no such connections exist.

### The Recent Virtual Group

The Recent virtual group stores the servers that were most recently accessed.

Use the **View** menu to toggle the visibility of the Recent group.

## Servers

Each server is identified by its name (which may be a network name or IP address), with an optional display name and associated logon credentials. These credentials can be inherited from a parent group if needed.

### Adding Servers Manually

When server names follow a defined pattern, you can add multiple servers at once. RDCMan supports two types of patterns:

* **Iteration** – Use `{a,b,c}` to iterate through a list of comma-separated values.
* **Range** – Use `[1-5]` to iterate over a numeric range; include leading zeros to define the minimum number of digits.

**Examples:**

* `server1{a,b,c}`: Expands to `server1a`, `server1b`, `server1c`
* `server[001-15]`: Expands to `server001`, `server002`, ..., `server015`
* `{dca,dcb}rack[1-5]sql[1-2]`: Generates `dcarack1sql1`, `dcarack1sql2`, ..., `dcarack5sql2`, `dcbrack1sql1`, ..., `dcbrack5sql2`

### Importing Servers from a Text File

You can import servers into a group using a text file. The format is simple—each line contains a single server name:

```txt
Server1
SecondServer
YANS
```

Alternatively, server names can be manually entered into the dialog box.

All imported servers are added to the same group using the same configuration settings. If a server already exists by name, its settings will be updated with the new values.

### Ad Hoc Connections

Ad hoc server connections can be initiated through the **\[Session.Connect to]** option. These connections will appear in the Connect To virtual group. You can convert these temporary connections into permanent ones by moving them to a custom group. If left in the Connect To group, they will not be saved when RDCMan is closed.

### Windows Azure

Within the **\[Connection Settings]** tab, enter the role name and instance name into the *Load balance config* field as outlined [here](/en-us/azure/cloud-services/cloud-services-role-enable-remote-desktop-powershell). For example:

```
Cookie: mstshash=MyServiceWebRole#MyServiceWebRole_IN_0#Microsoft.WindowsAzure.Plugins.RemoteAccess.Rdp
```

### Session Actions

While connected to a session, you can shift your focus to another session or return to the server tree.

* **Focus release left** (default: **Ctrl+Alt+Left**) – Switches to the previously active session
* **Focus release right** (default: **Ctrl+Alt+Right**) – Opens a dialog allowing you to focus on the most recently used sessions, the server tree, or minimize RDCMan

Some keyboard shortcuts and system-level commands can be tricky to send to a remote session, especially if RDCMan is running inside another remote session (e.g., **Ctrl+Alt+Del**). These actions are accessible via the **\[Session.Send keys]** and **\[Session.Remote actions]** menus.

## Global Options

The **\[Tools.Options]** menu option opens the Options Dialog, where global configuration settings can be adjusted. General settings such as client area dimensions can be modified here. Be aware that many server-specific configurations, like hot keys or settings under the experience tab, will only take effect after the server is reconnected.

### General

**Hide main menu until ALT is pressed**
This option hides the main menu bar until either the ALT key is pressed or the window's title bar is clicked.

**Auto save interval**
RDCMan offers automatic file saving at set intervals. To enable this, activate auto-save and specify the frequency (in minutes). Setting the interval to 0 disables automatic saving and prevents the prompt to save upon exit.

**Prompt to reconnect previously connected servers at launch**
RDCMan tracks servers that were connected when the application was last closed. When reopening, it prompts you to reconnect to those servers. Disabling this option makes all previously connected servers reconnect automatically. For overrides, refer to the **Command Line** section.

**Default group settings**
Clicking this button opens a dialog to set default values at the root of the inheritance chain. These settings apply wherever a File group inherits from its parent container.

### Tree

**Click to select also sets focus to remote client**
By default, clicking a server node in the tree keeps the keyboard focus within the tree itself. Enabling this option shifts the focus to the associated remote session.

**Dim tree nodes when tree control is inactive**
When enabled, RDCMan dims the server tree when it is not in focus, making it easier to identify where keyboard input is currently directed.

### Client Area

**Client Area Size**
This setting controls the overall size of RDCMan's client display area. These settings can also be adjusted through the **\[View\.Client size]** menu.

**Thumbnail Unit Size**
You can set the size of each thumbnail by specifying either a fixed pixel value or a relative percentage of the total width of the client area.

### Hot Keys

You can reconfigure many hot key combinations used in remote desktop sessions, although there are certain limitations. For example, if a default hot key contains ALT, the custom one must also include ALT. To assign a new hot key, place the cursor in the relevant box and press the desired key combination.

### Experience

Depending on your network bandwidth, you may want to disable certain Windows visual effects to boost performance. You can either select a preset connection speed that adjusts all settings at once or customize them individually. Options include disabling desktop backgrounds, menu and window animations, full content drag, and Windows visual themes.

### Full Screen

**Show full screen connection bar**
**Auto-hide connection bar**
In full-screen mode, the remote desktop ActiveX component shows a connection bar at the top of the screen. This bar can be turned on or off. If enabled, it can either remain visible or auto-hide when not in use.

**Full screen window always stays on top**
This setting ensures that the full-screen session window stays in the foreground above all other windows.

**Enable multi-monitor support when needed**
By default, a full-screen session is restricted to the monitor where it was launched. You can enable multi-monitor support through these settings, allowing the session to span across multiple monitors. If the remote desktop resolution exceeds the current monitor's resolution, the session will extend across adjacent screens. Note: Spanning is only supported across rectangular regions—if monitors have different vertical resolutions, the session will adjust to the shortest height. Additionally, the maximum resolution for the remote desktop is limited to 4096x2048.

## Local Options

Groups and individual servers have a set of tabbed property pages that offer various configuration options. Many of these tabs are common across both groups and servers. If the **"Inherit from parent"** checkbox is selected, the settings are inherited from the parent container. Many server-side changes, such as desktop resolution, will only take effect after reconnecting to the server.

### File Settings

This tab is available when editing the properties of a file group. It allows you to set the file’s group name, displays the full (read-only) file path, and includes a section for comments.

### Group Settings

This tab appears when you view or edit the properties of a group. It contains fields for setting the group’s name, selecting its parent (for nesting purposes), and adding an optional comment.


### Server Settings

This page is dedicated to the configuration of individual server properties. You can adjust settings for the server's name, optional display name, parent group, and a comment section. If you're working with SCVMM virtual machines, it's possible to connect via RDP to the host by using the VM console connect feature. To retrieve the VM ID, run the following PowerShell command:

```powershell
get-vm | ft ElementName,Name,Id
```

This command will return the necessary identifier for establishing the connection.

### Logon Credentials

The **Logon Credentials** tab manages all login-related configurations for remote sessions. You can specify the user name, password, and domain here. Alternatively, you can enter the domain and user in a combined format such as `domain\user`.

If the "domain" isn't a Windows domain (such as in workgroup setups), you can use `\[server]` or `\[display]` as placeholders. These will be dynamically replaced with the server's name or the display name during the login process. This feature is particularly helpful when dealing with machines that require administrator-level access.

The logon information entered here becomes the default for future connections. If you wish to override this on a one-time basis, you can use the **Connect As** option from the session menu.

### Gateway Settings

The **Gateway Settings** tab enables configuration of access through a Terminal Services Gateway (TS Gateway) server. Here, you can specify the gateway address, choose an authentication method, and decide whether to enable or disable local address bypass.

For users on Windows Vista SP1, Windows Server 2008 (Longhorn Server), or newer, additional features are available:

* Manually provide credentials for the gateway server
* Choose to share gateway credentials with the target remote server

### Connection Settings

This tab provides controls for managing how the remote session is initiated and the actions taken after logging in.

You can specify whether to connect to the console session and define the RDP port for the connection.

Additionally, RDCMan can be set to run a specific program immediately upon login by providing the program's path and optionally a working directory. Note that this behavior applies only when connecting to the console session for the first time. Reconnecting or starting non-console sessions will not trigger the program launch. This behavior aligns with how Terminal Services operates.

### Remote Desktop Settings

The **Remote Desktop Settings** tab allows you to define the resolution of the remote session’s desktop. This resolution defines the logical size of the desktop environment, which may differ from the actual client view.

For example, if the remote desktop is set to 1280×1024 and the RDCMan client window is 1024×768, scrollbars will appear to navigate the entire desktop. If the client’s view is larger (e.g., 1600×1200), the full desktop will be visible, surrounded by unused space.

Selecting **"Same as client area"** will make the remote desktop match the RDCMan client area (excluding the server tree). Choosing **"Full screen"** will resize the desktop to fit the entire screen where the session is displayed.

Note: The desktop resolution is set when the session starts. Changes made to this setting while connected will not resize the session.

The maximum supported desktop size depends on the Remote Desktop ActiveX control version in use. Version 5 (pre-Windows Vista) supports resolutions up to 1600×1200. Version 6 (Vista and later) supports up to 4096×2048. These limits are enforced during the connection process, not at the configuration stage, ensuring compatibility with RDCMan files shared across different systems.

### Local Resources

Various remote server resources can be redirected back to the client machine. For instance, audio can be played locally, kept on the remote server, or turned off completely. Key combinations involving the Windows key (e.g., **Alt+Tab**) can be directed exclusively to the client, strictly to the remote session, or dynamically—targeting the remote session in full-screen mode and the client when in windowed mode.

Additionally, client-side devices like drives, ports, printers, smart cards, and the clipboard can be shared with the remote machine during a session.

### Security Settings

This section lets you configure whether the remote computer must be authenticated before the connection can be made.

### Display Settings

This page offers customization options for the thumbnail previews of remote sessions.

The primary control here is **thumbnail scale**, which dictates the amount of space allocated to each server's thumbnail. The default scale is set to 1, but it can be increased to enhance visibility for servers requiring more space—for example, a scale of 3 or 5 can improve usability while still displaying other sessions alongside. Note that this setting only applies to individual servers.

Groups have three additional display options:

* **Preview session in thumbnail** – Displays live, continuously updated previews of server activity.
* **Allow thumbnail session interaction** – If previews are enabled, this option allows direct interaction with the session within the thumbnail window.
* **Show disconnected thumbnails** – Controls whether disconnected servers are still visible in the thumbnail panel.


### Encryption Settings

RDCMan can encrypt saved passwords using either the local user account (via `CryptProtectData`) or an X509 certificate. You can access the **Encryption Settings** tab within both the Default Group Settings and File Settings dialogs.

Any personal certificates stored in the current user's certificate store that include a private key are eligible for use. To create one, run the following PowerShell command:

```powershell
New-SelfSignedCertificate -KeySpec KeyExchange -KeyExportPolicy Exportable -HashAlgorithm SHA1 -KeyLength 2048 -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=MyRDCManCert"
```

This will generate a certificate named "`MyRDCManCert`" in the Personal Certificate Store of the current user. If you plan to use the certificate on a different machine, be sure to export it along with its private key.

### Profile Management

This tab allows you to create, edit, or delete credential profiles used for authentication during remote sessions.

## List Remote Sessions

RDCMan includes basic functionality for listing remote sessions that were not initiated through it. This feature is accessible via the **\[Session.List Sessions]** menu option.

Important: The user account running RDCMan must have **Query Information** privileges on the target remote server to retrieve session details. Additionally, the server must be directly reachable; connections routed through a gateway server will not work. To use **Disconnect** or **Logoff**, the corresponding permissions must also be granted. For more details, refer to MSDN’s documentation on Remote Desktop permissions.

## Command Line

By default, RDCMan automatically reopens any files that were active when the application was last closed. You can modify this behavior by specifying particular files as command-line arguments. The following additional command-line options are supported:

* `/reset` – Resets saved application preferences, including window position and size.
* `/noopen` – Launches RDCMan without reloading previously opened files, starting with a blank environment.
* `/c server1[,server2...]` – Connects directly to the specified servers.
* `/reconnect` – Automatically reconnects to all servers that were connected at the time of shutdown, without asking for confirmation.
* `/noconnect` – Skips the prompt to reconnect servers from the last session.

## Find Servers

RDCMan provides a dialog to help locate servers, accessible via **Ctrl+F** or the **Edit > Find (servers)** menu item. All servers matching the specified regular expression pattern will appear in the dialog. You can interact with these servers via the context menu. The match is evaluated against the full server name, which includes its group path (`group\server`).

## Credential Profiles

Credential profiles allow logon details to be saved either globally or within an individual RDCMan file. This ensures credentials can be reused consistently across different groups that do not share a common parent in the tree. A common use case is centralizing credentials for both servers and gateways, meaning when a password changes, only one update is required.

Credential profiles also make it easier to share RDG files among multiple users. Since RDCMan encrypts credentials in a user-specific manner, embedding passwords directly in shared files is not feasible. Instead, users can create a credential profile—such as "Me"—within their global credential store.

You can update an existing credential profile in two ways:

1. Open the credentials dialog, enter the same name and domain, and save it back to the same store (global or file). You’ll be prompted to update the existing profile.
2. Use the **Profile Management** tab within the group properties for either the global or file credential store.

Passwords in file-scoped profiles are encrypted based on the file’s **Encryption Settings**. In contrast, global profiles use the settings specified in the **Default Group Settings**.

## Policies

RDCMan can apply policy restrictions defined in the Windows Registry at the following key:
`HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\RDCMan`

* **DisableLogOff** – Add this `DWORD` entry and set it to any non-zero value to disable the log-off command across RDCMan.

## FAQ

* **How can I log in using smartcard credentials?**
  In the **Local Resources** tab, enable the **Redirect smart cards** option.

* **I’m receiving an error when connecting via a gateway (e.g., Error 50331656). What’s the cause?**
  Ensure the gateway is specified using its fully qualified domain name (FQDN).

* **How can I enable automatic logon?**
  Modify the Group Policy settings. Open the **Group Policy MMC Snap-in** and navigate to:
  `Local Computer Policy > Computer Configuration > Administrative Templates > Windows Components > Terminal Services > Encryption and Security`
  Then, double-click **Always prompt client for password upon connection** and set it to **Disabled**.

* **Is it possible to resize the remote desktop session while connected?**
  No, resizing the desktop requires disconnecting and reconnecting. Use the **Reconnect** feature to accomplish this seamlessly. In **Display Settings**, you can configure servers to auto-reconnect with the updated resolution, whether docked or undocked.
