# Hasil-laporan-praktikum-OOP

- Latihan 1: Membuat Class Hero
hero1 = Hero("Layla", 500, 15)
hero1.info()
hero2.info()

- Tugas Analisis 1:
• Apa yang terjadi jika kamu mengubah hero1.hp menjadi 500 setelah baris
hero1 = Hero...? Coba lakukan print(hero1.hp)
jawab: "hp hero tersebut akan berubah yang awalnya 100 menjadi 500"

- Latihan 2: Interaksi Antar Objek (Method)
def serang(self, lawan):
 print(f"{self.name} menyerang {lawan.name}!")
 lawan.diserang(self.attack_power)
 
 def diserang(self, damage):
 self.hp -= damage
 print(f"{self.name} terkena damage {damage}. Sisa HP: {self.hp}")

- Tugas Analisis 2:
• Perhatikan parameter lawan pada method serang. Parameter tersebut
menerima sebuah objek utuh, bukan hanya string nama. Mengapa ini
penting?
jawab: "agar bisa mengakases atribut dan mengambil method lawan"

Latihan 3: Pewarisan
class Mage(Hero):
 def __init__(self, name, hp, attack_power, mana):
 # Memanggil constructor milik Parent (Hero)
 super().__init__(name, hp, attack_power)
 self.mana = mana

 print("\n--- Update Class Hero ---")
eudora = Mage("Eudora", 80, 30, 100)

 eudora.info()

