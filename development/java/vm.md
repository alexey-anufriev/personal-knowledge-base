# VM

## OpenJ9

### Xdump Option Builder

[Xdump](https://www.eclipse.org/openj9/docs/xdump/) Option allows generating diagnostic information files for further analysis. But the format of it and possible combinations of keys for this option is quite big. [Xdump Option Builder](https://www.eclipse.org/openj9/tools/xdump_option_builder.html) can significantly simplify this exercise. 

## Process ID

`ManagementFactory.getRuntimeMXBean().getName()` normally gives `PID@Host` output.

