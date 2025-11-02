awsm-ws-switch (Awesome Workspace Switcher)

A powerful, single-script solution for monitor-aware workspace management in Hyprland.

This utility solves the multi-monitor headache by ensuring that your workspace bindings always target the correct workspace block (e.g., 1-5, 6-10, 11-15, etc.) on the currently active monitor.

✨ Features

Monitor Awareness: Automatically detects the active monitor and calculates the correct global workspace ID.

Index Inversion: Handles common monitor ordering issues (where your primary display might not be Hyprland's index 0).

Absolute Switching: Easily jump to a specific local workspace (e.g., always go to "workspace 3" on the current screen).

Relative Switching & Wrapping: Move to the next (+1) or previous (-1) local workspace, with seamless wrapping from the last to the first (and vice-versa).

Window Management: Supports both switching the workspace and moving the active window to the target workspace.

⚙️ Prerequisites

Hyprland: The script is built specifically for the Hyprland compositor.

jq: A lightweight and flexible command-line JSON processor, required for parsing Hyprland's structured output.

Installation (Debian/Ubuntu): sudo apt install jq

Installation (Arch/Manjaro): sudo pacman -S jq

🚀 Installation and Setup

Save the Script: Save the contents of ws-switch into a file named ws-switch.

Make it Executable:

chmod +x ws-switch


Move to PATH: Place the script in a directory included in your system's PATH (e.g., ~/.local/bin/):

mv ws-switch ~/.local/bin/


Configure Block Size: Open ws-switch and set the number of workspaces you want reserved per monitor (default is 5).

# ws-switch excerpt
WORKSPACES_PER_MONITOR=5


💡 Usage and Hyprland Bindings

The script accepts two arguments: <local_workspace|relative_change> and [action].

1. Absolute Switching (Super + 1-4)

Switches to the specified local workspace (1 through 4, based on your WORKSPACES_PER_MONITOR setting).

Command

Effect

ws-switch 1

Go to local workspace 1 (Global 1, 6, 11, etc.)

ws-switch 4

Go to local workspace 4 (Global 4, 9, 14, etc.)

Example hyprland.conf bindings:

# $mainMod is usually Super (Windows/Cmd key)
bind = $mainMod, 1, exec, ~/.local/bin/ws-switch 1
bind = $mainMod, 2, exec, ~/.local/bin/ws-switch 2
bind = $mainMod, 3, exec, ~/.local/bin/ws-switch 3
bind = $mainMod, 4, exec, ~/.local/bin/ws-switch 4


2. Relative Switching and Wrapping (Super + Arrows)

Moves to the next (+1) or previous (-1) workspace. If you are on the last workspace, +1 wraps you back to the first.

Command

Effect

ws-switch +1

Go to the next local workspace (wraps)

ws-switch -1

Go to the previous local workspace (wraps)

Example hyprland.conf bindings:

bind = $mainMod, right, exec, ~/.local/bin/ws-switch +1
bind = $mainMod, left, exec, ~/.local/bin/ws-switch -1


3. Moving Windows (Super + Shift + 1-4 or Arrows)

By adding the optional move argument, the active window is moved instead of the view.

Command

Effect

ws-switch 3 move

Move window to local workspace 3

ws-switch +1 move

Move window to the next local workspace (wraps)

Example hyprland.conf bindings:

# Absolute move bindings
bind = $mainMod SHIFT, 1, exec, ~/.local/bin/ws-switch 1 move
bind = $mainMod SHIFT, 2, exec, ~/.local/bin/ws-switch 2 move

# Relative move bindings
bind = $mainMod SHIFT, right, exec, ~/.local/bin/ws-switch +1 move
bind = $mainMod SHIFT, left, exec, ~/.local/bin/ws-switch -1 move


📜 License

This project is licensed under the MIT License. See the top of the ws-switch file for the full license text.
