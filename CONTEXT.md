# rm_config

Per-robot configuration. Deploying code to a robot means selecting/editing that robot's config file — no code changes needed to switch chassis or subsystems.

## Language

**Robot config file**:
The yaml file for one robot, e.g. `config/rm_controllers/series_legged.yaml`. Declares every controller the robot runs and all their parameters.

**Config-driven deployment**:
The workflow where the config file is the single source of truth for what runs on a robot: the `type:` field under each controller block names the controller plugin to load (e.g. `type: rm_chassis_controllers/BipedalController`). Changing robots = changing which config is loaded, not changing code.
_Avoid_: hardcoding robot selection

**EtherCAT setup file**:
`config/rm_ecat_hw/<robot>.yaml` — passed to the `rm_ecat_hw` node as `setupFile`. Declares the master (`ethercat_master.time_step`, 0.001 = 1 kHz) and every slave (`ethercat_devices`: type `Rm`/`Mit`, name, NIC, EtherCAT address, and a pointer to its device configuration).

**Device configuration**:
`config/rm_ecat_hw/device_configurations/<name>.yaml` — describes the CAN devices behind one EtherCAT slave: `can_motors` (can_id, can_bus, name, motor type, `need_calibration`), `can_imus`, `dbus`. The standard (Rm) slave is a CAN-to-EtherCAT bridge board.
