# [ArchWiki article on QEMU](https://wiki.archlinux.org/title/QEMU)

### 1. First, download an iso file for a desired OS

### 2. Then create a hard-disk image

```
qemu-img create -f format_type image_file.img size
```

where *format_type* stands for either *raw* or *qcow2*, difference being:

- *qcow2* allocates space dynamically, e.g. if I specify *size* as 10G it won't take actually 10G off from my storage immediately, instead it will take up space as the guest OS writes to it
- *raw* allocates all the space it was given, however, it's faster to access by guest OS


### 3. After that mount the iso file to the image file

```
qemu-system-x86_64 -cdrom image.iso -boot order=d -cpu host -m mem_amount -accel kvm -smp cores_amount -device virtio-vga-gl -display display_type,gl=on -monitor stdio -drive file=image_file.img,format=format_type
```

where:

- `-drive` specifies the image file and format
- `-cpu host` means that the host cpu will be emulated
- `-accel kvm` will use kvm as the hypervisor
- `-m` will allocate *mem_amount* of RAM, I think 8G is optimal 
- `-smp` will make *cores_amount* of cores available to guest, I think 8 is optimal
- `-device` will specify the graphics driver, I think virtio is optimal, however Windows will require additional drivers to be installed
- `-display` will use the *display_type* graphics renderer, I use either *sdl* or *gtk*
- `-monitor stdio` will give access to qemu monitor in the same terminal its being run

This step will also boot you into the live installment 

### 4. The last step is to run the OS

```
qemu-system-x86_64 -cpu host -m mem_amount -accel kvm -smp cores_amount -device virtio-vga-gl -display [display],gl=on -monitor stdio -nic user,id=nic0,smb=path/to/shared_dir -drive file=image_file.img,format=format_type -audio driver=driver_name,model=model_name,id=id_name
```

where:

- `-nic user,id=nic0,smb=path/to/shared_dir` will add a shared directory with a specified path to the ip address 10.0.2.4 using Samba
	- `smb` stands for *Samba* and requires it to be installed on the host and *smbclients* installed on the guest. Be sure to check the path to shared folder has the right permissions or else you won't be able to access it. For instance, I made a *Shared* folder in /home/ directory and made it accessible to everybody (rwx permissions), so I won't have to login as any user in the guest OS
- here I set `-display` to *sdl* instead, because it seems to work better with HiDPI than gtk and doesn't require additional scaling configuration
- `-audio` is needed to configure sound output, *driver_name* selects the driver, and *model_name* is the name of sound card, *id_name* can be anything, e.g. *Sound*. Recommended options: for driver select *pipewire* if using Wayland, for model select *virtio* as it's already enabled in *-device* section

### Optionally

To disable networking completely on the guest add switch `-nic none`

### Example

Creating disk image:

```
qemu-img create -f qcow2 vm.img 35G
```

Installing the OS:

```
qemu-system-x86_64 -cdrom os.iso -boot order=d -cpu host -m 8G -accel kvm -smp 8 -device virtio-vga-gl -display gtk,gl=on,zoom-to-fit=off -monitor stdio -drive file=vm.img,format=qcow2
```

Running the OS:

```
qemu-system-x86_64 -cpu host -m 8G -accel kvm -smp 8 -device virtio-vga-gl -display sdl,gl=on -monitor stdio -nic user,id=nic0,smb=shared -audio driver=pipewire,model=virtio,id=Sound -drive file=vm.img,format=qcow2
```
	
P.S. Instead of typing every option each time you want to use QEMU, it's better to create a short bash script and run it instead, e.g.:

```
#!/bin/bash

exec qemu-system-x86_64 -cpu host -m 4G -accel kvm -smp 4 -device virtio-vga-gl -display sdl,gl=on -monitor stdio -nic user,id=nic0,smb=shared -drive file=image_file,format=raw
```

### Creating snapshots

Snapshots are only available for qcow2 formats. To create a snapshot run:

```
qemu-img create -f qcow2 -b original.img -F qcow2 snapshot.img
```

Now any changes to snapshot.img **will not be redirected** to original.img. If you want to preserve the snapshot, **DO NOT** change anything in the original image or else the snapshot image **WILL BE CORRUPTED**. Returning to previous version of the snapshot is **not possible**, so just delete it and create new.

For instance, if I corrupt my system in a snapshot, the original image will be completely unaffected, so I can just delete the snapshot and make a new one to undo everything. However, if my original image is corrupted, then all snapshots will be too.
