# Ujian_Akhir_Semester_Pemrograman_Berorientasi_Objek
PROGRAM SEDERHANA APLIKASI NOTEPAD

# Penjelasan Kode
```py
import tkinter as tk                                      # Mengimpor modul tkinter dengan alias tk
from tkinter import filedialog, messagebox, colorchooser  # Mengimpor beberapa modul dari tkinter

class Notepad:
    def __init__(self, root):
        self.root = root
        self.root.title("Notepad")                     # Menetapkan judul jendela sebagai "Notepad"
        self.text_area = tk.Text(self.root)            # Membuat widget Text dan menambahkannya ke jendela
        self.text_area.pack(expand=True, fill='both')  # Mengatur widget Text agar memenuhi jendela
        self.create_menu()                             # Memanggil metode create_menu

    def create_menu(self):
        menu_bar = tk.Menu(self.root)                                # Membuat menu bar
        file_menu = tk.Menu(menu_bar, tearoff=0)                     # Membuat sub-menu File
        file_menu.add_command(label="New", command=self.new_file)    # Menambahkan opsi New ke sub-menu File
        file_menu.add_command(label="Open", command=self.open_file)  # Menambahkan opsi Open ke sub-menu File
        file_menu.add_command(label="Save", command=self.save_file)  # Menambahkan opsi Save ke sub-menu File
        file_menu.add_separator()                                    # Menambahkan garis pemisah di sub-menu File
        file_menu.add_command(label="Exit", command=self.exit_app)   # Menambahkan opsi Exit ke sub-menu File
        menu_bar.add_cascade(label="File", menu=file_menu)           # Menambahkan sub-menu File ke menu bar

        format_menu = tk.Menu(menu_bar, tearoff=0)                                    # Membuat sub-menu Format
        format_menu.add_command(label="Font Color", command=self.change_font_color)   # Menambahkan opsi Font Color ke sub-menu Format
        format_menu.add_command(label="Bold", command=self.make_text_bold)            # Menambahkan opsi Bold ke sub-menu Format
        format_menu.add_command(label="Italic", command=self.make_text_italic)        # Menambahkan opsi Italic ke sub-menu Format
        format_menu.add_command(label="Underline", command=self.make_text_underline)  # Menambahkan opsi Underline ke sub-menu Format
        menu_bar.add_cascade(label="Format", menu=format_menu)                        # Menambahkan sub-menu Format ke menu bar

        self.root.config(menu=menu_bar)                                               # Mengatur menu bar sebagai menu utama pada jendela

    def new_file(self):
        self.text_area.delete(1.0, tk.END)                                            # Menghapus konten teks di dalam widget Text

    def open_file(self):
        file_path = filedialog.askopenfilename(filetypes=[("Text Files", ".txt"), ("All Files", ".*")])  # Meminta pengguna untuk memilih file yang akan dibuka
        if file_path:
            try:
                with open(file_path, 'r') as file:                # Membuka file teks dalam mode membaca
                    self.text_area.delete(1.0, tk.END)            # Menghapus konten teks di dalam widget Text
                    self.text_area.insert(tk.END, file.read())    # Menampilkan isi file di dalam widget Text
            except Exception as e:
                messagebox.showerror("Error", str(e))             # Menampilkan pesan kesalahan jika terjadi kesalahan

    def save_file(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".txt", filetypes=[("Text Files", "*.txt")])  # Meminta pengguna untuk memilih lokasi dan nama file untuk disimpan
        if file_path:
            try:
                with open(file_path, 'w') as file:                  # Membuka file teks dalam mode menulis
                    text_content = self.text_area.get(1.0, tk.END)  # Mengambil konten teks dari widget Text
                    file.write(text_content)                        # Menyimpan konten teks ke dalam file
            except Exception as e:
                messagebox.showerror("Error", str(e))               # Menampilkan pesan kesalahan jika terjadi kesalahan

    def exit_app(self):
        if messagebox.askokcancel("Quit", "Are you sure you want to quit?"):  # Menampilkan dialog konfirmasi sebelum keluar
            self.root.destroy()                                               # Menutup jendela aplikasi

    def change_font_color(self):
        color = colorchooser.askcolor(title="Select Font Color")  # Meminta pengguna untuk memilih warna font
        if color[1]:
            self.text_area.config(fg=color[1])                    # Mengubah warna font di dalam widget Text

    def make_text_bold(self):
        current_tags = self.text_area.tag_names("sel.first")               # Mendapatkan daftar tag yang ada pada teks yang dipilih
        if "bold" in current_tags:                                         # Jika tag "bold" sudah ada pada teks yang dipilih
            self.text_area.tag_remove("bold", "sel.first", "sel.last")     # Menghapus tag "bold" dari teks yang dipilih
        else:
            self.text_area.tag_add("bold", "sel.first", "sel.last")        # Menambahkan tag "bold" ke teks yang dipilih
            self.text_area.tag_config("bold", font=("Arial", 12, "bold"))  # Mengonfigurasi tag "bold" dengan font yang sesuai

    def make_text_italic(self):
        current_tags = self.text_area.tag_names("sel.first")               # Mendapatkan daftar tag yang ada pada teks yang dipilih
        if "italic" in current_tags:                                       # Jika tag "italic" sudah ada pada teks yang dipilih
            self.text_area.tag_remove("italic", "sel.first", "sel.last")   # Menghapus tag "italic" dari teks yang dipilih
        else:
            self.text_area.tag_add("italic", "sel.first", "sel.last")          # Menambahkan tag "italic" ke teks yang dipilih
            self.text_area.tag_config("italic", font=("Arial", 12, "italic"))  # Mengonfigurasi tag "italic" dengan font yang sesuai

    def make_text_underline(self):
        current_tags = self.text_area.tag_names("sel.first")                 # Mendapatkan daftar tag yang ada pada teks yang dipilih
        if "underline" in current_tags:                                      # Jika tag "underline" sudah ada pada teks yang dipilih
            self.text_area.tag_remove("underline", "sel.first", "sel.last")  # Menghapus tag "underline" dari teks yang dipilih
        else:
            self.text_area.tag_add("underline", "sel.first", "sel.last")     # Menambahkan tag "underline" ke teks yang dipilih
            self.text_area.tag_config("underline", underline=True)           # Mengonfigurasi tag "underline" dengan pengaturan underline yang benar

if __name__ == '__main__':
    root = tk.Tk()                  # Membuat jendela aplikasi
    notepad = Notepad(root)         # Membuat objek Notepad
    root.mainloop()                 # Menjalankan siklus utama aplikasi (event loop) untuk menampilkan jendela dan merespons interaksi pengguna
```


    Kode program diatas adalah implementasi sederhana dari aplikasi Notepad menggunakan modul tkinter dalam Python. Aplikasi Notepad ini memungkinkan pengguna untuk membuat teks dalam bentuk file, membuka file, menyimpan file, dan mengedit teks dengan menu format sederhana seperti perubahan warna font, cetakan tebal, miring, dan garis bawah.
