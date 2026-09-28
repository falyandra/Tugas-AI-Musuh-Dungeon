# Algoritma Pergerakan Enemy di Dungeon

## Identifikasi Algoritma
Algoritma yang digunakan adalah kombinasi dari:
1. **Finite State Machine (FSM):** Sebagai "otak" untuk menentukan kapan musuh diam, mengejar, atau menyerang.
2. **Breadth-First Search (BFS) / A-Star:** Sebagai pencari rute (pathfinding) agar musuh bisa berjalan menghindari tembok menuju pemain.

---

## Flowchart Algoritma Pencari Rute (Versi Simpel)
*(Berikut adalah logika sederhana saat musuh mencari jalan ke arah pemain)*

```plantuml
@startuml
start
:Mulai dari posisi Musuh;

repeat
    :Pilih rute yang perkiraannya
    **paling cepat** sampai ke Pemain;

    if (Apakah sudah sampai ke Pemain?) then (Ya)
        :**RUTE DITEMUKAN!**
        Musuh siap mengejar;
        stop
    else (Belum)
        :Lihat kotak di sekitarnya 
        (Atas, Bawah, Kiri, Kanan);

        if (Apakah ada jalan yang bisa dilewati?) then (Ada)
            :Ingat jalan-jalan baru ini 
            sebagai pilihan rute selanjutnya;
        else (Hanya Tembok/Mentok)
            :Abaikan, rute ini buntu;
        endif
    endif

repeat while (Masih ada pilihan rute lain?) is (Ya)

:**JALUR BUNTU!**
(Musuh tidak bisa mencapai Pemain);
stop
@enduml

# Peta Dungeon sederhana: 0 = Jalan, 1 = Tembok
peta = [
    [0, 0, 0, 0],
    [0, 1, 1, 0],
    [0, 0, 1, 0],
    [0, 0, 0, 0]
]

def musuh_cari_jalan(posisi_musuh, posisi_pemain):
    antrian_rute = [[posisi_musuh]]  
    sudah_dilewati = set()

    while len(antrian_rute) > 0:
        rute_sekarang = antrian_rute.pop(0)  
        posisi_saat_ini = rute_sekarang[-1] 

        if posisi_saat_ini == posisi_pemain:
            return rute_sekarang 

        if posisi_saat_ini not in sudah_dilewati:
            sudah_dilewati.add(posisi_saat_ini)
            baris, kolom = posisi_saat_ini

            arah_sekitar = [(baris, kolom + 1), (baris, kolom - 1), (baris + 1, kolom), (baris - 1, kolom)]

            for b, k in arah_sekitar:
                if 0 <= b < 4 and 0 <= k < 4 and peta[b][k] == 0:
                    rute_baru = list(rute_sekarang)
                    rute_baru.append((b, k))
                    antrian_rute.append(rute_baru)

    return "Jalan Buntu!"

# Jalankan Kode
musuh = (0, 0)
pemain = (3, 3)
print("Langkah musuh:", musuh_cari_jalan(musuh, pemain))
