# fzf-talosctl

*Your interactive sidekick for managing Talos Linux nodes.*

`fzf-talosctl` is a powerful, interactive TUI (Text-based User Interface) wrapper for `talosctl`, built with `fzf`. It simplifies common `talosctl` operations by providing interactive, filterable, and sortable views for logs, services, containers, and more, directly in your terminal.

## Features

*   **Interactive Node Selection**: If no node IP is provided, the script interactively prompts you to select a node from your current `kubectl` context.
*   **Comprehensive Main Menu**: A clean, organized menu to access various `talosctl` functions.
*   **Powerful Interactive Filtering**: Leverages `fzf` to provide instant fuzzy finding (just start typing) and exact-match searching (prefix with `'`).
*   **Live Log Streaming**: View logs for services and containers in real-time.
    *   New logs appear at the top for easy viewing.
    *   Jump to the newest log entry with `Ctrl+J`.
    *   Toggle log tracking on and off with `Ctrl+T`.
*   **Live `dmesg` Streaming**: View kernel messages in real-time, just like other logs.
    *   Jump to the newest entry with `Ctrl+J`.
    *   Toggle log tracking on and off with `Ctrl+T`.
*   **Service Status with Preview**: Browse services and see their detailed status and events in a live preview pane.
*   **Dual Container Views**: Separately view Kubernetes-managed containers and Talos system containers.
*   **Advanced `netstat` Viewer**:
    *   View detailed network connection status (`netstat -alkpvx`).
    *   Sort by Proto, Local/Foreign Address, State, UID, Program, and Pod.
    *   Handles inconsistent column output between TCP and UDP connections.
*   **Directory Tree Browser**:
    *   Navigate the node's filesystem.
    *   Sort files and directories by name (`Ctrl+N`) or size (`Ctrl+S`).
    *   View file contents directly in `less`.

## Dependencies

Before using `fzf-talosctl`, make sure you have the following command-line tools installed and available in your `PATH`:

*   `talosctl`
*   `kubectl`
*   `fzf`
*   `stdbuf` (part of `coreutils`)

## Installation

1.  Clone this repository or download the `fzf-talosctl` script.
2.  Make the script executable:
    ```sh
    chmod +x fzf-talosctl
    ```
3.  (Optional) Place the script in a directory that is in your `PATH` (e.g., `/usr/local/bin`) for easy access from anywhere. You can also create a symlink.
    ```sh
    # Example:
    ln -s /path/to/fzf-talosctl /usr/local/bin/fzf-talosctl
    ```

## Usage

There are two ways to run the script:

1.  **Interactive Mode**: Run the script without any arguments. It will prompt you to select a node from your current `kubectl` context.
    ```sh
    ./fzf-talosctl
    ```

2.  **Direct Mode**: Provide the IP address of a Talos node as the first argument. This is useful for scripting or integrating with other tools like K9s.
    ```sh
    ./fzf-talosctl 10.55.0.2
    ```

## Integration with K9s

You can easily integrate `fzf-talosctl` into K9s as a node-level plugin. This allows you to select a node in K9s and press `t` to launch an interactive session for that specific node.

1.  Find your K9s `plugins.yaml` file by running `k9s info`.
2.  Add the following block to your `plugins.yaml`:

    ```yaml
    talos-interactive:
      shortCut: t
      description: "Talos: Interactive session"
      scopes:
        - nodes
      command: fzf-talosctl
      background: false
      args:
        - $COL-INTERNAL-IP
    ```

## Keybindings

In addition to the powerful filtering capabilities mentioned in the Features section, `fzf-talosctl` uses a consistent set of keys for navigation:

*   **Navigate**: Use `Up`/`Down` arrows, `Ctrl-P`/`Ctrl-N`, or `Ctrl-J`/`Ctrl-K`.
*   **Select**: Press `Enter`.
*   **Go Back / Cancel**: Press `ESC`.

The tables below detail *additional* keybindings specific to each view.

#### Log Viewer (Services/Containers)
| Key | Action |
|---|---|
| `Ctrl+J` | Jump to newest log entry (top of the list) |
| `Ctrl+T` | Toggle log tracking on/off |
| `ESC` | Return to the previous menu |

#### Dmesg Viewer
| Key | Action |
|---|---|
| `Ctrl+J` | Jump to newest log entry (top of the list) |
| `Ctrl+T` | Toggle log tracking on/off |
| `ESC` | Return to the main menu |

#### File Browser
| Key | Action |
|---|---|
| `Enter` | Descend into directory or view file |
| `Ctrl+N` | Sort by Name (toggle asc/desc) |
| `Ctrl+S` | Sort by Size (toggle asc/desc) |
| `ESC` | Go to parent directory (`..`) |

#### Netstat Viewer
| Key | Action |
|---|---|
| `Ctrl+O` | Sort by Proto |
| `Ctrl+L` | Sort by Local Address |
| `Ctrl+F` | Sort by Foreign Address |
| `Ctrl+S` | Sort by State |
| `Ctrl+U` | Sort by UID |
| `Ctrl+G` | Sort by Program |
| `Ctrl+P` | Sort by Pod |
| `ESC` | Return to the main menu |

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Contributing

Contributions are welcome! Please feel free to open an issue or submit a pull request.