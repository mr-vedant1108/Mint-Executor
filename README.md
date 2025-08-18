https://github.com/mr-vedant1108/Mint-Executor/releases

# Mint Executor — Lightweight Roblox Script Runtime & Learning Tool 🌿🧩

[![Releases](https://img.shields.io/badge/Releases-Download-brightgreen)](https://github.com/mr-vedant1108/Mint-Executor/releases)  
![License](https://img.shields.io/badge/License-MIT-blue) ![Language](https://img.shields.io/badge/Language-C%2B%2B%20%7C%20Lua-orange) ![Status](https://img.shields.io/badge/Status-Level%203-yellow)

![Mint Executor Banner](https://images.unsplash.com/photo-1524594154906-30b2f91b5d5a?auto=format&fit=crop&w=1650&q=80)

Overview
--------
Mint Executor is a compact, open-source runner built as an educational tool and lightweight runtime for script evaluation. It targets developers, reverse engineers, and testers who want a controlled environment to learn how script hosts and runtime injection work in a sandbox. The project focuses on clarity, modular design, and safe local testing.

Mint offers:
- keyless access for straightforward testing workflows
- partial support for two script encodings and runtime modes (approximate split: 34% sUNC, 67% UNC feature coverage)
- a neutral compatibility profile (Level 3): reliable for basic workloads, limited for advanced cases

If you want the release package, download the release file and execute it in a controlled test environment from the releases page: https://github.com/mr-vedant1108/Mint-Executor/releases

Badges and Quick Links
----------------------
- Releases: [![Releases](https://img.shields.io/badge/Releases-Download-brightgreen)](https://github.com/mr-vedant1108/Mint-Executor/releases)  
- Documentation: /docs (local or in repo)  
- Issues: Use GitHub Issues to report bugs or request features

Project Goals
-------------
- Provide a transparent reference implementation of a script runtime host.
- Keep core smaller and readable to make learning easier.
- Offer safe patterns for testing injection and runtime evaluation locally.
- Document trade-offs and compatibility reasons so users can make informed choices.

Who this is for
---------------
- Developers learning how script hosts manage memory and execution.
- Security researchers building safe test harnesses.
- Contributors who want a small, focused codebase to study or extend.
- Educators who need a simple runtime demonstration for classes or workshops.

What Mint is not
----------------
- Mint is not a production exploit toolkit.
- Mint does not aim to provide high compatibility across every platform.
- Mint does not include closed-source binaries without reason; all core logic stays open.

Quick feature list
------------------
- Script loading and sandboxed evaluation
- Basic API stub for runtime hooks and callbacks
- Two-mode encoding support (labeled sUNC and UNC here)
- Minimal GUI for local testing (optional)
- Logging and telemetry for debugging (local files only)
- Modular plugin loader for community extensions

Architecture overview
---------------------
Mint splits responsibilities into small modules:

- Core runtime
  - Script parser and loader
  - Execution scheduler
  - Memory sandboxing primitives

- IO layer
  - File loader for local scripts
  - Release loader for packaged artifacts

- Compatibility layer
  - sUNC adapter (partial)
  - UNC adapter (partial)

- Plugin system
  - Interface definition for extensions
  - Loader that isolates plugin runtime

- UI (optional)
  - Minimal window for input, output, and logs
  - Command palette for quick commands

Design principles
-----------------
- Clear APIs. Keep public interfaces small and well-documented.
- Fail safe. Errors return clear codes. Logging stays local.
- Isolated testing. Encourage execution inside VMs or containers.
- Modular growth. Add features as pluggable modules.

Compatibility and rating
------------------------
Mint carries a Level 3 compatibility rating. That means it works well for standard scripts and learning scenarios. It does not guarantee compatibility with advanced, platform-specific features. When compatibility matters, check feature matrices and run tests in your environment.

Support breakdown (approximate)
- sUNC mode: ~34% of targeted behaviors implemented
- UNC mode: ~67% of targeted behaviors implemented

This split reflects the current implementation focus. Community contributions can expand either side.

Where to get releases
---------------------
Download the release file and execute it in a controlled test environment from the releases page: https://github.com/mr-vedant1108/Mint-Executor/releases

If you use the release artifacts, review their contents and run them in an isolated VM, container, or sandbox environment. The releases page lists packaged builds and sample assets.

Installation
------------
This section lists safe, general steps to start using Mint as a local development and learning tool. Mint targets a desktop development environment and developer workflows. It avoids system-wide changes.

Important general workflow
1. Visit the releases page and download the release artifact for your platform. The release file needs to be downloaded and executed in a controlled test environment: https://github.com/mr-vedant1108/Mint-Executor/releases
2. Inspect the package contents. Review source files, scripts, and runtime manifests.
3. Run in an isolated VM, container, or sandbox with no network access for initial tests.
4. Use the provided test scripts and sample plugins to verify behavior.

Platform notes
- Windows: Run the included .exe from a restricted user account inside a VM.
- macOS: Use the packaged bundle. Execute inside an isolated environment if possible.
- Linux: Use the AppImage or tarball inside a container.

Package contents (typical)
- bin/ — runtime binaries (platform-specific)
- lib/ — core shared modules
- samples/ — example scripts and tests
- plugins/ — community plugin examples
- docs/ — detailed API docs and guides
- CHANGELOG.md, LICENSE, README.md

Local build (developer mode)
1. Clone repository.
2. Install build dependencies (CMake, compiler toolchain).
3. Build using CMake: configure, generate, then build.
4. Run unit tests included in tests/.

Testing and sandboxing
----------------------
Mint includes test harnesses. The harnesses validate basic runtime features and compatibility modes.

Testing checklist
- Run unit tests: ensure core parsers and adapters pass.
- Run integration tests inside a sandbox: verify the runtime does not leak resources.
- Use the sample scripts under samples/ to validate UNC and sUNC modes.

Recommended sandbox options
- VirtualBox or VMware VM with snapshots
- Docker or LXC container (for Linux)
- Dedicated test machine disconnected from production networks

Logging and telemetry
---------------------
Mint writes logs to a local file in the workspace. Logs include timestamps and structured entries to help debugging. Logging aims to be readable and actionable.

Log locations (defaults)
- Windows: %APPDATA%/MintExecutor/logs/
- macOS/Linux: ~/.mint-executor/logs/

Log format
- Timestamp | Module | Level | Message | Meta JSON

Levels: DEBUG, INFO, WARN, ERROR

Configuration
-------------
Configuration uses simple JSON files in the repo root or user config directory.

Example config keys
- runtimeMode: "UNC" | "sUNC"
- maxMemoryMB: integer
- enableGui: true | false
- pluginDirs: array of paths
- logLevel: "INFO" | "DEBUG" | "WARN"

Sample config (schema)
{
  "runtimeMode": "UNC",
  "maxMemoryMB": 256,
  "enableGui": true,
  "pluginDirs": ["./plugins"],
  "logLevel": "DEBUG"
}

Scripting API and hooks
-----------------------
Mint exposes a small scripting API for runtime interaction. The API aims to teach how hosts expose services to loaded scripts.

Core APIs
- mint.loadScript(path) — load a script into the runtime sandbox
- mint.runScript(handle) — schedule and run a loaded script
- mint.setTimeout(fn, ms) — timer primitive (sandbox-bound)
- mint.registerHook(name, fn) — register host-provided callbacks
- mint.log(level, message) — log through host

Plugin API
Plugins must implement a minimal interface:

- init(host) — called when plugin loads; host provides API surface
- shutdown() — cleanup
- metadata — JSON with name, version, author

Plugin isolation
- Plugins load in a separate context
- Plugins cannot access host internals directly; they use the public host API
- Plugins run with restricted privileges to avoid side effects

Sample plugin layout
plugins/sample-plugin/
  plugin.json
  plugin.dll (or .so or .dylib or script)

Sample plugin metadata
{
  "name": "sample-plugin",
  "version": "0.1.0",
  "author": "contributor",
  "description": "Simple plugin demonstrating hook usage"
}

Security model
--------------
Mint treats all scripts as untrusted. The runtime provides limited APIs and denies direct OS access. Security centers on safe defaults.

Key measures
- Memory caps per-script
- No outbound network by default
- File access allowed only to configured paths
- Runtime execution limits (time slices and watchdog timers)
- Transparent logging of resource use

Development and coding style
----------------------------
Stick to these guidelines:
- Use small functions with single responsibility.
- Keep public API surface minimal.
- Write tests for any behavior change.
- Document any non-obvious behavior in the docs/ folder.

Coding standard (high level)
- C++ code uses RAII patterns for resource management.
- Avoid global mutable state.
- Use clear naming for modules and functions.
- Comment complex algorithms. Prefer simple implementations.

Contribution process
--------------------
- Fork the repo
- Create a feature branch: feature/<short-name>
- Add tests for any new behavior
- Submit a PR with description and reference to issues
- Maintainers will review and request changes

Issue policy
- Open descriptive issues with steps to reproduce and logs.
- Tag issues with categories: bug, enhancement, discussion.
- Provide minimal reproducible examples when possible.

Testing and CI
--------------
The repository integrates unit tests and integration tests.

CI checks
- Build on multiple platforms
- Run unit tests
- Linting and static checks

Local test commands (developer environment)
- cmake -S . -B build
- cmake --build build
- ctest --test-dir build

Performance notes
-----------------
Mint targets compactness and clarity rather than raw speed. Performance is adequate for small workloads and learning. Expect overhead from sandboxing and safety checks.

Profiling tips
- Use built-in timing hooks and log durations
- Run test cases under profiler in an isolated environment
- Adjust maxMemoryMB and time-slice values to match expected script load

Limitations and trade-offs
--------------------------
- Not all platform-specific ops are implemented.
- The sUNC adapter remains partial by design.
- No guarantee against false positives in compatibility tests.
- The minimal GUI aims for clarity, not for polishing.

Roadmap
-------
Planned items:
- Expand sUNC coverage to >60%
- Add more sample scripts and tests
- Improve plugin isolation mechanism
- Add language bindings for a higher-level API (e.g., Python)
- Create structured docs with interactive examples

Changelog highlights
--------------------
See CHANGELOG.md in the repo for a full history of releases and breaking changes.

Common troubleshooting
----------------------
- Runtime fails to start: check logs in ~/.mint-executor/logs (or platform equivalent).
- Scripts time out: increase runtime timeout or split workloads.
- Plugin fails to load: verify plugin metadata and compatibility.

Frequently asked questions (FAQ)
-------------------------------
Q: What does Level 3 mean?
A: Level 3 indicates stable basic features and partial compatibility. The runtime handles common cases but will fail on edge or advanced features.

Q: Is Mint safe to use on public platforms?
A: Run Mint only in controlled environments. The runtime does not modify other processes or external services.

Q: How do I add a new adapter?
A: Implement the adapter interface in lib/adapters, include tests, and register the adapter in the compatibility layer.

Q: Where do I report bugs?
A: Use GitHub Issues with a clear reproduction and logs.

Samples and examples
--------------------
The samples directory contains example scripts for both sUNC and UNC modes. Each sample includes comments and test harness entries.

Example sample structure
- samples/basic/hello_world.lua
- samples/compat/sunc_sample.lua
- samples/compat/unc_sample.lua

Each sample file includes:
- Purpose comment
- Required host features
- Expected output

Developer notes: build and test locally
---------------------------------------
- Follow the local build steps in docs/dev-setup.md
- Use the sample scripts to validate the runtime
- Run tests in a sandbox or VM

Code of conduct
---------------
Follow a respectful, constructive tone in all interactions. Provide clear, reproducible issue reports. Maintain open collaboration.

License
-------
Mint uses the MIT license. See LICENSE in the repo for terms.

Attribution and credits
-----------------------
- Core contributors: see CONTRIBUTORS.md
- Community plugin authors: listed in plugins/metadata
- Inspiration from community runtimes and academic work on sandboxing

Contact and support
-------------------
- Use GitHub Issues
- For private or security disclosures, open an issue with the label security and mark it private, or contact maintainers via the emails in CONTRIBUTORS.md

Appendix A — Glossary
---------------------
- Runtime: component that evaluates scripts in a host environment.
- Sandbox: isolated execution context with restricted access.
- Adapter: compatibility layer for script variants (sUNC, UNC).
- Plugin: modular extension that adds features to the core host.

Appendix B — File audit checklist
---------------------------------
Before running any release artifact:
- Inspect all files in the release package.
- Verify checksums and signatures when available.
- Run inside an isolated VM or container.
- Check for unexpected scripts, network code, or obfuscated binaries.

Appendix C — Example config and sample script
---------------------------------------------
Sample config (config.json)
{
  "runtimeMode": "sUNC",
  "maxMemoryMB": 128,
  "enableGui": false,
  "pluginDirs": ["./plugins"],
  "logLevel": "DEBUG"
}

Sample script (samples/basic/hello_world.lua)
-- Hello world sample for Mint Executor
mint.log("INFO", "Hello world from sample script")
local handle = mint.loadScript("samples/basic/helper.lua")
mint.runScript(handle)

Further reading
---------------
- Read docs/architecture.md for deep dives into core modules
- See docs/sandboxing.md for isolation design and rationale
- Consult docs/plugin-spec.md for plugin interface details

Releases and downloads
----------------------
Download the release file and execute it in a controlled test environment from the releases page: https://github.com/mr-vedant1108/Mint-Executor/releases

Stay mindful when running binaries. Inspect artifacts first.