Di dalam kelas Notepad, terdapat beberapa fungsi untuk mengatur fungsi-fungsi aplikasi Notepad diatarannya adalah create_menu(): fungsi  ini digunakan untuk membuat menu-bar aplikasi dengan menu-file dan menu-format.  new_file(): fungsi ini digunakan untuk menghapus isi area teks. open_file(): Fungsi ini digunakan untuk membuka file teks yang ada dan menampilkan isinya di area teks. save_file(): fungsi ini digunakan untuk menyimpan isi area teks ke dalam file. exit_app(): fetode ini digunakan untuk mengakhiri aplikasi Notepad. change_font_color(): fungsi ini digunakan untuk mengubah warna font pada teks yang dipilih. make_text_bold():fungsi ini digunakan untuk memberi atau menghapus format cetakan tebal pada teks yang dipilih. make_text_italic(): fungsi ini digunakan untuk memberi atau menghapus format cetakan miring pada teks yang dipilih. make_text_underline(): fungsi ini digunakan untuk memberi atau menghapus format garis bawah pada teks yang dipilih.
Di luar kelas Notepad, program utama dimulai dengan membuat instance dari kelas Tkinter (root window) dan Notepad. Dalam program ini, root window adalah jendela utama aplikasi Notepad yang dibuat menggunakan modul tkinter. Kemudian, instance dari kelas Notepad dibuat dengan memberikan root window sebagai argumen untuk menghubungkannya dengan jendela utama. Terakhir, program utama dijalankan dengan memanggil method mainloop() pada root untuk menjalankan aplikasi Notepad dan menunggu interaksi pengguna.
    Kemudian pada kode program tersebut  pengimplementasian konsep OOP dalam bahasa pemrograman Python menggunakan pustaka Tkinter. Berikut adalah beberapa konsep OOP yang dapat ditemukan dalam kode tersebut:
    Kelas class Notepad: Dalam OOP, class adalah blueprint atau template untuk membuat objek. Di sini, Notepad adalah kelas yang menggambarkan notepad atau aplikasi editor teks. Objek notepad = Notepad(root): adalah instansi dari sebuah kelas. 
    Inheritance adalah konsep di mana sebuah kelas dapat mewarisi atribut dan fungsi  dari kelas lain. Dalam contoh ini, kelas Notepad mewarisi sifat dan fungsi dari kelas tkinter. Tkinter melalui pewarisan class Notepad(root). Fungsi def __init__(self, root), def create_menu(self), def new_file(self), def open_file(self), def save_file(self), def exit_app(self), def change_font_color(self), def make_text_bold(self), def make_text_italic(self), def make_tekt_underline(self): Fungsi tersebut adalah fungsi yang terkait dengan sebuah kelas dan dapat dijalankan pada objek kelas tersebut. Dalam kode tersebut, terdapat beberapa metode seperti __init__, create_menu, new_file, open_file, save_file, exit_app, change_font_color, make_text_bold, make_text_italic, make_tekt_underline yang menggambarkan perilaku yang terkait dengan notepad.
   Konsep encapsulation melibatkan penggabungan data dan perilaku terkait dalam sebuah kelas. Dalam kode tersebut, atribut seperti root dan text_area dienkapsulasi ke dalam kelas Notepad, sehingga menjadi bagian dari objek yang dibuat.
   Polymorphism adalah kemampuan untuk menggunakan fungsi dengan nama yang sama dalam banyak kelas. Dalam kode tersebut, metode-metode seperti make_text_bold, make_text_italic, dan make_text_underline menggunakan nama yang sama, tetapi memiliki perilaku yang berbeda saat dipanggil.
    Abstraction melibatkan penyembunyian detail yang tidak relevan dari pengguna dan hanya mengekspos fungsionalitas yang diperlukan. Dalam kode tersebut, pengguna tidak perlu mengetahui detail implementasi internal seperti manipulasi tag pada text_area, tetapi cukup menggunakan antarmuka yang disediakan oleh metode-metode seperti make_text_bold dan make_text_italic.
    GUI Programming: Tkinter adalah pustaka GUI (Graphical User Interface) yang digunakan dalam kode tersebut. Dengan menggunakan OOP, kode tersebut mengorganisir antarmuka pengguna dalam kelas Notepad dan metode terkaitnya.
    Dengan menggunakan konsep-konsep OOP, kode tersebut lebih terstruktur, modular, dan dapat dengan mudah dikelola dan dikembangkan. Dan dengan konsep tersebut maka kode program dapat menghasilkan aplikasi notepad yang memiliki berbagai fitur seperti new, open, save, exit, font color, bold, italic, dan underline. 
