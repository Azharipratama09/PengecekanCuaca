# PengecekanCuaca
 Tugas6-Muhammad Azhari Nur Pratama-2210010326
## 1. Deskripsi Program:
• Integrasi dengan API cuaca eksternal (misalnya OpenWeatherMap) untuk
mendapatkan data cuaca secara real-time
• Tampilkan data cuaca dalam bentuk gambar berdasarkan kondisi cuaca
(cerah, berawan, hujan, dan sebagainya)

## 2. Komponen GUI: 
JFrame, JPanel, JLabel, JTextField, JButton, JComboBox
## 3. Logika Program: 
API eksternal, penampilan gambar
## 4. Events:
• ActionListener untuk tombol Cek Cuaca
~~~
 private void jButton3ActionPerformed(java.awt.event.ActionEvent evt) {                                         
            // Action untuk tombol "Cek Cuaca"
     String cityName = jTextField1.getText();
        if (!cityName.isEmpty()) {
         try {
             tampilkanCuaca(cityName);
             
             boolean exists = false;
             for (int i = 0; i < jComboBox1.getItemCount(); i++) {
                 if (jComboBox1.getItemAt(i).equalsIgnoreCase(cityName)) {
                     exists = true;
                     break;
                 }
             }
             
             if (!exists) {
                 jComboBox1.addItem(cityName);
                 simpanKotaKeFile(); // Simpan kota ke file setelah penambahan
             }
         } catch (JSONException ex) {
             Logger.getLogger(CekCuaca.class.getName()).log(Level.SEVERE, null, ex);
         }
        } else {
            JOptionPane.showMessageDialog(this, "Silakan masukkan nama kota!");
        }
    }       
~~~
• ItemListener pada JComboBox untuk memilih lokasi cuaca
~~~
 private void jComboBox1ItemStateChanged(java.awt.event.ItemEvent evt) {                                            
        if (evt.getStateChange() == ItemEvent.SELECTED) {
            String selectedCity = (String) jComboBox1.getSelectedItem();
            jTextField1.setText(selectedCity); // Mengisi txtCityName dengan kota yang dipilih
        }
    }  
~~~
## 5. Variasi:
• Tambahkan kemampuan untuk menyimpan kota yang sering dicek ke dalam daftar lokasi favorit, sehingga dapat dipilih dengan cepat dari JComboBox
~~~
 private void jButton5ActionPerformed(java.awt.event.ActionEvent evt) {                                         
       String cityName = jTextField1.getText();
        String weatherDescription = jLabel9.getText();
        String temperature = jLabel11.getText();

        if (!cityName.isEmpty() && !weatherDescription.isEmpty() && !temperature.isEmpty()) {
            simpanDataCuacaKeFile(cityName, weatherDescription, Double.parseDouble(temperature.replace("°C", "")));
        } else {
            JOptionPane.showMessageDialog(this, "Data cuaca tidak lengkap. Pastikan semua data sudah terisi.");
        }
    }   
~~~
• Sediakan tombol untuk menyimpan data dari tabel ke dalam file CSV atau teks agar data cuaca dapat diakses kembali
~~~
      private void muatKotaDariFile() {
        try
            (BufferedReader reader = new BufferedReader(new FileReader("cities.txt"))) {
            String city;
            while ((city = reader.readLine()) != null) {
                jComboBox1.addItem(city);
            }
        } catch (IOException e) {
            // Jika file tidak ditemukan, abaikan saja
            System.out.println("No saved cities found.");
        }
    }
     private void simpanDataCuacaKeFile(String cityName, String weatherDescription, double temperature) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("weather_data.csv", true))) {
            // Menuliskan data dalam format CSV
            writer.write(cityName + "," + weatherDescription + "," + temperature + "°C");
            writer.newLine();
            JOptionPane.showMessageDialog(this, "Data cuaca berhasil disimpan!");
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "Terjadi kesalahan saat menyimpan data cuaca.");
            e.printStackTrace();
        }
    }
~~~
• Tambahkan fitur untuk memuat data cuaca yang tersimpan dan menampilkannya di JTable.
~~~
  private void jButton4ActionPerformed(java.awt.event.ActionEvent evt) {                                         
       tableModel.setRowCount(0); // Hapus data lama di tabel sebelum memuat data baru

        try (BufferedReader reader = new BufferedReader(new FileReader("weather_data.csv"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] data = line.split(",");
                if (data.length == 3) {
                    // Menambahkan data ke dalam tabel
                    tableModel.addRow(new Object[]{data[0], data[1], data[2]});
                }
            }
            JOptionPane.showMessageDialog(this, "Data cuaca berhasil dimuat ke tabel.");
        } catch (IOException e) {
            JOptionPane.showMessageDialog(this, "Terjadi kesalahan saat memuat data cuaca.");
            e.printStackTrace();
        }    
    }
~~~

## Hasil Setelah Di Run
![Cuplikan layar 2024-11-15 150051](https://github.com/user-attachments/assets/da870d34-e19a-401b-b387-cf8d206eb1a3)

## Indikator Penilaian:
| No  | Komponen         |  Persentase  |
| :-: | --------------   |   :-----:    |
|  1  | Komponen GUI     |    10    |
|  2  | Logika Program   |    20    |
|  3  | EVENT            |    10    |
|  4  | Kesesuaian UI     |    20    |
|  5  | Memenuhi Variasi |    40    |
|     | TOTAL        | 100 |


## Pembuat
Nama  : Muhammad Azhari Nur Pratama
NPM   : 2210010326
