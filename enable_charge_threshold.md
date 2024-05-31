To enable battery charge threshold (60%), do this as root:

```
echo 1 > /sys/bus/platform/drivers/ideapad_acpi/VPC2004:00/conservation_mode
```

Conversely, to disable it (back to 100%), do this as root:

```
echo 0 > /sys/bus/platform/drivers/ideapad_acpi/VPC2004:00/conservation_mode
```