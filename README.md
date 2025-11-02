
# ws-switch: Awesome Monitor-Aware Hyprland Workspace Switcher 🖥️

---

## 🚀 Overview

`ws-switch` is a versatile BASH script designed for **Hyprland** users managing **multi-monitor setups**. Its primary goal is to decouple your workspace navigation from Hyprland's global workspace IDs (which can be confusing across monitors) by implementing a local workspace mapping system.

The script **partitions** your total set of workspaces into contiguous blocks, assigning one block exclusively to each connected monitor. For example, if you set the configuration to 5 workspaces per monitor:
* **Monitor 1** uses local workspaces 1-5 (Global IDs 1-5)
* **Monitor 2** uses local workspaces 1-5 (Global IDs 6-10)
* **Monitor 3** uses local workspaces 1-5 (Global IDs 11-15)

This allows you to bind keys like `SUPER + 1` to always go to the *first* workspace on your **currently active monitor**, regardless of its global ID.

---

## ✨ Key Features

* **Monitor-Aware Dispatching:** Calculates the correct global workspace ID based on the active monitor's index.
* **Local & Relative Input:** Accepts absolute local numbers (`3`) or relative changes (`+1`, `-2`).
* **Boundary Wrapping:** Relative shifts wrap around the local block size (e.g., moving left from local 1 goes to local 5 if block size is 5).
* **Dual Actions:** Supports both **switching** focus (`dispatch workspace`) and **moving** the active window (`dispatch movetoworkspace`).
* **Dependencies:** Relies only on **`jq`** for JSON parsing of `hyprctl` output.

---

## ⚙️ Prerequisites

To ensure `ws-switch` functions correctly, you must have the following installed:

1.  **Hyprland:** The compositor environment.
2.  **`jq`:** The command-line JSON processor.

Installation example (Arch Linux):
```bash
sudo pacman -S jq
````

-----

## 🛠️ Setup

1.  **Save the Script:** Save the provided script content to a location in your `$PATH`, e.g., `/usr/local/bin/ws-switch`.
2.  **Permissions:** Ensure it is executable:
    ```bash
    chmod +x /usr/local/bin/ws-switch
    ```

-----

## 📝 Configuration

The script behavior is controlled via a single variable at the top of the script file:

### `WORKSPACES_PER_MONITOR`

This integer defines the size of the local workspace pool dedicated to *each* physical monitor.

```bash
# In the script file:
WORKSPACES_PER_MONITOR=5 
```

**Adjust this value** within the script file to match the number of workspaces you want to use per monitor.

> **Note on Monitor Ordering:** The script attempts to dynamically determine the physical monitor order using `hyprctl monitors -j` and inverts the index if necessary to better match common expectations (though Hyprland's own reporting can sometimes be non-deterministic).

-----

## 💡 Usage Syntax

The script takes the desired target as the first argument, and an optional action as the second.

$$\text{ws-switch} \langle \text{target} \rangle \ [ \text{action} ]$$

### Arguments

| Target Type | Example | Description |
| :--- | :--- | :--- |
| **Absolute Local WS** | `3` | Targets local workspace **3** on the current monitor. |
| **Relative Shift** | `+1` | Shifts one local workspace **forward** (e.g., 3 $\to$ 4). |
| **Relative Shift** | `-2` | Shifts two local workspaces **backward** (e.g., 4 $\to$ 2). |

| Action Type | Example | Description |
| :--- | :--- | :--- |
| **`switch`** | `ws-switch 2` | **Switches** focus to the calculated workspace. (Default) |
| **`move`** | `ws-switch 5 move` | **Moves** the active window to the calculated workspace. |

-----

## 🖼️ Hyprland Keybinds (Recommended)

This is where `ws-switch` shines. You will replace your standard `workspace` and `movetoworkspace` binds in your `hyprland.conf` file with calls to this script.

### ➡️ Absolute Switching and Moving

These binds are set up for a `WORKSPACES_PER_MONITOR=5` configuration.

```conf
# -----------------------------------------------------------------------------
# Absolute Switching (SUPER + Number)
# -----------------------------------------------------------------------------
bind = SUPER, 1, exec, ws-switch 1
bind = SUPER, 2, exec, ws-switch 2
bind = SUPER, 3, exec, ws-switch 3
bind = SUPER, 4, exec, ws-switch 4
bind = SUPER, 5, exec, ws-switch 5

# -----------------------------------------------------------------------------
# Absolute Moving (SUPER + SHIFT + Number)
# -----------------------------------------------------------------------------
bind = SUPER SHIFT, 1, exec, ws-switch 1 move
bind = SUPER SHIFT, 2, exec, ws-switch 2 move
bind = SUPER SHIFT, 3, exec, ws-switch 3 move
bind = SUPER SHIFT, 4, exec, ws-switch 4 move
bind = SUPER SHIFT, 5, exec, ws-switch 5 move
```

### 🔁 Relative Switching and Moving

These binds are essential for quick, continuous navigation with wrapping enabled.

```conf
# -----------------------------------------------------------------------------
# Relative Switching (SUPER + Arrow)
# Cycles to the next or previous local workspace with wrapping (e.g., 5 -> 1 or 1 -> 5).
# -----------------------------------------------------------------------------
bind = SUPER, right, exec, ws-switch +1
bind = SUPER, left, exec, ws-switch -1

# -----------------------------------------------------------------------------
# Relative Moving (SUPER + SHIFT + Arrow)
# Moves the active window to the next or previous local workspace.
# -----------------------------------------------------------------------------
bind = SUPER SHIFT, right, exec, ws-switch +1 move
bind = SUPER SHIFT, left, exec, ws-switch -1 move
```

-----

```

You can now use the content above to create your `README.md` file! Let me know if you need any further help with your Hyprland setup.
```
