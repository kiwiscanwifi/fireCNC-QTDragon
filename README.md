
# a fireCNC Project - LinuxCNC Expansion Module

**QtDragon CNC extension for LinuxCNC**

 - EtherCAT LC10E support
 - Safe auto-tuning
 - Multi-drive / Drive Groups for gantry
 - Tool + ATC framework
 - Editable UI styles
 - Teach mode & macro system


**Visual Map of the folder structure**

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


**Files - Tree** - Updated 5/1/2026

fireCNC/                     # Root folder for the fireCNC application
├─ atc/                      # Automatic Tool Changer modules (Batch 6-7, 16-20)
│  ├─ VirtualATC.py          # Simulates ATC behavior, pickup/return, diagnostics
│  ├─ ATCManager.py          # Handles multiple ATCs, slot assignments, macros
│  └─ SeedData/               # Example ATC configurations for seed data
│     └─ atc_tools_seed.json  # Example tools and slots for testing
│
├─ core/                     # Core modules used across the application (Batches 1-5)
│  ├─ DiagnosticBus.py        # Publish-subscribe system for diagnostics
│  ├─ DiagnosticEvent.py      # Encapsulates diagnostics messages, severity, recovery
│  ├─ Logger.py               # Central logging utility used by all modules
│  ├─ ConfigManager.py        # Loads, saves, manages configuration files
│  ├─ EtherCATTopology.py     # Represents EtherCAT network and devices
│  ├─ AutoTuneEngine.py       # Safe auto-tune algorithm for LC10E servos
│  └─ ToolWearPredictor.py    # Analyzes diagnostic data to predict tool wear
│
├─ data/                     # Seed data and persistent storage
│  ├─ ethercat_seed.json       # Example EtherCAT LC10E configuration
│  ├─ atc_tools_seed.json      # Default tools and slots
│  ├─ teach_timeline_seed.json # Example teach-mode timeline steps
│  ├─ gcode_sample.ngc         # Example GCode file with comments
│  ├─ style_config.json        # Default style guide values (fonts/colors)
│  └─ workspace_config.json    # Saved positions, tool offsets, work offsets
│
├─ gcode/                    # GCode management and translation (Batches 2, 21-25)
│  ├─ GCodeCommentTranslator.py  # Translates GCode to human-readable with modal awareness
│  ├─ CutTimeEstimator.py         # Estimates machining time from feed/path
│  ├─ ModalStateManager.py        # Tracks modal states G54-G59.3, G41/G42
│  └─ ToolOffsetManager.py        # Handles multi-tool Z offsets and spindle offsets
│
├─ intelligence/             # AI / prediction / simulation modules (Batch 25-30)
│  ├─ MacroDebugger.py         # Step-through debugger for macros
│  ├─ DigitalTwin.py           # Simulated machine for verification/testing
│  ├─ TimelineValidator.py     # Validates teach-mode timelines
│  └─ RecoveryManager.py       # Automatic recovery macros triggered from diagnostics
│
├─ macros/                   # Predefined macro scripts for ATC, tools, safety
│  ├─ MacroError.py           # Exception class for macro aborts
│  ├─ ATCMacros.py            # Pickup/return macros for ATC slots
│  ├─ ToolMacros.py           # Zeroing, probing, tool-specific actions
│  └─ SafetyMacros.py         # E-Stop, safe motion, spindle/shutdown macros
│
├─ safety/                   # Safety logic and E-Stop escalation (Batch 21-25)
│  ├─ SafetyController.py      # Handles limits, E-Stop escalation rules
│  ├─ SafetyConfig.json        # User-configurable safety parameters
│  └─ RecoveryActions.py       # Predefined recovery sequences
│
├─ teach/                    # Teach-mode workflow (Batches 1-10, 16-20)
│  ├─ TimelineEditor.py        # GUI editor for timeline steps
│  ├─ TimelineValidator.py     # Validation for timeline integrity
│  ├─ TeachController.py       # Executes teach-mode timelines with diagnostics
│  └─ GCodeExecutor.py         # Runs GCode with commenting and diagnostics
│
├─ ui/                       # QtDragon UI components
│  ├─ MainWindow.py            # Main application window
│  ├─ DiagnosticPanel.py       # Displays diagnostic events in real time
│  ├─ ATCPanel.py              # Interactive ATC control UI
│  ├─ TeachModePanel.py        # Timeline editor panel
│  ├─ StyleEditor.py           # UI to edit fonts/colors, hybrid style system
│  ├─ WorkspaceVisualizer.py   # 2D/3D visualization of axes, tools, ATC slots
│  ├─ MacroDebuggerPanel.py    # Step-through macro debugger UI
│  └─ DigitalTwinPanel.py      # Simulation panel for Digital Twin
│
├─ ethercat/                  # EtherCAT device support (LC10E, multi-drivers)
│  ├─ LC10EController.py       # Interface with LC10E servo drives
│  ├─ EtherCATMonitor.py       # Monitors EtherCAT communication & PDOs
│  └─ PDOMapper.py             # Maps EtherCAT PDOs to HAL pins & variables
│
├─ sim/                       # Simulation support
│  ├─ MotionSimulator.py       # Simulates machine motion for testing
│  └─ CollisionDetector.py     # Detects collisions in digital twin
│
└─ README.md                  # Full documentation, installation, and usage guide

