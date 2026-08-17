# Тестовый лабораторный стенд "Обмен информацией между ЛИС и приборами"

Тестовый стенд реализован через Proxmox VE

Создаём виртуальную машину на Ubuntu Server 24.04 LTS с помощью qm

```bash
cd /var/lib/vz/template
wget https://releases.ubuntu.com/24.04/ubuntu-24.04.3-live-server-amd64.iso
ls -lh /var/lib/vz/template/iso/
```

Для ЛИС используем ВМ на 2 ядра и 4 Гб ОЗУ

```bash
qm create 101 \
  --name lab-lis \
  --memory 4096 \
  --balloon 0 \
  --cores 2 \
  --cpu host \
  --numa 1 \
  --net0 virtio,bridge=vmbr0 \
  --scsihw virtio-scsi-single \
  --scsi0 local-lvm:40,discard=on,ssd=1 \
  --ide2 local:iso/ubuntu-24.04.3-live-server-amd64.iso,media=cdrom \
  --boot order=scsi0;ide2 \
  --ostype l26 \
  --agent enabled=1,fstrim_cloned_disks=1 \
  --start 0
```

