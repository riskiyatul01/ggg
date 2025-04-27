## Pengembangan Streamlit GUI

    - Pustaka yang Digunakan: disini kita menggunakan Streamlit untuk membangun aplikasi web. Streamlit adalah pustaka Python yang memungkinkan pembuatan aplikasi interaktif dengan antarmuka pengguna yang sederhana tanpa memerlukan pengetahuan HTML/CSS.
    - Struktur Dasar GUI:
        - Judul Aplikasi:
          ```
          st.title("Mini-AES 16-bit Encryption Tool 🔒")
          ```
          memberikan judul pada aplikasi.
        - Sidebar dengan Menu Pilihan: Menggunakan st.sidebar.selectbox, pengguna dapat memilih menu utama yang berisi tiga opsi:
            1. Encrypt/Decrypt - Untuk mengenkripsi atau mendekripsi pesan.
            3. Avalanche Effect Test - Untuk menguji efek Avalanche dari algoritma enkripsi.
            4. File Operations - Untuk mengunggah dan mengenkripsi/dekripsi file teks.
    - Input Pengguna:
        - Plaintext dan Key: Pengguna memasukkan plaintext (pesan yang akan dienkripsi) dan key (kunci enkripsi) dalam format hexadecimal menggunakan st.text_input.
        - Mode Operasi: Pengguna memilih mode enkripsi/dekripsi, yaitu ECB atau CBC, menggunakan st.selectbox.
        - IV untuk CBC: Jika mode CBC dipilih, pengguna juga memasukkan Initialization Vector (IV).
          
           cuplikan kode :
          ```      
            plaintext_input = st.text_input("Plaintext (Hex 16-bit)").strip()
            key_input = st.text_input("Key (Hex 16-bit)").strip()
            mode = st.selectbox("Mode Operasi", ["ECB", "CBC"])
            iv_input = st.text_input("Initialization Vector (IV) untuk CBC (hex)", value="0000").strip()
          ```
    - Tombol Aksi:
        - Encrypt: Tombol ini memulai proses enkripsi. Berdasarkan mode yang dipilih (ECB atau CBC), data dienkripsi menggunakan fungsi yang sesuai dari mini_aes.py.
        - Decrypt: Tombol ini memungkinkan pengguna untuk mendekripsi ciphertext yang telah dienkripsi sebelumnya menggunakan mode yang sama (ECB atau CBC).
        - Avalanche Effect: Tombol untuk menguji efek Avalanche dengan mengubah satu bit dari plaintext dan membandingkan perbedaan ciphertext.
        - File Operations: Tombol untuk mengunggah file, mengenkripsi atau mendekripsi file tersebut dengan AES.
    - Feedback untuk Pengguna:
        - Success & Error Messages: Streamlit menyediakan berbagai fungsi seperti st.success(), st.error(), st.warning(), dan st.info() untuk menampilkan pesan hasil aksi pengguna, misalnya hasil enkripsi, kesalahan input, atau status file yang diunggah.
     
          cuplikan kode :
          ```
            decrypted_hex = ''.join(hex(block)[2:].zfill(4) for block in decrypted_blocks)
                st.success(f"Decrypted (hex): {decrypted_hex}")

                decrypted_text = hex_to_text_safe(decrypted_hex)
                if decrypted_text is not None:
                    st.info(f"Decrypted (text): {decrypted_text}")
                else:
                    st.warning("Hasil dekripsi tidak bisa dikonversi ke teks utf-8 yang valid.")
            except Exception as e:
            st.error(f"Error: {e}")
          ```

## Manajemen Antarmuka Pengguna

    - Pengelolaan Status Sesi:
      - st.session_state digunakan untuk menyimpan status antara enkripsi dan dekripsi. Contoh:
        - Setelah enkripsi berhasil, ciphertext disimpan di st.session_state['ciphertext_blocks'], yang kemudian digunakan untuk dekripsi.
        - Key, IV, dan Mode juga disimpan dalam sesi untuk digunakan kembali jika pengguna ingin mendekripsi pesan yang sudah dienkripsi sebelumnya.

          cuplikan kode :
          ```
           st.session_state['ciphertext_blocks'] = ciphertext_blocks
            st.session_state['key'] = key
            st.session_state['iv'] = iv
            st.session_state['mode'] = mode
          ```
    - Fungsi Konversi:
        - to_hex_if_needed(): Fungsi ini memastikan input yang dimasukkan oleh pengguna dalam bentuk teks (plaintext atau kunci) dikonversi menjadi format hexadecimal yang sesuai.
        - hex_to_text_safe(): Fungsi ini digunakan untuk mengonversi ciphertext kembali menjadi teks, jika hasil dekripsi valid.
     
          cuplikan kode : 
          ```
           plaintext_hex = to_hex_if_needed(plaintext_input)
            key_hex = to_hex_if_needed(key_input)
            iv_hex = to_hex_if_needed(iv_input)
          ```
    - Mode Operasi:
        - ECB (Electronic Codebook): Enkripsi dilakukan blok per blok tanpa keterkaitan antara blok.
        - CBC (Cipher Block Chaining): Enkripsi dilakukan dengan menggunakan IV yang menghubungkan blok-blok ciphertext secara berantai.
    - File Operations:
        - Upload File: Pengguna dapat mengunggah file teks yang berisi data yang ingin dienkripsi. File dibaca dan diubah menjadi blok-blok ciphertext setelah enkripsi.
        - Save to File: Setelah proses enkripsi atau dekripsi, hasilnya disimpan ke dalam file baru yang dapat diunduh oleh pengguna.
     
          cuplikan kode : 
            ```
           save_to_file("ciphertext_output.txt", ciphertext_blocks)
            st.success("Ciphertext file saved as ciphertext_output.txt")
            except Exception as e:
            st.error(f"Error: {e}")
          ```

## Fungsi AES (mini_aes.py)

    - AES Encryption: Fungsi mini_aes_encrypt() dan mini_aes_decrypt() mengimplementasikan algoritma AES mini untuk mengenkripsi dan mendekripsi data, menggunakan operasi seperti Substitution (S-box), Shift Rows, Mix Columns, dan Add Round Key.
    - ECB & CBC Mode:
        - ECB: Setiap blok plaintext dienkripsi secara independen.
        - CBC: Setiap blok plaintext di-XOR dengan blok ciphertext sebelumnya untuk meningkatkan keamanan.
        - Avalanche Effect: Fungsi ini menguji seberapa besar perbedaan ciphertext saat satu bit dari plaintext diubah. Hal ini mengukur sensitivitas algoritma terhadap perubahan kecil dalam input, yang merupakan sifat penting dari algoritma kriptografi yang baik.

    cuplikan kode : 
      ```
      def avalanche_effect(plaintext, key):
        cipher1 = mini_aes_encrypt(plaintext, key)
        cipher2 = mini_aes_encrypt(plaintext ^ (1 << 0), key)  # flip 1 bit
        diff = cipher1 ^ cipher2
        return bin(diff).count('1')
       ```
