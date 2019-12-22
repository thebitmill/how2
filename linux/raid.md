# Raid 1 + LVM

1. `# mdadm --create --verbose --level=1 --metadata=1.2 --raid-devices=2 /dev/md/0 /dev/sda1 /dev/sdb1`
2. `# mdadm --create --verbose --level=1 --metadata=1.2 --raid-devices=2 /dev/md/1 /dev/sdc1 /dev/sdd1`
3. Add array to configuration and assemble, see arch wiki
4. `# pvcreate /dev/md/0`
5. `# pvcreate /dev/md/1`
6. `# vgcreate vg0 /dev/md/0`
6. `# vgextend vg0 /dev/md/1`
