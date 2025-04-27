1. Pengembangan Streamlit GUI
- Pustaka yang Digunakan: disini kita menggunakan Streamlit untuk membangun aplikasi web. Streamlit adalah pustaka Python yang memungkinkan pembuatan aplikasi interaktif dengan antarmuka pengguna yang sederhana tanpa memerlukan pengetahuan HTML/CSS.

- Struktur Dasar GUI:
    - Judul Aplikasi:
      
      ![image](https://github.com/user-attachments/assets/e5718ae6-0d3a-4584-b63f-d8311fc42504)
      
      memberikan judul pada aplikasi

    - Sidebar Menu Pilihan: Menggunakan st.sidebar.selectbox untuk menampilkan menu yang memungkinkan pengguna memilih mode aplikasi (Encrypt/Decrypt, Avalanche Effect Test, File Operations).
    - 
      ![image](https://github.com/user-attachments/assets/623785ca-3b45-4712-bc11-216c129979e2)

    - Enkripsi dan Dekripsi (Encrypt/Decrypt):
      1. Pengguna memasukkan plaintext, key, dan memilih mode (ECB atau CBC).
      2. Jika mode CBC dipilih, pengguna juga harus memasukkan IV (Initialization Vector).
      3. Tombol Encrypt dan Decrypt digunakan untuk memulai enkripsi atau dekripsi.
         
        ![image](https://github.com/user-attachments/assets/99a95942-42b7-45d5-8d6e-efd8108ed714)
      
        ![image](https://github.com/user-attachments/assets/14d74b2a-ab31-4bce-9a22-df3a7a539f3f)
      
        ![image](https://github.com/user-attachments/assets/71b417df-024b-49af-9819-1a03114fb4fc)

- Avalanche Effect:
    - Pengguna memasukkan plaintext dan key untuk menguji efek Avalanche, yang mengukur berapa banyak bit yang berubah jika satu bit dari plaintext diganti.
      
        ![image](https://github.com/user-attachments/assets/7d31b909-f740-45fd-b880-8e1d981620be)\
      
- File Operations:
  - Mengunggah file dan mengenkripsi/dekrip file tersebut.

    ![image](https://github.com/user-attachments/assets/f4d8309c-21fb-4783-b688-d52b87cabed2)

    ![image](https://github.com/user-attachments/assets/32af690a-5286-4488-98af-0df16c1ff715)

