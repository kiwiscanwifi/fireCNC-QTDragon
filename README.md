
# a fireCNC Project - LinuxCNC Expansion Module

**QtDragon CNC extension for LinuxCNC**

 - EtherCAT LC10E support
 - Safe auto-tuning
 - Multi-drive / Drive Groups for gantry
 - Tool + ATC framework
 - Editable UI styles
 - Teach mode & macro system




| Folder/File     | Purpose / Why It Exists                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------- |
| `atc/`          | All ATC-related logic and simulation. Handles slots, pickups, returns, tool history, and macros.                    |
| `core/`         | Central system logic: diagnostics, auto-tune, EtherCAT, tool wear, config management. Other modules depend on this. |
| `data/`         | Seed data and persistent storage for tools, EtherCAT, timelines, GCode, style guide, workspace configuration.       |
| `gcode/`        | Handles all GCode parsing, translation, modal awareness, tool/work offsets, and cut-time estimation.                |
| `intelligence/` | Advanced features: macro debugger, timeline validation, digital twin, automatic recovery.                           |
| `macros/`       | Pre-written scripts for ATC, safety, tools. Used in teach mode and recovery.                                        |
| `safety/`       | Handles limits, E-Stop escalation, and safe recovery actions.                                                       |
| `teach/`        | Teach-mode timeline editor, validation, execution of user-recorded steps.                                           |
| `ui/`           | QtDragon-based GUI: panels for teach mode, diagnostics, ATC, style editor, visualizer, debugger, and digital twin.  |
| `ethercat/`     | Communication with EtherCAT drives (LC10E), including PDO mapping, monitoring, and multiple driver support.         |
| `sim/`          | Optional simulation support for motion, digital twin, and collision detection.                                      |
| `README.md`     | Central documentation, installation guide, usage instructions.                                                      |



**Dependency Notes**
 - ui/ panels rely on core/ for diagnostics and configuration.
 - teach/ depends on gcode/ for execution and macros/ for actions.
 - atc/ interacts with teach/ and macros/ to pick/return tools.
 - ethercat/ modules provide servo/axis data to teach mode, ATC, and
   simulator.
 - intelligence/ modules monitor and validate workflows, feed recovery
   instructions to macros and UI.
 -sim/ is optional but uses data from teach/, atc/, and ethercat/ for
   predictive testing.
