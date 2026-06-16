import random

# 1. Masalah yang mau diselesaikan: Cari urutan angka terbaik
angka = [10, -5, 20, 30, -15]

# Fungsi untuk menghitung "skor" dari sebuah susunan angka
# Makin besar skornya, makin bagus susunannya (ini tujuan optimasinya)
def hitung_skor(susunan):
    skor = 0
    for indeks, val in enumerate(susunan):
        skor += val * indeks  # Rumus nyari skor: angka dikali posisi indeksnya
    return skor

# 2. Setup Awal Tabu Search
solusi_sekarang = angka.copy()
skor_sekarang = hitung_skor(solusi_sekarang)

solusi_terbaik = solusi_sekarang.copy()
skor_terbaik = skor_sekarang

# Membuat TABU LIST (Daftar Larangan) berbentuk Dictionary
# Formatnya -> angka: jumlah_durasi_dilarang
tabu_list = {}
durasi_tabu = 3  # Angka yang ditukar bakal dilarang selama 3 putaran

print("Susunan Awal :", solusi_sekarang, " | Skor:", skor_sekarang)
print("-" * 60)

# 3. Proses Pencarian (Searching) sebanyak 10 Putaran/Iterasi
for iterasi in range(1, 11):
    
    # Kurangi masa hukuman angka yang ada di Tabu List di setiap putaran
    for kunci in list(tabu_list.keys()):
        tabu_list[kunci] -= 1
        if tabu_list[kunci] <= 0:
            del tabu_list[kunci] # Hapus dari daftar larangan kalau hukumannya habis

    # Cari tetangga (coba acak/tukar 2 posisi angka secara acak)
    idx1, idx2 = random.sample(range(len(angka)), 2)
    angka1, angka2 = solusi_sekarang[idx1], solusi_sekarang[idx2]
    
    # Cek apakah angka yang mau ditukar lagi dihukum (TABU)?
    if angka1 in tabu_list or angka2 in tabu_list:
        # SKIP/Jangan lakukan penukaran kalau salah satu angka berstatus TABU
        continue 
        
    # Kalau TIDAK TABU, mari kita coba tukar posisinya (Langkah/Move)
    solusi_tentative = solusi_sekarang.copy()
    solusi_tentative[idx1], solusi_tentative[idx2] = solusi_tentative[idx2], solusi_tentative[idx1]
    skor_tentative = hitung_skor(solusi_tentative)
    
    # Berpindah ke solusi baru tersebut (meskipun skornya bisa lebih buruk, ini ciri khas TS!)
    solusi_sekarang = solusi_tentative.copy()
    skor_sekarang = skor_tentative
    
    # Catat kedua angka yang barusan ditukar ke dalam TABU LIST
    tabu_list[angka1] = durasi_tabu
    tabu_list[angka2] = durasi_tabu
    
    # Simpan susunan terbaik yang pernah ditemukan selama perjalanan
    if skor_sekarang > skor_terbaik:
        solusi_terbaik = solusi_sekarang.copy()
        skor_terbaik = skor_sekarang
        
    print(f"Iterasi {iterasi} : Dicoba tukar indeks {idx1} & {idx2} -> {solusi_sekarang} | Skor: {skor_sekarang}")

print("-" * 60)
print("SUSUNAN TERBAIK HASIL TABU SEARCH :", solusi_terbaik, " | Skor Maksimal:", skor_terbaik)
