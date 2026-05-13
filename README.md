
*Show these to your friends or family. My dad bought a 16tb usb from Chinese sellers and i confirmed scamage to him through these commands. Help who you can by showing them these things that are output here.* 

Important: Some legitimate USB enclosures may fail SMART detection or behave differently under Linux. One failed command alone does not prove a device is counterfeit. The strongest evidence comes from combining F3 probe results, kernel I/O errors, inconsistent device geometry, and suspicious controller behavior together.


# Fake USB / Fake SSD Discovery Commands

## 1. SMART / Bridge Detection

```
sudo smartctl -a /dev/sda
```

### Red flags:

- Unknown USB bridge
- SMART unavailable
- Nonsense manufacturer info
- Failure to identify controller properly

Important:  
Some legitimate USB bridges also block SMART, so this alone is not proof — just a warning sign.

---

## 2. F3 Validation (MOST IMPORTANT)

```
sudo f3probe --destructive --time-ops /dev/sda
```

### Red flags:

- counterfeit of type limbo
- usable size much smaller than advertised
- invalid geometry
- read/write failures

This is basically the gold-standard Linux fake-flash detector.

---

## 3. Kernel Error Inspection

```
dmesg | tail -50
```

### Red flags:

- Logical block address out of range
- Buffer I/O error
- USB disconnects
- reset high-speed USB device
- async page read/write failures

---

## 4. Disk Geometry / Capacity

```
sudo fdisk -l /dev/sda
```

### Red flags:

- wildly inconsistent size reports
- impossible geometry
- missing partition tables
- strange sector sizes

---

## 5. USB Controller Identification

```
lsusb
```

### Red flags:

- generic “Flash Disk”
- suspicious controllers
- Chipsbank
- Alcor
- unknown generic vendors

Not proof alone, but useful context.

---

# Additional GOOD commands to add

## 6. Filesystem/Partition Inspection

```
lsblk -f
```

### Red flags:

- partitions appear/disappear
- filesystem missing
- blank FSTYPE
- unstable partition layout

---

## 7. Low-Level Filesystem Detection

```
sudo blkid
```

and:

```
sudo file -s /dev/sda1
```

### Red flags:

- filesystem not detected
- DOS/MBR boot sector only
- inconsistent filesystem signatures

---

## 8. Write Test

```
sudo dd if=/dev/zero of=/dev/sda bs=1M count=10 status=progress
```

### Red flags:

- I/O errors
- disconnects
- extremely slow write speeds

---

# VERY important practical indicator

You mentioned this already and it’s worth keeping:

> “Another red flag is that it doesn't immediately connect to Linux.”

Absolutely true.
