<div align='center'>

# **Panduan Instalasi virtualbox**

# **Wilma Auraruna Khalif - A11.2024.15841**

</div>

## Installasi VM menggunakan Arch linux

Langkah pertama adalah menggunakan _package manager Arch_ 'pacman' untuk menginstal paket `virtualbox`

```bash
sudo pacman -S virtualbox
```

![maaf gagal](./img/installing.png)

## Konfigurasi Kernel dan Hak Akses Pengguna

### Kernel

Kernel adalah suatu media pusat sistem operasi untuk menjebatani antara hardware dan software.

Virtualbox memerlukan kernel aga bisa berinteraksi dengan hardware komputer.

```bash
sudo modprobe vboxdrv
```

- **modprobe** adalah perintah linux yang digunakan untuk menambah atau menghapus modul dari kernel Linux.

- **vboxdrv** adalah nama modul driver utama virtualbox.

> [!NOTE]
> Perintah ini akan memuat driver VirtualBox ke dalam _kernel_ yang memerlukan kernel aga bisa berinteraksi dengan hardware komputer.sedang berjalan, sehingga virtualbox dapat berfungsi.

### Group

Untuk mengizinkan pengguna biasa menjalankan virtualbox, pengguna harus menjadi anggota dari grup khusus.

```bash
sudo usermod -aG vboxusers $USER
```

- **usermod:** Modifikasi user yang sudah ada. Menambahkan user ke dalam grup sudo agar bisa menjalankan perintah sebagai root.
- **aG:** Artinya (-a) adalah _append_ pengguna ke group (-G).
- **vboxusers:** Nama grup khusus virtualbox.
- **$USER:** Singkatan praktis untuk nama pengguna yang sedang aktif.

![maaf gagal](./img/kernelDanGroup.png)

## Menjalankan Virtualbox

![MaafTidakBisa](./openVirtual.png)

## Instalasi Ubuntu

Sebelum itu, kita membutuhkan iso ubuntu terlebih dahulu. **Berikut Link untuk mendownload:**

```link
https://ubuntu.com/download/desktop
```

Setelah itu tekan **Ctrl + N** untuk membuat VM yang baru.

![MaafTidakBisa](./img/memilihIso.png)

- Pilih ISO image yang sudah di download.
- Namai dengan ubuntu

### Set up Unattended guest OS installation

![MaafTidakBisa](./img/setUp.png)
Samakan saja seperti yang di gambar.

### Set Up Hardware Untuk VM

Spesifikasi minimum yang direkomendasikan ubuntu, yaitu 25 Gb storage, RAM 2 GB (disarankan 4 GB atau lebih), dan CPU dual-core 64 bit.

![MaafTidakBisa](./img/hardware2.png)
Setelah itu press next lalu finish.

### Install Ubuntu

**Step Terakhir** yaitu melakukan instalasi ubuntu dari ubuntu installer.

## ![MaafTidakBisa](./img/installUbuntu.png)

### Ubuntu VM sudah siap digunakan

![MaafTidakBisa](./img/openUbuntu2.png)

## Network Configuration

### NAT

---

**Nat** adalah cara paling simpel untuk mengakses jaringan eksternal dari _virtual machine_. **Gampangnya** ketika VM meminta akses internet, **host** akan menerjemahkan perintah itu ke router.

### Cara check adapter:

1. Click ubuntu lalu shortcut `ctrl + S`

![MaafTidakBisa](./img/machines.png)

2. Pilih Network

Virtualbox otomatis akan menggunakan NAT sebagai default adapter.
![MaafTidakBisa](./img/nat.png)
