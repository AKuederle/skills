---
name: background-gui-automation
description: Run desktop GUI automation invisibly in an isolated KDE Wayland session through kwin-mcp. Use whenever an agent needs to launch or operate GUI apps without showing windows, moving the user's pointer, or taking keyboard focus, including background app testing and workflows involving several apps.
---

# Background GUI automation

For GUI automation the user should not see, always use the `kwin-mcp` MCP server's isolated virtual session. Keep the user's desktop, pointer, keyboard focus, and clipboard undisturbed. Ordinary KDE virtual desktops and extra monitors do not provide separate input.

Direct APIs, file operations, and browser automation that already runs without desktop input do not need a graphical session. When desktop interaction is required, use this workflow. Operating an existing window on the user's live desktop requires an explicit request for that live interaction.

## Start and use a private session

1. Discover the configured `kwin-mcp` tools and inspect their current schemas. Start with `session_start`, never `session_connect`. Do not pass `--default-live-session` or inherit the user's display as a fallback. The global Codex configuration disables `session_connect` for this server.
2. Start one virtual session for the task. Prefer temporary app settings through home isolation unless the task needs an existing profile. Use absolute paths for input files and outputs that must survive cleanup. Home isolation separates settings; it does not restrict filesystem or network access.
3. Launch every participating app through the session's launch tool. Several windows and apps can share this session and interact normally. Enable its clipboard only when the workflow needs it. Use dedicated browser profiles or new-instance options when an app might otherwise reuse a host process.
4. Wait for the intended window or element to appear, then focus it before input. App launch returning a PID does not mean its window is ready. Observe through the session's accessibility tools and screenshots. Focus, click, type, and drag through its own input tools. Use fresh element information or screenshots for targets and verify the result. Follow the screenshot's coordinate mapping when it has been resized or cropped. Use the Unicode typing tool for text outside ASCII.
5. Release any held keys or buttons and call `session_stop` when finished, including after errors. Save requested outputs outside temporary session directories first. Retain screenshots or profiles only when needed, and report retained paths.

## Installed tool versions

The published package tested during setup was kwin-mcp 0.12.0. Its names differ from the upstream 1.0 documentation. The advertised MCP schemas are authoritative.

For 0.12, a minimal sequence of MCP calls is:

```text
session_start(isolate_home=true, screen_width=1920, screen_height=1080)
launch_app(command="kate --new")
launch_app(command="kcalc")
list_windows()
accessibility_tree(app_name="kcalc")
screenshot()
session_stop()
```

Use `enable_clipboard=true` on `session_start` when apps must exchange clipboard contents. Other 0.12 tools include `wait_for_element`, `find_ui_elements`, `invoke_ui_action`, `focus_window`, `mouse_click`, `keyboard_type`, `keyboard_type_unicode`, and `keyboard_key`. In 1.0, examples include `app_launch`, `window_list`, `element_query`, and `screen_capture`; inspect their schemas rather than mixing versions.

## Failure handling

If the server is missing, discover whether its tools need loading, then check `codex mcp get kwin-mcp` and `kwin-mcp --help`. A newly configured server may require a fresh agent session. The local interactive equivalent is `kwin-mcp-cli`; its commands belong to one persistent process, not separate invocations per action.

If startup, capture, or input fails, inspect the reported session logs and fix the private session. The server needs the current user's valid `XDG_RUNTIME_DIR` even when the MCP client filters inherited environment variables. Never solve a failure by injecting input through `ydotool`, host-targeted `xdotool`, the host remote-desktop portal, or a live desktop connection. If private automation cannot proceed, report the blocker without opening apps on the user's desktop.

The server's source and dependency instructions are at [kwin-mcp](https://github.com/isac322/kwin-mcp). Global setup uses a locally installed executable; do not silently upgrade it or replace the user's MCP configuration during ordinary automation.