- Tugas Analisis 3:
• Eksperimen Fungsi super(): Pada class Mage, coba hapus (atau jadikan
komentar #) baris kode super().__init__(name, hp, attack_power). Kemudian
jalankan programnya.
• Pertanyaan: Error apa yang muncul saat kamu mencoba melihat info Eudora
(eudora.info())? Mengapa error tersebut mengatakan Mage object has no
attribute 'name', padahal kita sudah mengirim nama "Eudora" saat
pembuatan objek?
• Jelaskan peran fungsi super() dalam menghubungkan data dari class Anak ke
class Induk!
jawab: "karena object nya eudora adalah pewarisan dari class Hero, yang dimana dari class Hero itu mengambil name, gp, attack_power jadi kalau di command akan error no attribute, dan peran fungsi super()
       adalah untuk memanggil constructor/method yang ada di class induk"

- Latihan 4: Enkapsulasi

class Hero:
    def __init__(self, nama, hp_awal):
        self.nama = nama
        # Enkapsulasi: HP bersifat Private (hanya bisa diakses di dalam class ini)
        self.__hp = hp_awal
    # GETTER: Cara resmi melihat HP
    def get_hp(self):
        return self.__hp
    # SETTER: Cara resmi mengubah HP (dengan validasi)
    def set_hp(self, nilai_baru):
            self.__hp = nilai_baru
    def diserang(self, damage):
        # Kita pakai setter/getter bahkan di dalam class sendiri agar aman
        sisa_hp = self.get_hp() - damage
        self.set_hp(sisa_hp)
        print(f"{self.nama} terkena damage {damage}. Sisa HP: {self.get_hp()}")
        
# -- Uji Coba --
hero1 = Hero("Layla", 100)
# hero1.__hp = 9999 # GAGAL (Tidak akan mengubah hp asli)
# print(hero1.__hp) # ERROR (Tidak bisa dibaca langsung)
hero1.set_hp(-100) # Coba set negatif
print(hero1.get_hp()) # Output: 0 (Karena dicegat oleh logika Setter)
print(f"Mencoba akses paksa: {hero1._Hero__hp}")

- Tugas Analisis 4:
1. Percobaan Hacking: Coba tambahkan baris kode berikut di bagian paling
bawah (luar class):
print(f"Mencoba akses paksa: {hero1._Hero__hp}")
Pertanyaan: Apakah nilai HP muncul atau Error? Jika muncul, diskusikan dengan
temanmy mengapa Python masih mengizinkan akses ini (konsep Name Mangling)
dan mengapa kita tetap tidak boleh melakukannya dalam standar pemrograman
yang baik.
jawab: "jadi Name Mangling itu mekanisme di Python yang digunakan untuk menyembunyikan atribut private pada sebuah class agar tidak mudah diakses langsung dari luar class, dia itu ga ngehilangin tapi ngubah
       nama variabel, dan tidak boleh karena melanggar konsep enskapsulasi yang harus memakai getter dan setter, dan membuat code susah di kontrol karena bisa di akses bobol dari mana2"
   
2. Uji Validasi: Hapus logika if dan elif di dalam method set_hp, sehingga isinya
hanya self.__hp = nilai_baru.
Pertanyaan: Kemudian lakukan hero1.set_hp(-100).
Apa yang terjadi pada data HP Hero? Jelaskan mengapa keberadaan method
Setter sangat penting untuk menjaga integritas data dalam game!
jawab: "langsung jadi -100 padahal hp game itu gaboleh negatif, karena setter adalah method untuk validasi data jadi seperti sisytem biar game itu hp tidak mencapai negatif"

- Latihan 5: Abstraction & Interface

from abc import ABC, abstractmethod
# 1. Interface / Abstract Class
# Ini adalah KONTRAK. Semua turunan wajib punya method di bawah ini.
class GameUnit(ABC):

 @abstractmethod
 def serang(self, target):
 pass
 @abstractmethod
 def info(self):
 pass
# 2. Implementasi pada Class Konkret
class Hero(GameUnit):
 def __init__(self, nama):
 self.nama = nama

 # Kita WAJIB membuat method serang, kalau tidak akan Error
 def serang(self, target):
 print(f"Hero {self.nama} menebas {target}!")
 def info(self):
 print(f"Saya adalah Hero: {self.nama}")
class Monster(GameUnit):
 def __init__(self, jenis):
 self.jenis = jenis
 # Implementasi serang versi Monster
 def serang(self, target):
 print(f"Monster {self.jenis} menggigit {target}!")
 def info(self):
 print(f"Saya adalah Monster: {self.jenis}")
# -- Uji Coba --
# unit = GameUnit() # ERROR! Abstract class tidak bisa jadi objek.
h = Hero("Alucard")
m = Monster("Serigala")
h.info()
m.info()

- Tugas Analisis 5:
1. Melanggar Kontrak: Pada class Hero, hapus (atau jadikan komentar #) seluruh
blok method def serang(self, target):. Jalankan programnya.
Pertanyaan: Error apa yang muncul? Jelaskan dengan bahasamu sendiri, apa arti
pesan error Can't instantiate abstract class Hero with abstract
method...?
Apa konsekuensinya jika kita lupa membuat method yang sudah dijanjikan di
Interface?
jawab: "jadi arti error tersebut ialah class Hero itu belum memenuhi janji dengan abstrak class GameUnit, nah jadi python itu mikir ini belum bisa jadi objek karena masih bersifat abstrak,
       dan konsenkuensinya adalah codingan bakal error dan class yang dimau tidak akan menjadi objek"

2. Mencetak Cetakan: Coba aktifkan baris kode unit = GameUnit().
Pertanyaan: Mengapa class GameUnit dilarang untuk dibuat menjadi objek?
Apa gunanya ada class GameUnit jika tidak bisa dibuat menjadi objek nyata?
jawab: "karena GameUnit itu seperti template untuk class lain, dan dia itu biasanya tidak punya isi yang gabisa dipakai secara langsung, dan guna dari GameUnit itu
       sebagai aturan jadi semua class dan keturubnan harus memiliki isi yang sama seperti template nya, dan menjaga struktur pemorgraman menjadi konsisten"

- Latihan 6: Polymorphism

# Parent Class
# Parent Class
class Hero:
    def __init__(self, nama):
        self.nama = nama

    def serang(self):
        print("Hero menyerang dengan tangan kosong.")
        
# Child Class 1
class Mage(Hero):
    def serang(self):
        print(f"{self.nama} (Mage) menembakkan Bola Api! Boom!")
        
# Child Class 2
class Archer(Hero):
    def tembak_panah(self):
        print(f"{self.nama} (Archer) memanah dari jauh! Jleb!")
        
# Child Class 3
class Fighter(Hero):
    def serang(self):
        print(f"{self.nama} (Fighter) memukul dengan pedang! Slash!")
        
class Healer(Hero):
    def serang(self):
         print(f"{self.nama} (Healer) tidak menyerang, tapi menyembuhkan teman!")
        
# -- Penerapan Polymorphism --
# Kita punya daftar hero campuran
pasukan = [
    Mage("Eudora"),
    Archer("Miya"),
    Fighter("Zilong"),
    Mage("Gord"),
    Healer("Angela")
]

print("--- PERANG DIMULAI ---")
# Satu perintah loop, tapi respon berbeda-beda (Polymorphism)
for pahlawan in pasukan:
    pahlawan.serang()

- Tugas Analisis 6:
1. Uji Skalabilitas (Kemudahan Menambah Fitur): Tanpa mengubah satu huruf
pun pada kode Looping (for pahlawan in pasukan:), buatlah satu class
baru bernama Healer(Hero).
Isi method serang milik Healer dengan: print(f"{self.name} tidak
menyerang, tapi menyembuhkan teman!").
Masukkan objek Healer ke dalam list pasukan.
o Pertanyaan: Apakah program berjalan lancar?
o Kesimpulannya, apa keuntungan Polimorfisme bagi seorang programmer
ketika harus mengupdate game dengan karakter baru di masa depan?
jawab: "ya, program masih berjalan lancar meski kita menambah class baru Healer code looping ngga perlu diubah karena method nya masih sama serang(), kesimpulannya membuat kode lebih
       fleksibel dan scalable, dan membuat kita mudah menambah kode baru tanpa mengubah kode lama"

2. Konsistensi Penamaan: Ubah nama method serang pada class Archer
menjadi tembak_panah. Jalankan program.
Pertanyaan: Apa yang terjadi?
Mengapa dalam konsep Polimorfisme, nama method antara Parent Class dan
berbagai Child Class harus persis sama?
jawab: "error "AttributeError: 'Archer' object has no attribute 'serang'", Dalam konsep Polimorfisme, semua class turunan harus memiliki nama method yang sama
        agar dapat dipanggil dengan cara yang sama"


       

