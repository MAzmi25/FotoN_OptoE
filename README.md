<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Fotonika?", "id": "Ilmu Dan Teknologi Yang Berkaitan Dengan Cahaya." },
  { "en": "Apa Itu Optoelektronika?", "id": "Studi Perangkat Elektronik Yang Berinteraksi Dengan Cahaya." },
  { "en": "Apa Partikel Dasar Cahaya?", "id": "Foton." },
  { "en": "Apa Sifat Dualisme Cahaya?", "id": "Dapat Bertindak Sebagai Gelombang Dan Partikel." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Jenis Radiasi Elektromagnetik." },
  { "en": "Berapa Kecepatan Cahaya Di Ruang Hampa?", "id": "Sekitar 299,792 Kilometer Per Detik." },
  { "en": "Apa Itu Panjang Gelombang?", "id": "Jarak Antara Puncak Gelombang Yang Berurutan." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Getaran Gelombang Per Detik." },
  { "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Hubungan Antara Panjang Gelombang Dan Frekuensi?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Apa Itu Indeks Bias?", "id": "Ukuran Seberapa Cepat Cahaya Melewati Material." },
  { "en": "Apa Hukum Pemantulan Cahaya?", "id": "Sudut Datang Sama Dengan Sudut Pantul." },
  { "en": "Apa Hukum Pembiasan Cahaya (Hukum Snellius)?", "id": "Menjelaskan Pembelokan Cahaya Antar Media." },
  { "en": "Apa Itu Dispersi?", "id": "Pemisahan Cahaya Putih Menjadi Warna Komponennya." },
  { "en": "Apa Itu Difraksi?", "id": "Pembelokan Gelombang Saat Melewati Celah." },
  { "en": "Apa Itu Interferensi?", "id": "Kombinasi Dua Atau Lebih Gelombang." },
  { "en": "Apa Itu Polarisasi?", "id": "Orientasi Osilasi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Laser?", "id": "Sumber Cahaya Koheren Dan Monokromatik." },
  { "en": "Apa Kepanjangan Dari LASER?", "id": "Light Amplification by Stimulated Emission of Radiation." },
  { "en": "Apa Itu Emisi Spontan?", "id": "Elektron Melepaskan Foton Tanpa Stimulasi Eksternal." },
  { "en": "Apa Itu Emisi Terstimulasi?", "id": "Foton Masuk Menyebabkan Pelepasan Foton Identik." },
  { "en": "Apa Itu Inversi Populasi?", "id": "Kondisi Dimana Lebih Banyak Elektron Berenergi Tinggi." },
  { "en": "Apa Tiga Komponen Utama Laser?", "id": "Media Penguat, Sumber Pompa, Resonator Optik." },
  { "en": "Apa Itu Dioda Pemancar Cahaya (Light Emitting Diode)?", "id": "Perangkat Semikonduktor Yang Memancarkan Cahaya." },
  { "en": "Apa Bahan Utama Pembuatan LED?", "id": "Bahan Semikonduktor Seperti Galium Arsenida." },
  { "en": "Apa Itu Elektroluminesensi?", "id": "Fenomena Emisi Cahaya Dari Bahan." },
  { "en": "Apa Keuntungan LED Dibanding Lampu Pijar?", "id": "Lebih Efisien, Tahan Lama, Ukuran Kecil." },
  { "en": "Apa Itu Fotodetektor?", "id": "Sensor Yang Mengubah Energi Cahaya Menjadi Listrik." },
  { "en": "Sebutkan Contoh Fotodetektor?", "id": "Fotodioda, Fototransistor, Dan Sel Surya." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Semikonduktor Yang Berfungsi Sebagai Fotodetektor." },
  { "en": "Apa Itu Efek Fotolistrik?", "id": "Pelepasan Elektron Saat Cahaya Mengenai Material." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Mengubah Energi Matahari Menjadi Listrik." },
  { "en": "Apa Itu Serat Optik?", "id": "Untaian Kaca Tipis Untuk Transmisi Cahaya." },
  { "en": "Apa Prinsip Kerja Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Pemantulan Internal Total?", "id": "Cahaya Dipantulkan Sempurna Di Dalam Media." },
  { "en": "Apa Dua Bagian Utama Serat Optik?", "id": "Inti (Core) Dan Selubung (Cladding)." },
  { "en": "Bagaimana Indeks Bias Inti Dan Selubung?", "id": "Indeks Bias Inti Lebih Tinggi." },
  { "en": "Apa Itu Serat Optik Mode Tunggal?", "id": "Hanya Mengizinkan Satu Mode Cahaya Merambat." },
  { "en": "Apa Itu Serat Optik Mode Jamak?", "id": "Mengizinkan Banyak Mode Cahaya Merambat." },
  { "en": "Apa Keuntungan Komunikasi Serat Optik?", "id": "Bandwidth Tinggi, Keamanan, Tahan Interferensi." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Kekuatan Sinyal Selama Transmisi." },
  { "en": "Apa Penyebab Atenuasi Dalam Serat Optik?", "id": "Penyerapan, Hamburan, Dan Kerugian Bending." },
  { "en": "Apa Itu Hamburan Rayleigh?", "id": "Hamburan Cahaya Akibat Fluktuasi Kepadatan." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Penyebaran Pulsa Optik Akibat Kecepatan Berbeda." },
  { "en": "Apa Itu Penguat Optik?", "id": "Memperkuat Sinyal Optik Tanpa Konversi Listrik." },
  { "en": "Apa Itu Erbium-Doped Fiber Amplifier (EDFA)?", "id": "Jenis Umum Penguat Serat Optik." },
  { "en": "Apa Itu Optocoupler (Opto-isolator)?", "id": "Mengisolasi Rangkaian Listrik Menggunakan Cahaya." },
  { "en": "Apa Komponen Dalam Optocoupler?", "id": "LED Dan Fotodetektor Dalam Satu Paket." },
  { "en": "Apa Itu Tampilan Kristal Cair (Liquid Crystal Display)?", "id": "Tampilan Datar Menggunakan Sifat Kristal Cair." },
  { "en": "Bagaimana LCD Bekerja?", "id": "Mengontrol Polarisasi Cahaya Yang Melewatinya." },
  { "en": "Apa Itu Tampilan Plasma?", "id": "Tampilan Datar Menggunakan Sel-sel Gas Plasma." },
  { "en": "Apa Itu Organic Light Emitting Diode (OLED)?", "id": "LED Dengan Lapisan Organik." },
  { "en": "Apa Keuntungan OLED Dibanding LCD?", "id": "Kontras Lebih Tinggi, Lebih Tipis, Fleksibel." },
  { "en": "Apa Itu Interferometer?", "id": "Alat Yang Menggunakan Interferensi Gelombang Cahaya." },
  { "en": "Sebutkan Contoh Interferometer?", "id": "Michelson, Fabry-Pérot, Dan Mach-Zehnder." },
  { "en": "Apa Itu Holografi?", "id": "Teknik Untuk Merekam Dan Merekonstruksi Gambar 3D." },
  { "en": "Apa Yang Dibutuhkan Untuk Membuat Hologram?", "id": "Sumber Cahaya Koheren Seperti Laser." },
  { "en": "Apa Itu Optik Geometri?", "id": "Cabang Optik Yang Menggambarkan Cahaya Sebagai Sinar." },
  { "en": "Apa Itu Optik Fisik?", "id": "Cabang Optik Yang Mempelajari Sifat Gelombang Cahaya." },
  { "en": "Apa Itu Optik Kuantum?", "id": "Mempelajari Interaksi Cahaya Dan Materi Kuantum." },
  { "en": "Apa Itu Lensa?", "id": "Komponen Optik Untuk Memfokuskan Atau Menyebarkan Cahaya." },
  { "en": "Apa Itu Lensa Cembung?", "id": "Lensa Yang Mengkonvergensikan Sinar Cahaya." },
  { "en": "Apa Itu Lensa Cekung?", "id": "Lensa Yang Mendivergensikan Sinar Cahaya." },
  { "en": "Apa Itu Jarak Fokus?", "id": "Jarak Dari Lensa Ke Titik Fokus." },
  { "en": "Apa Itu Aberasi Sferis?", "id": "Ketidaksempurnaan Lensa Yang Menyebabkan Keburaman." },
  { "en": "Apa Itu Aberasi Kromatik?", "id": "Pemisahan Warna Akibat Indeks Bias Berbeda." },
  { "en": "Apa Itu Cermin?", "id": "Permukaan Yang Memantulkan Cahaya." },
  { "en": "Apa Itu Prisma?", "id": "Benda Optik Untuk Membiaskan Dan Mendispersikan Cahaya." },
  { "en": "Apa Itu Monokromator?", "id": "Alat Untuk Memilih Panjang Gelombang Tertentu." },
  { "en": "Apa Itu Spektrometer?", "id": "Mengukur Sifat Cahaya Pada Spektrum Tertentu." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Dengan Konduktivitas Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Celah Pita (Band Gap)?", "id": "Energi Yang Dibutuhkan Elektron Untuk Menjadi Konduktif." },
  { "en": "Apa Itu Semikonduktor Celah Pita Langsung?", "id": "Elektron Dapat Berpindah Tanpa Perubahan Momentum." },
  { "en": "Mengapa Semikonduktor Celah Pita Langsung Penting?", "id": "Efisien Dalam Memancarkan Cahaya (Untuk LED)." },
  { "en": "Apa Itu Doping?", "id": "Menambahkan Pengotor Ke Semikonduktor Murni." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Didoping Dengan Atom Donor Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Didoping Dengan Atom Akseptor Elektron." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Batas Antara Semikonduktor Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Daerah Deplesi?", "id": "Wilayah Di Sekitar Sambungan P-N." },
  { "en": "Apa Itu Elektro-Optik?", "id": "Cabang Fisika Yang Mempelajari Interaksi." },
  { "en": "Apa Itu Efek Pockels?", "id": "Perubahan Indeks Bias Sebanding Medan Listrik." },
  { "en": "Apa Itu Efek Kerr?", "id": "Perubahan Indeks Bias Sebanding Kuadrat Medan." },
  { "en": "Apa Itu Modulator Elektro-Optik?", "id": "Perangkat Untuk Memodulasi Sinar Cahaya." },
  { "en": "Apa Itu Akusto-Optik?", "id": "Interaksi Antara Gelombang Suara Dan Cahaya." },
  { "en": "Apa Itu Modulator Akusto-Optik?", "id": "Menggunakan Gelombang Suara Untuk Memodulasi Cahaya." },
  { "en": "Apa Itu Optik Non-Linier?", "id": "Mempelajari Perilaku Cahaya Di Media Non-Linier." },
  { "en": "Apa Itu Pembuatan Harmonik Kedua (Second-Harmonic Generation)?", "id": "Menggabungkan Dua Foton Menjadi Satu Foton." },
  { "en": "Apa Itu Fotokonduktivitas?", "id": "Peningkatan Konduktivitas Listrik Akibat Cahaya." },
  { "en": "Apa Itu Bolometer?", "id": "Alat Untuk Mengukur Daya Radiasi Elektromagnetik." },
  { "en": "Apa Itu Charge-Coupled Device (CCD)?", "id": "Sensor Gambar Yang Terdiri Dari Larik Kapasitor." },
  { "en": "Apa Itu Complementary Metal-Oxide-Semiconductor (CMOS) Sensor?", "id": "Jenis Sensor Gambar Lainnya." },
  { "en": "Apa Perbedaan Utama CCD Dan CMOS?", "id": "CMOS Mengintegrasikan Lebih Banyak Fungsi Di Chip." },
  { "en": "Apa Itu Optik Terintegrasi?", "id": "Desain Dan Fabrikasi Sirkuit Optik." },
  { "en": "Apa Itu Photonic Integrated Circuit (PIC)?", "id": "Chip Yang Mengandung Komponen Fotonik." },
  { "en": "Apa Itu Pandu Gelombang (Waveguide)?", "id": "Struktur Yang Memandu Perambatan Gelombang." },
  { "en": "Apa Itu Grating?", "id": "Elemen Optik Dengan Struktur Periodik." },
  { "en": "Apa Itu Bragg Grating?", "id": "Grating Untuk Memantulkan Panjang Gelombang Tertentu." },
  { "en": "Apa Itu Fiber Bragg Grating (FBG)?", "id": "Jenis Bragg Grating Dalam Serat Optik." },
  { "en": "Apa Aplikasi Dari Fiber Bragg Grating?", "id": "Sensor, Filter Optik, Dan Laser Serat." },
  { "en": "Apa Itu Sirkulator Optik?", "id": "Perangkat Tiga Port Yang Mengarahkan Cahaya." },
  { "en": "Apa Itu Isolator Optik?", "id": "Mengizinkan Cahaya Lewat Hanya Dalam Satu Arah." },
  { "en": "Apa Prinsip Kerja Isolator Optik?", "id": "Menggunakan Efek Faraday." },
  { "en": "Apa Itu Efek Faraday?", "id": "Rotasi Polarisasi Akibat Medan Magnet." },
  { "en": "Apa Itu Dioda Laser?", "id": "Laser Berbasis Semikonduktor." },
  { "en": "Apa Keuntungan Dioda Laser?", "id": "Ukuran Kecil, Efisiensi Tinggi, Modulasi Langsung." },
  { "en": "Apa Itu Dioda Laser Fabry-Pérot?", "id": "Jenis Dioda Laser Paling Dasar." },
  { "en": "Apa Itu Dioda Laser Distributed Feedback (DFB)?", "id": "Laser Dengan Grating Terintegrasi Untuk Seleksi Mode." },
  { "en": "Apa Itu Vertical-Cavity Surface-Emitting Laser (VCSEL)?", "id": "Laser Yang Memancarkan Cahaya Secara Vertikal." },
  { "en": "Apa Keuntungan VCSEL?", "id": "Emisi Sinar Sirkular, Mudah Diuji." },
  { "en": "Apa Itu Laser Kuantum Sumur (Quantum Well Laser)?", "id": "Laser Dengan Lapisan Aktif Sangat Tipis." },
  { "en": "Apa Itu Laser Titik Kuantum (Quantum Dot Laser)?", "id": "Laser Yang Menggunakan Titik Kuantum." },
  { "en": "Apa Itu Laser Serat (Fiber Laser)?", "id": "Laser Dimana Media Penguatnya Serat Optik." },
  { "en": "Apa Itu Mode-Locking?", "id": "Teknik Untuk Menghasilkan Pulsa Cahaya Ultrapendek." },
  { "en": "Apa Itu Q-Switching?", "id": "Teknik Untuk Menghasilkan Pulsa Laser Berenergi Tinggi." },
  { "en": "Apa Itu Fototransistor?", "id": "Transistor Yang Dikontrol Oleh Paparan Cahaya." },
  { "en": "Apa Perbedaan Fotodioda Dan Fototransistor?", "id": "Fototransistor Memiliki Penguatan Internal." },
  { "en": "Apa Itu Avalanche Photodiode (APD)?", "id": "Fotodioda Dengan Penguatan Internal Melalui Efek Avalanche." },
  { "en": "Apa Itu Efek Avalanche?", "id": "Multiplikasi Arus Dalam Semikonduktor." },
  { "en": "Apa Itu Photomultiplier Tube (PMT)?", "id": "Detektor Sangat Sensitif Untuk Cahaya Sangat Lemah." },
  { "en": "Apa Itu Optik Fotonik Kristal?", "id": "Struktur Periodik Yang Mempengaruhi Gerakan Foton." },
  { "en": "Apa Itu Celah Pita Fotonik (Photonic Bandgap)?", "id": "Rentang Frekuensi Dimana Foton Tidak Dapat Merambat." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Rekayasa Dengan Sifat Tidak Biasa." },
  { "en": "Apa Contoh Sifat Metamaterial?", "id": "Indeks Bias Negatif." },
  { "en": "Apa Itu Plasmonika?", "id": "Studi Tentang Plasmon Permukaan." },
  { "en": "Apa Itu Plasmon Permukaan?", "id": "Osilasi Elektron Di Permukaan Logam." },
  { "en": "Apa Itu Silicon Photonics?", "id": "Membangun Komponen Optik Menggunakan Silikon." },
  { "en": "Mengapa Silikon Digunakan Dalam Fotonika?", "id": "Kompatibel Dengan Teknologi Fabrikasi CMOS." },
  { "en": "Apa Itu Cahaya Koheren?", "id": "Cahaya Dimana Semua Foton Memiliki Fasa Sama." },
  { "en": "Apa Itu Cahaya Monokromatik?", "id": "Cahaya Yang Terdiri Dari Satu Panjang Gelombang." },
  { "en": "Apa Itu Radiometri?", "id": "Ilmu Pengukuran Radiasi Elektromagnetik." },
  { "en": "Apa Itu Fotometri?", "id": "Ilmu Pengukuran Cahaya Yang Diterima Mata Manusia." },
  { "en": "Apa Satuan Fluks Cahaya?", "id": "Lumen." },
  { "en": "Apa Satuan Intensitas Cahaya?", "id": "Candela." },
  { "en": "Apa Itu Wavelength Division Multiplexing (WDM)?", "id": "Mengirim Banyak Sinyal Optik Di Satu Serat." },
  { "en": "Bagaimana WDM Bekerja?", "id": "Setiap Sinyal Menggunakan Panjang Gelombang Berbeda." },
  { "en": "Apa Itu Dense WDM (DWDM)?", "id": "WDM Dengan Jarak Antar Kanal Sangat Dekat." },
  { "en": "Apa Itu Coarse WDM (CWDM)?", "id": "WDM Dengan Jarak Antar Kanal Lebih Lebar." },
  { "en": "Apa Itu Multiplexer Optik?", "id": "Menggabungkan Beberapa Sinyal Optik Menjadi Satu." },
  { "en": "Apa Itu Demultiplexer Optik?", "id": "Memisahkan Sinyal Optik Gabungan." },
  { "en": "Apa Itu Optical Add-Drop Multiplexer (OADM)?", "id": "Menambah Atau Menghapus Sinyal Panjang Gelombang." },
  { "en": "Apa Itu Lidar (Light Detection and Ranging)?", "id": "Metode Penginderaan Jauh Menggunakan Laser." },
  { "en": "Apa Itu Mikroskop?", "id": "Alat Optik Untuk Melihat Objek Sangat Kecil." },
  { "en": "Apa Itu Teleskop?", "id": "Alat Optik Untuk Melihat Objek Jauh." },
  { "en": "Apa Itu Kamera?", "id": "Alat Optik Untuk Merekam Gambar." },
  { "en": "Apa Itu Apertur?", "id": "Bukaan Tempat Cahaya Masuk." },
  { "en": "Apa Itu Shutter Speed?", "id": "Durasi Waktu Sensor Terkena Cahaya." },
  { "en": "Apa Itu ISO Dalam Fotografi?", "id": "Ukuran Sensitivitas Sensor Terhadap Cahaya." },
  { "en": "Apa Itu Bilangan F (f-number)?", "id": "Rasio Jarak Fokus Terhadap Diameter Apertur." },
  { "en": "Apa Itu Depth of Field?", "id": "Rentang Jarak Objek Yang Tampak Tajam." },
  { "en": "Apa Itu Lensa Asferis?", "id": "Lensa Untuk Mengurangi Aberasi Sferis." },
  { "en": "Apa Itu Lapisan Anti-Refleksi?", "id": "Lapisan Tipis Untuk Mengurangi Pantulan." },
  { "en": "Apa Itu Filter Optik?", "id": "Memilih Atau Menolak Panjang Gelombang Cahaya." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Panjang Gelombang Panjang." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Panjang Gelombang Pendek." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Rentang Panjang Gelombang Tertentu." },
  { "en": "Apa Itu Celah Udara (Air Gap)?", "id": "Celah Antara Dua Komponen Optik." },
  { "en": "Apa Itu Optik Adaptif?", "id": "Sistem Optik Yang Mengoreksi Distorsi." },
  { "en": "Di Mana Optik Adaptif Digunakan?", "id": "Astronomi, Komunikasi Optik, Dan Mikroskopi." },
  { "en": "Apa Itu Optik Fourier?", "id": "Mempelajari Sistem Optik Menggunakan Transformasi Fourier." },
  { "en": "Apa Itu Bidang Fokus (Focal Plane)?", "id": "Bidang Tempat Sinar Paralel Difokuskan." },
  { "en": "Apa Itu Resolusi Spasial?", "id": "Kemampuan Membedakan Detail Kecil." },
  { "en": "Apa Itu Kriteria Rayleigh?", "id": "Mendefinisikan Batas Resolusi Sistem Optik." },
  { "en": "Apa Itu Numerical Aperture (NA)?", "id": "Ukuran Kemampuan Lensa Mengumpulkan Cahaya." },
  { "en": "Apa Itu Mode Lapangan (Field of View)?", "id": "Area Yang Dapat Diamati." },
  { "en": "Apa Itu Fotoluminesensi?", "id": "Emisi Cahaya Setelah Penyerapan Foton." },
  { "en": "Apa Itu Fluoresensi?", "id": "Jenis Fotoluminesensi Dengan Emisi Cepat." },
  { "en": "Apa Itu Fosforesensi?", "id": "Jenis Fotoluminesensi Dengan Emisi Lambat." },
  { "en": "Apa Itu Kemiluminesensi?", "id": "Emisi Cahaya Dari Reaksi Kimia." },
  { "en": "Apa Itu Bioluminesensi?", "id": "Kemiluminesensi Dalam Organisme Hidup." },
  { "en": "Apa Itu Efek Raman?", "id": "Hamburan Cahaya Inelastis Oleh Molekul." },
  { "en": "Apa Itu Spektroskopi Raman?", "id": "Teknik Untuk Mengidentifikasi Molekul." },
  { "en": "Apa Itu Pinset Optik (Optical Tweezers)?", "id": "Menggunakan Sinar Laser Untuk Memanipulasi Partikel." },
  { "en": "Apa Itu Radiasi Benda Hitam?", "id": "Radiasi Elektromagnetik Dari Benda Opak." },
  { "en": "Apa Hukum Planck?", "id": "Menjelaskan Spektrum Radiasi Benda Hitam." },
  { "en": "Apa Hukum Pergeseran Wien?", "id": "Menghubungkan Suhu Benda Dengan Puncak Emisi." },
  { "en": "Apa Hukum Stefan-Boltzmann?", "id": "Menghubungkan Total Energi Yang Dipancarkan Dengan Suhu." },
  { "en": "Apa Itu Piroelektrik?", "id": "Bahan Yang Menghasilkan Tegangan Saat Dipanaskan." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Berdasarkan Efek Seebeck." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Perbedaan Tegangan Akibat Perbedaan Suhu." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Yang Nilainya Berubah Dengan Suhu." },
  { "en": "Apa Itu Kamera Inframerah?", "id": "Kamera Yang Mendeteksi Radiasi Inframerah." },
  { "en": "Apa Itu Penglihatan Malam (Night Vision)?", "id": "Kemampuan Melihat Dalam Kondisi Cahaya Rendah." },
  { "en": "Apa Itu Image Intensifier?", "id": "Tabung Yang Memperkuat Cahaya Rendah." },
  { "en": "Apa Itu Radiasi Ultraviolet (UV)?", "id": "Radiasi Elektromagnetik Dengan Panjang Gelombang Pendek." },
  { "en": "Apa Itu Sinar-X?", "id": "Bentuk Radiasi Elektromagnetik Berenergi Tinggi." },
  { "en": "Apa Itu Sinar Gamma?", "id": "Bentuk Radiasi Elektromagnetik Paling Energetik." },
  { "en": "Apa Itu Pita Absorpsi?", "id": "Rentang Panjang Gelombang Yang Diserap Material." },
  { "en": "Apa Itu Transmitansi?", "id": "Fraksi Cahaya Yang Melewati Suatu Sampel." },
  { "en": "Apa Itu Absorbansi?", "id": "Ukuran Jumlah Cahaya Yang Diserap." },
  { "en": "Apa Hukum Beer-Lambert?", "id": "Menghubungkan Absorbansi Dengan Konsentrasi Zat." },
  { "en": "Apa Itu Kolorimeter?", "id": "Alat Untuk Mengukur Absorbansi Cahaya." },
  { "en": "Apa Itu Fluorometer?", "id": "Alat Untuk Mengukur Parameter Fluoresensi." },
  { "en": "Apa Itu Foton Tunggal?", "id": "Satu Kuantum Cahaya." },
  { "en": "Apa Itu Detektor Foton Tunggal?", "id": "Detektor Yang Mampu Mendeteksi Foton Tunggal." },
  { "en": "Apa Itu Efek Compton?", "id": "Hamburan Foton Berenergi Tinggi Oleh Elektron." },
  { "en": "Apa Itu Produksi Pasangan?", "id": "Penciptaan Partikel Dan Antipartikel Dari Foton." },
  { "en": "Apa Itu Anihilasi?", "id": "Proses Partikel Dan Antipartikel Saling Memusnahkan." },
  { "en": "Apa Itu Optika Atom?", "id": "Mempelajari Perilaku Atom Dalam Medan Cahaya." },
  { "en": "Apa Itu Pendinginan Laser?", "id": "Teknik Menggunakan Laser Untuk Mendinginkan Atom." },
  { "en": "Apa Itu Kondensat Bose-Einstein?", "id": "Keadaan Materi Pada Suhu Sangat Rendah." },
  { "en": "Apa Itu Kaca Fotonik?", "id": "Bahan Amorf Dengan Celah Pita Fotonik." },
  { "en": "Apa Itu Kuasikristal Fotonik?", "id": "Struktur Fotonik Dengan Tatanan Jarak Jauh." },
  { "en": "Apa Itu Efek Fotorefraktif?", "id": "Perubahan Indeks Bias Akibat Cahaya." },
  { "en": "Apa Itu Soliton Optik?", "id": "Pulsa Cahaya Yang Menjaga Bentuknya Saat Merambat." },
  { "en": "Apa Itu Time Division Multiplexing (TDM) Optik?", "id": "Menggabungkan Sinyal Dalam Domain Waktu." },
  { "en": "Apa Itu Optical Code Division Multiple Access (OCDMA)?", "id": "Teknik Akses Jamak Menggunakan Kode Optik." },
  { "en": "Apa Itu Saklar Optik (Optical Switch)?", "id": "Mengalihkan Sinyal Optik Antar Jalur." },
  { "en": "Apa Itu Micro-Electro-Mechanical System (MEMS)?", "id": "Sistem Mikro Mekanis Dan Elektris." },
  { "en": "Bagaimana MEMS Digunakan Dalam Saklar Optik?", "id": "Menggunakan Cermin Mikro Untuk Mengarahkan Cahaya." },
  { "en": "Apa Itu Pandu Gelombang Bocor (Leaky Waveguide)?", "id": "Pandu Gelombang Yang Memancarkan Daya." },
  { "en": "Apa Itu Serat Mikrostruktur (Microstructured Fiber)?", "id": "Serat Optik Dengan Lubang Udara." },
  { "en": "Apa Nama Lain Serat Mikrostruktur?", "id": "Serat Kristal Fotonik (Photonic Crystal Fiber)." },
  { "en": "Apa Itu Efek Elektro-Absorpsi?", "id": "Perubahan Penyerapan Cahaya Akibat Medan Listrik." },
  { "en": "Apa Itu Modulator Elektro-Absorpsi (EAM)?", "id": "Modulator Berbasis Efek Elektro-Absorpsi." },
  { "en": "Apa Itu Efek Franz-Keldysh?", "id": "Perubahan Absorpsi Pada Semikonduktor." },
  { "en": "Apa Itu Efek Quantum-Confined Stark?", "id": "Perubahan Energi Celah Pita Akibat Medan." },
  { "en": "Apa Itu Superluminesensi?", "id": "Emisi Spontan Yang Diperkuat." },
  { "en": "Apa Itu Superluminescent Diode (SLD)?", "id": "Sumber Cahaya Antara LED Dan Dioda Laser." },
  { "en": "Apa Karakteristik Spektrum SLD?", "id": "Lebar, Halus, Dan Seperti Lonceng." },
  { "en": "Apa Itu Optical Coherence Tomography (OCT)?", "id": "Teknik Pencitraan Medis Beresolusi Tinggi." },
  { "en": "Sumber Cahaya Apa Yang Digunakan OCT?", "id": "Cahaya Koherensi Rendah Seperti SLD." },
  { "en": "Apa Itu Heterodyne Detection?", "id": "Teknik Pencampuran Sinyal Untuk Deteksi Sensitif." },
  { "en": "Apa Itu Koherensi Spasial?", "id": "Korelasi Fasa Antara Titik Berbeda." },
  { "en": "Apa Itu Koherensi Temporal?", "id": "Korelasi Fasa Pada Waktu Berbeda." },
  { "en": "Apa Itu Panjang Koherensi?", "id": "Jarak Perambatan Dimana Cahaya Tetap Koheren." },
  { "en": "Apa Itu Optik Statistik?", "id": "Mempelajari Cahaya Dengan Fluktuasi Acak." },
  { "en": "Apa Itu Speckle Pattern?", "id": "Pola Interferensi Acak Dari Cahaya Koheren." },
  { "en": "Apa Itu Optik Difraksi?", "id": "Menggunakan Elemen Difraksi Untuk Membentuk Cahaya." },
  { "en": "Apa Itu Elemen Optik Difraksi (DOE)?", "id": "Elemen Optik Yang Dibuat Dengan Pola Mikro." },
  { "en": "Apa Itu Zona Fresnel?", "id": "Serangkaian Zona Elipsoidal Konsentris." },
  { "en": "Apa Itu Lensa Fresnel?", "id": "Lensa Kompak Yang Dibuat Dari Zona Fresnel." },
  { "en": "Apa Itu Lapisan Fotonik?", "id": "Struktur Tipis Untuk Mengontrol Perambatan Cahaya." },
  { "en": "Apa Itu DBR (Distributed Bragg Reflector)?", "id": "Cermin Berbasis Beberapa Lapisan Tipis." },
  { "en": "Apa Itu Filter Fabry-Pérot?", "id": "Resonator Optik Dengan Dua Cermin Paralel." },
  { "en": "Apa Itu Finesse?", "id": "Ukuran Kualitas Resonator Fabry-Pérot." },
  { "en": "Apa Itu Free Spectral Range (FSR)?", "id": "Jarak Frekuensi Antara Puncak Transmisi." },
  { "en": "Apa Itu Laser Gas?", "id": "Laser Yang Menggunakan Gas Sebagai Media Penguat." },
  { "en": "Sebutkan Contoh Laser Gas?", "id": "Helium-Neon, Karbon Dioksida, Dan Argon." },
  { "en": "Apa Itu Laser Helium-Neon (HeNe)?", "id": "Laser Gas Yang Umum Memancarkan Cahaya Merah." },
  { "en": "Apa Itu Laser Karbon Dioksida (CO2)?", "id": "Laser Gas Inframerah Berdaya Tinggi." },
  { "en": "Apa Itu Laser Excimer?", "id": "Laser Gas Yang Menggunakan Molekul Eksimer." },
  { "en": "Apa Itu Laser Pewarna (Dye Laser)?", "id": "Laser Yang Menggunakan Pewarna Organik." },
  { "en": "Apa Keuntungan Laser Pewarna?", "id": "Dapat Ditala Pada Rentang Panjang Gelombang." },
  { "en": "Apa Itu Laser Solid-State?", "id": "Laser Yang Menggunakan Media Penguat Padat." },
  { "en": "Sebutkan Contoh Laser Solid-State?", "id": "Nd:YAG, Ti:Sapphire, Dan Ruby." },
  { "en": "Apa Itu Laser Neodymium-Doped Yttrium Aluminium Garnet (Nd:YAG)?", "id": "Laser Solid-State Yang Umum Digunakan." },
  { "en": "Apa Itu Laser Titanium-Sapphire (Ti:Sapphire)?", "id": "Laser Solid-State Yang Dapat Ditala." },
  { "en": "Apa Itu Laser Kimia?", "id": "Laser Yang Energinya Berasal Dari Reaksi Kimia." },
  { "en": "Apa Itu Laser Elektron Bebas?", "id": "Laser Yang Elektronnya Bergerak Dalam Medan Magnet." },
  { "en": "Apa Itu Sinar Laser Gaussian?", "id": "Profil Intensitas Sinar Laser Paling Umum." },
  { "en": "Apa Itu Pinggang Sinar (Beam Waist)?", "id": "Lokasi Dimana Diameter Sinar Paling Kecil." },
  { "en": "Apa Itu Kualitas Sinar Laser (Beam Quality)?", "id": "Ukuran Seberapa Dekat Sinar Dengan Ideal." },
  { "en": "Apa Itu Faktor M²?", "id": "Parameter Kuantitatif Untuk Kualitas Sinar Laser." },
  { "en": "Apa Itu Divergensi Sinar?", "id": "Ukuran Seberapa Cepat Sinar Menyebar." },
  { "en": "Apa Itu Kolimasi?", "id": "Membuat Sinar Cahaya Menjadi Paralel." },
  { "en": "Apa Itu Cahaya Hitam (Black Light)?", "id": "Lampu Yang Memancarkan Sebagian Besar Radiasi UV." },
  { "en": "Apa Itu Fotofor?", "id": "Bahan Yang Menunjukkan Fosforesensi." },
  { "en": "Apa Itu Kaca Spion Elektrokromik?", "id": "Spion Yang Dapat Meredup Secara Otomatis." },
  { "en": "Apa Itu Efek Elektrokromik?", "id": "Perubahan Warna Akibat Tegangan Listrik." },
  { "en": "Apa Itu Saklar Magneto-Optik?", "id": "Saklar Yang Menggunakan Efek Magneto-Optik." },
  { "en": "Apa Itu Efek Kerr Magneto-Optik (MOKE)?", "id": "Perubahan Polarisasi Cahaya Yang Dipantulkan." },
  { "en": "Apa Itu Bistabilitas Optik?", "id": "Sistem Optik Dengan Dua Keadaan Stabil." },
  { "en": "Apa Itu Optical Parametric Oscillator (OPO)?", "id": "Sumber Cahaya Koheren Berbasis Interaksi Parametrik." },
  { "en": "Apa Itu Perhitungan Optik?", "id": "Menggunakan Foton Alih-alih Elektron Untuk Komputasi." },
  { "en": "Apa Itu Transparent Conductor?", "id": "Bahan Yang Menghantarkan Listrik Dan Transparan." },
  { "en": "Apa Contoh Konduktor Transparan?", "id": "Indium Tin Oxide (ITO)." },
  { "en": "Apa Itu Sel Pockels?", "id": "Modulator Optik Berdasarkan Efek Pockels." },
  { "en": "Apa Itu Detektor Kuantum?", "id": "Fotodetektor Berbasis Interaksi Foton-Elektron." },
  { "en": "Apa Itu Detektor Termal?", "id": "Fotodetektor Berbasis Efek Pemanasan Cahaya." },
  { "en": "Apakah Fotodioda Termasuk Detektor Kuantum?", "id": "Ya, Fotodioda Adalah Detektor Kuantum." },
  { "en": "Apakah Bolometer Termasuk Detektor Termal?", "id": "Ya, Bolometer Adalah Detektor Termal." },
  { "en": "Apa Itu Responsivitas Fotodetektor?", "id": "Rasio Arus Listrik Terhadap Daya Optik." },
  { "en": "Apa Itu Arus Gelap (Dark Current)?", "id": "Arus Yang Mengalir Tanpa Adanya Cahaya." },
  { "en": "Apa Itu Noise-Equivalent Power (NEP)?", "id": "Daya Optik Minimum Yang Dapat Dideteksi." },
  { "en": "Apa Itu Quantum Efficiency?", "id": "Rasio Elektron Dihasilkan Per Foton Masuk." },
  { "en": "Apa Itu Semikonduktor Senyawa?", "id": "Semikonduktor Terbuat Dari Dua Atau Lebih Elemen." },
  { "en": "Sebutkan Contoh Semikonduktor Senyawa?", "id": "Galium Arsenida (GaAs), Indium Fosfida (InP)." },
  { "en": "Apa Itu Heterostruktur?", "id": "Antarmuka Antara Dua Semikonduktor Berbeda." },
  { "en": "Apa Itu Sumur Kuantum (Quantum Well)?", "id": "Lapisan Tipis Yang Mengurung Partikel." },
  { "en": "Apa Itu Kawat Kuantum (Quantum Wire)?", "id": "Struktur Yang Mengurung Partikel Dalam Dua Dimensi." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor Yang Mengurung Partikel." },
  { "en": "Apa Itu Epitaksi?", "id": "Metode Menumbuhkan Lapisan Kristal Tipis." },
  { "en": "Apa Itu Molecular Beam Epitaxy (MBE)?", "id": "Teknik Epitaksi Untuk Lapisan Berkualitas Tinggi." },
  { "en": "Apa Itu Metal-Organic Chemical Vapour Deposition (MOCVD)?", "id": "Teknik Epitaksi Populer Lainnya." },
  { "en": "Apa Itu Litografi?", "id": "Proses Untuk Membuat Pola Pada Substrat." },
  { "en": "Apa Itu Fotolitografi?", "id": "Litografi Yang Menggunakan Cahaya Untuk Transfer Pola." },
  { "en": "Apa Itu Photoresist?", "id": "Bahan Peka Cahaya Yang Digunakan Dalam Fotolitografi." },
  { "en": "Apa Itu Electron Beam Lithography?", "id": "Litografi Menggunakan Berkas Elektron Untuk Pola." },
  { "en": "Apa Itu Fabrikasi Ruang Bersih (Cleanroom)?", "id": "Lingkungan Terkontrol Untuk Fabrikasi Semikonduktor." },
  { "en": "Apa Itu Optofluida?", "id": "Integrasi Optik Dan Mikrofluida." },
  { "en": "Apa Itu Biosensor?", "id": "Perangkat Analitis Yang Menggunakan Komponen Biologis." },
  { "en": "Bagaimana Fotonika Digunakan Dalam Biosensor?", "id": "Untuk Mendeteksi Perubahan Optik Akibat Reaksi." },
  { "en": "Apa Itu Surface Plasmon Resonance (SPR)?", "id": "Teknik Populer Untuk Biosensor Berbasis Optik." },
  { "en": "Apa Itu Ellipsometri?", "id": "Teknik Pengukuran Optik Untuk Karakterisasi Lapisan Tipis." },
  { "en": "Apa Itu Matriks Jones?", "id": "Representasi Matematis Untuk Elemen Optik Polarisasi." },
  { "en": "Apa Itu Vektor Jones?", "id": "Mewakili Keadaan Polarisasi Gelombang Cahaya." },
  { "en": "Apa Itu Parameter Stokes?", "id": "Ukuran Alternatif Untuk Polarisasi Cahaya." },
  { "en": "Apa Itu Bola Poincaré?", "id": "Representasi Geometris Dari Keadaan Polarisasi." },
  { "en": "Apa Itu Birefringence (Pembiasan Ganda)?", "id": "Sifat Optik Material Dengan Indeks Bias Berbeda." },
  { "en": "Apa Itu Sumbu Optik?", "id": "Arah Dalam Kristal Dimana Tidak Terjadi Birefringence." },
  { "en": "Apa Itu Waveplate (Pelat Gelombang)?", "id": "Elemen Optik Yang Mengubah Keadaan Polarisasi." },
  { "en": "Apa Itu Half-Wave Plate?", "id": "Waveplate Yang Menggeser Fasa Sebesar 180 Derajat." },
  { "en": "Apa Itu Quarter-Wave Plate?", "id": "Waveplate Yang Menggeser Fasa Sebesar 90 Derajat." },
  { "en": "Bagaimana Quarter-Wave Plate Membuat Polarisasi Sirkular?", "id": "Dengan Mengubah Cahaya Terpolarisasi Linier." },
  { "en": "Apa Itu Polarisator?", "id": "Filter Optik Yang Melewatkan Polarisasi Tertentu." },
  { "en": "Apa Itu Hukum Malus?", "id": "Menjelaskan Intensitas Cahaya Yang Melewati Polarisator." },
  { "en": "Apa Itu Sudut Brewster?", "id": "Sudut Datang Dimana Cahaya Terpolarisasi Sempurna." },
  { "en": "Apa Itu Aktivitas Optik?", "id": "Rotasi Polarisasi Cahaya Oleh Bahan Kiral." },
  { "en": "Apa Itu Bahan Kiral?", "id": "Struktur Yang Tidak Superimposabel Pada Bayangan Cerminnya." },
  { "en": "Apa Itu Tampilan Stereoskopik?", "id": "Teknologi Yang Menciptakan Ilusi Kedalaman 3D." },
  { "en": "Apa Itu Autostereoscopy?", "id": "Tampilan 3D Yang Tidak Memerlukan Kacamata." },
  { "en": "Apa Itu Lensa Lentikular?", "id": "Lensa Yang Digunakan Dalam Tampilan Autostereoskopik." },
  { "en": "Apa Itu Efek Fotovoltaik?", "id": "Penciptaan Tegangan Listrik Akibat Paparan Cahaya." },
  { "en": "Apa Bahan Utama Sel Surya Silikon?", "id": "Silikon Monokristalin Atau Polikristalin." },
  { "en": "Apa Itu Sel Surya Lapisan Tipis?", "id": "Sel Surya Yang Dibuat Dari Lapisan Tipis." },
  { "en": "Apa Itu Sel Surya Organik?", "id": "Sel Surya Yang Menggunakan Polimer Organik." },
  { "en": "Apa Itu Sel Surya Perovskite?", "id": "Jenis Sel Surya Baru Dengan Efisiensi Tinggi." },
  { "en": "Apa Itu Sel Surya Multi-Sambungan?", "id": "Sel Surya Dengan Beberapa Sambungan P-N." },
  { "en": "Mengapa Sel Surya Multi-Sambungan Sangat Efisien?", "id": "Karena Menyerap Rentang Spektrum Matahari Lebih Lebar." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Teknik Untuk Mendapatkan Daya Maksimal Dari Sel Surya." },
  { "en": "Apa Itu Koefisien Penyerapan?", "id": "Ukuran Seberapa Kuat Cahaya Diserap Material." },
  { "en": "Apa Itu Kedalaman Penetrasi?", "id": "Jarak Dimana Intensitas Cahaya Menjadi 1/e." },
  { "en": "Apa Itu Rekombinasi?", "id": "Proses Elektron Dan Lubang Saling Memusnahkan." },
  { "en": "Apa Itu Rekombinasi Radiatif?", "id": "Rekombinasi Yang Menghasilkan Emisi Foton." },
  { "en": "Apa Itu Rekombinasi Non-Radiatif?", "id": "Rekombinasi Yang Menghasilkan Panas (Fonon)." },
  { "en": "Apa Itu Waktu Hidup Pembawa (Carrier Lifetime)?", "id": "Waktu Rata-rata Elektron Dan Lubang Bertahan." },
  { "en": "Apa Itu Fotodetektor Intrinsik?", "id": "Menggunakan Bahan Semikonduktor Murni." },
  { "en": "Apa Itu Fotodetektor Ekstrinsik?", "id": "Menggunakan Bahan Semikonduktor Yang Didoping." },
  { "en": "Apa Itu Photoconductive Gain?", "id": "Peningkatan Arus Dalam Fotokonduktor." },
  { "en": "Apa Itu Waktu Transit?", "id": "Waktu Yang Dibutuhkan Pembawa Muatan Melintasi Perangkat." },
  { "en": "Apa Itu Responsivitas Spektral?", "id": "Responsivitas Fotodetektor Sebagai Fungsi Panjang Gelombang." },
  { "en": "Apa Itu Panjang Gelombang Cut-off?", "id": "Panjang Gelombang Maksimum Yang Dapat Dideteksi." },
  { "en": "Apa Itu Detektor Inframerah?", "id": "Detektor Yang Sensitif Terhadap Radiasi Inframerah." },
  { "en": "Apa Itu Pendinginan Kriogenik?", "id": "Mendinginkan Detektor Untuk Mengurangi Derau Termal." },
  { "en": "Apa Itu Derau Termal (Johnson-Nyquist Noise)?", "id": "Derau Akibat Agitasi Termal Pembawa Muatan." },
  { "en": "Apa Itu Derau Tembak (Shot Noise)?", "id": "Derau Akibat Sifat Diskrit Muatan Listrik." },
  { "en": "Apa Itu Derau Generasi-Rekombinasi?", "id": "Derau Akibat Fluktuasi Generasi Pembawa." },
  { "en": "Apa Itu Derau Kedip (Flicker Noise / 1/f)?", "id": "Derau Yang Kepadatannya Berbanding Terbalik Frekuensi." },
  { "en": "Apa Itu Optik Terintegrasi Silikon-Nitrat?", "id": "Platform Optik Terintegrasi Dengan Kerugian Rendah." },
  { "en": "Apa Itu Resonator Cincin Mikro?", "id": "Resonator Optik Berbentuk Cincin Kecil." },
  { "en": "Apa Itu Aplikasi Resonator Cincin Mikro?", "id": "Filter, Modulator, Sensor, Dan Saklar." },
  { "en": "Apa Itu Frekuensi Mode Bisik (Whispering Gallery Mode)?", "id": "Mode Resonansi Dalam Struktur Sirkular." },
  { "en": "Apa Itu Faktor-Q (Quality Factor)?", "id": "Ukuran Kualitas Resonator Optik." },
  { "en": "Faktor-Q Tinggi Berarti Apa?", "id": "Kerugian Rendah Dan Penyimpanan Energi Yang Baik." },
  { "en": "Apa Itu Sambungan Fusi (Fusion Splicing)?", "id": "Menyambung Dua Serat Optik Secara Permanen." },
  { "en": "Apa Itu Sambungan Mekanis?", "id": "Menyambung Serat Optik Secara Tidak Permanen." },
  { "en": "Apa Itu Konektor Serat Optik?", "id": "Ujung Kabel Serat Untuk Koneksi." },
  { "en": "Sebutkan Jenis Konektor Serat Optik?", "id": "SC, ST, LC, Dan FC." },
  { "en": "Apa Itu Optical Time-Domain Reflectometer (OTDR)?", "id": "Alat Uji Untuk Mengkarakterisasi Serat Optik." },
  { "en": "Bagaimana OTDR Bekerja?", "id": "Mengirim Pulsa Dan Menganalisis Sinyal Balik." },
  { "en": "Apa Itu Hamburan Balik (Backscattering)?", "id": "Cahaya Yang Dihamburkan Kembali Ke Arah Sumber." },
  { "en": "Apa Itu Zona Mati OTDR?", "id": "Area Dimana OTDR Tidak Dapat Melakukan Pengukuran." },
  { "en": "Apa Itu Pemrosesan Sinyal Optik?", "id": "Memproses Sinyal Langsung Dalam Domain Optik." },
  { "en": "Apa Itu Memori Optik?", "id": "Menyimpan Data Menggunakan Media Optik." },
  { "en": "Apa Itu Cakram Blu-ray?", "id": "Format Cakram Optik Menggunakan Laser Biru-Ungu." },
  { "en": "Mengapa Laser Biru Digunakan Di Blu-ray?", "id": "Panjang Gelombang Lebih Pendek Memungkinkan Kepadatan Lebih." },
  { "en": "Apa Itu Penyimpanan Data Holografik?", "id": "Menyimpan Informasi Di Seluruh Volume Media." },
  { "en": "Apa Itu Efek Akusto-Optik?", "id": "Interaksi Antara Cahaya Dan Gelombang Akustik." },
  { "en": "Apa Itu Deflektor Akusto-Optik?", "id": "Menggunakan Suara Untuk Mengubah Arah Sinar." },
  { "en": "Apa Itu Optical Parametric Amplification (OPA)?", "id": "Penguatan Sinyal Optik Berbasis Efek Non-Linier." },
  { "en": "Apa Itu Four-Wave Mixing (FWM)?", "id": "Proses Non-Linier Melibatkan Empat Gelombang." },
  { "en": "Apa Itu Efek Fotoakustik?", "id": "Pembentukan Gelombang Suara Akibat Penyerapan Cahaya." },
  { "en": "Apa Itu Mikroskopi Fotoakustik?", "id": "Teknik Pencitraan Berbasis Efek Fotoakustik." },
  { "en": "Apa Itu Laser Terawatt?", "id": "Laser Yang Menghasilkan Daya Puncak Terawatt." },
  { "en": "Apa Itu Laser Petawatt?", "id": "Laser Yang Menghasilkan Daya Puncak Petawatt." },
  { "en": "Apa Itu Chirped Pulse Amplification (CPA)?", "id": "Teknik Untuk Menghasilkan Pulsa Laser Ultraintens." },
  { "en": "Apa Itu Optik Attosekon?", "id": "Ilmu Yang Mempelajari Fenomena Waktu Attosekon." },
  { "en": "Apa Itu High Harmonic Generation (HHG)?", "id": "Proses Untuk Menghasilkan Cahaya Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Serat Optik Plastik (POF)?", "id": "Serat Optik Yang Terbuat Dari Polimer." },
  { "en": "Apa Keuntungan Serat Optik Plastik?", "id": "Lebih Fleksibel Dan Murah Dibanding Kaca." },
  { "en": "Apa Kerugian Serat Optik Plastik?", "id": "Atenuasi Lebih Tinggi Dibanding Serat Kaca." },
  { "en": "Apa Itu Sol-Gel Process?", "id": "Metode Kimia Untuk Membuat Bahan Padat." },
  { "en": "Bagaimana Sol-Gel Digunakan Dalam Fotonika?", "id": "Untuk Membuat Kaca, Keramik, Dan Lapisan Optik." },
  { "en": "Apa Itu Upconversion?", "id": "Proses Yang Mengubah Cahaya Energi Rendah." },
  { "en": "Apa Itu Downconversion?", "id": "Proses Yang Memecah Satu Foton Menjadi Dua." },
  { "en": "Apa Itu Spontaneous Parametric Down-Conversion (SPDC)?", "id": "Sumber Penting Untuk Pasangan Foton Terjerat." },
  { "en": "Apa Itu Foton Terjerat (Entangled Photons)?", "id": "Pasangan Foton Dengan Sifat Kuantum Terhubung." },
  { "en": "Apa Itu Interferometer Hong-Ou-Mandel?", "id": "Menunjukkan Sifat Kuantum Dari Dua Foton." },
  { "en": "Apa Itu Efek Talbot?", "id": "Efek Difraksi Medan Dekat." },
  { "en": "Apa Itu Karpet Talbot?", "id": "Pola Intensitas Fraktal Yang Dihasilkan Efek Talbot." },
  { "en": "Apa Itu Efek Goos-Hänchen?", "id": "Pergeseran Lateral Sinar Saat Pemantulan Internal." },
  { "en": "Apa Itu Spin Hall Effect of Light?", "id": "Fenomena Fotonik Yang Terkait Spin Foton." },
  { "en": "Apa Itu Sinar Bessel?", "id": "Sinar Cahaya Yang Tidak Mengalami Difraksi." },
  { "en": "Apa Itu Sinar Airy?", "id": "Sinar Cahaya Yang Dapat Berakselerasi Sendiri." },
  { "en": "Apa Itu Orbital Angular Momentum (OAM) of Light?", "id": "Momentum Sudut Cahaya Terkait Fasa Spasial." },
  { "en": "Apa Itu Sinar Vorteks?", "id": "Sinar Cahaya Yang Membawa Orbital Angular Momentum." },
  { "en": "Apa Itu Radiasi Cherenkov?", "id": "Radiasi Elektromagnetik Saat Partikel Bergerak Cepat." },
  { "en": "Apa Itu Radiasi Synchrotron?", "id": "Radiasi Elektromagnetik Dari Partikel Bermuatan." },
  { "en": "Apa Itu Fluoresensi Sinar-X?", "id": "Emisi Sinar-X Karakteristik Dari Bahan." },
  { "en": "Apa Itu Kristalografi Sinar-X?", "id": "Teknik Menentukan Struktur Atom Kristal." },
  { "en": "Apa Itu Mikroskop Elektron?", "id": "Mikroskop Yang Menggunakan Berkas Elektron." },
  { "en": "Apa Itu Scanning Electron Microscope (SEM)?", "id": "Menghasilkan Gambar Dengan Memindai Permukaan." },
  { "en": "Apa Itu Transmission Electron Microscope (TEM)?", "id": "Menembakkan Elektron Melalui Spesimen Sangat Tipis." },
  { "en": "Apa Itu Aberasi Dalam Mikroskop Elektron?", "id": "Distorsi Yang Membatasi Resolusi Gambar." },
  { "en": "Apa Itu Lensa Elektromagnetik?", "id": "Menggunakan Medan Magnet Untuk Memfokuskan Elektron." },
  { "en": "Apa Itu Optik Terintegrasi Hibrid?", "id": "Menggabungkan Berbagai Material Dalam Satu Chip." },
  { "en": "Apa Itu Laser Kaskade Kuantum?", "id": "Laser Semikonduktor Untuk Daerah Inframerah." },
  { "en": "Apa Prinsip Kerja Laser Kaskade Kuantum?", "id": "Berdasarkan Transisi Antar Sub-pita Kuantum." },
  { "en": "Apa Itu Detektor Superkonduktor?", "id": "Detektor Sangat Sensitif Berbasis Efek Superkonduktivitas." },
  { "en": "Apa Itu Superconducting Nanowire Single-Photon Detector (SNSPD)?", "id": "Jenis Detektor Foton Tunggal Superkonduktor." },
  { "en": "Apa Itu Frequency Comb?", "id": "Spektrum Optik Terdiri Dari Garis-garis Terpisah." },
  { "en": "Apa Aplikasi Dari Frequency Comb?", "id": "Metrologi Frekuensi, Jam Atom, Spektroskopi." },
  { "en": "Apa Itu Jam Atom Optik?", "id": "Standar Frekuensi Berbasis Transisi Atom." },
  { "en": "Apa Itu Litografi Nanoimprint?", "id": "Teknik Litografi Dengan Cetakan Mekanis." },
  { "en": "Apa Itu Self-Assembly?", "id": "Proses Komponen Mengatur Diri Menjadi Struktur." },
  { "en": "Apa Itu Blok Kopolimer?", "id": "Polimer Yang Terdiri Dari Blok-blok Berbeda." },
  { "en": "Bagaimana Blok Kopolimer Digunakan Dalam Fotonika?", "id": "Untuk Membuat Struktur Nano Periodik." },
  { "en": "Apa Itu Nanofotonika?", "id": "Studi Perilaku Cahaya Pada Skala Nanometer." },
  { "en": "Apa Itu Antena Optik?", "id": "Mengubah Energi Elektromagnetik Terlokalisasi." },
  { "en": "Apa Itu Near-Field Scanning Optical Microscope (NSOM)?", "id": "Mikroskop Yang Melebihi Batas Difraksi." },
  { "en": "Apa Itu Medan Evanescent?", "id": "Medan Elektromagnetik Stasioner Dekat Permukaan." },
  { "en": "Apa Itu Optik Gradien Indeks (GRIN)?", "id": "Optik Dimana Indeks Bias Bervariasi." },
  { "en": "Apa Itu Lensa GRIN?", "id": "Lensa Yang Dibuat Dari Material Gradien Indeks." },
  { "en": "Apa Itu Serat Optik Gradien Indeks?", "id": "Serat Mode Jamak Dengan Indeks Bias Gradual." },
  { "en": "Bagaimana Serat Gradien Indeks Mengurangi Dispersi?", "id": "Dengan Membuat Semua Mode Bergerak Hampir Sama." },
  { "en": "Apa Itu Serat Optik Indeks Tangga (Step-Index)?", "id": "Serat Dengan Perubahan Indeks Bias Tajam." },
  { "en": "Apa Itu Dispersi Modal?", "id": "Penyebaran Pulsa Akibat Mode Berbeda." },
  { "en": "Di Jenis Serat Apa Dispersi Modal Terjadi?", "id": "Hanya Dalam Serat Optik Mode Jamak." },
  { "en": "Apa Itu Dispersi Pandu Gelombang?", "id": "Dispersi Akibat Geometri Pandu Gelombang." },
  { "en": "Apa Itu Dispersi Material?", "id": "Dispersi Akibat Variasi Indeks Bias." },
  { "en": "Apa Itu Panjang Gelombang Zero-Dispersion?", "id": "Panjang Gelombang Dimana Dispersi Kromatik Nol." },
  { "en": "Apa Itu Serat Dispersion-Shifted?", "id": "Serat Yang Dirancang Menggeser Panjang Gelombang." },
  { "en": "Apa Itu Serat Dispersion-Flattened?", "id": "Serat Dengan Dispersi Rendah Pada Rentang Lebar." },
  { "en": "Apa Itu Polarization Mode Dispersion (PMD)?", "id": "Penyebaran Pulsa Akibat Birefringence Serat." },
  { "en": "Apa Itu Konektor Ferrule?", "id": "Lengan Penjajaran Presisi Dalam Konektor Serat." },
  { "en": "Apa Itu Physical Contact (PC) Polish?", "id": "Jenis Poles Ujung Konektor Serat." },
  { "en": "Apa Itu Angled Physical Contact (APC) Polish?", "id": "Poles Ujung Serat Pada Sudut." },
  { "en": "Mengapa Poles APC Digunakan?", "id": "Untuk Mengurangi Pantulan Balik Secara Signifikan." },
  { "en": "Apa Itu Return Loss?", "id": "Ukuran Cahaya Yang Dipantulkan Kembali." },
  { "en": "Apa Itu Insertion Loss?", "id": "Kehilangan Daya Sinyal Akibat Penyisipan Komponen." },
  { "en": "Apa Itu Optical Power Meter?", "id": "Alat Untuk Mengukur Daya Sinyal Optik." },
  { "en": "Apa Itu Visual Fault Locator (VFL)?", "id": "Menggunakan Laser Merah Untuk Menemukan Kerusakan." },
  { "en": "Apa Itu Optical Spectrum Analyzer (OSA)?", "id": "Mengukur Daya Optik Pada Berbagai Panjang Gelombang." },
  { "en": "Apa Itu Fiber Optic Sensor (FOS)?", "id": "Sensor Yang Menggunakan Serat Optik." },
  { "en": "Apa Keuntungan Sensor Serat Optik?", "id": "Kekebalan Elektromagnetik, Ukuran Kecil, Sensitivitas." },
  { "en": "Apa Itu Sensor Suhu FBG?", "id": "Mengukur Suhu Berdasarkan Pergeseran Bragg." },
  { "en": "Apa Itu Giroskop Serat Optik?", "id": "Mengukur Rotasi Menggunakan Efek Sagnac." },
  { "en": "Apa Itu Efek Sagnac?", "id": "Pergeseran Fasa Akibat Rotasi." },
  { "en": "Apa Itu Hidrofon Serat Optik?", "id": "Sensor Akustik Bawah Air." },
  { "en": "Apa Itu Sistem Komunikasi Koheren Optik?", "id": "Menggunakan Deteksi Koheren Untuk Kinerja Tinggi." },
  { "en": "Apa Itu Deteksi Koheren?", "id": "Mencampur Sinyal Diterima Dengan Osilator Lokal." },
  { "en": "Apa Itu Osilator Lokal (Local Oscillator)?", "id": "Sumber Laser Di Sisi Penerima." },
  { "en": "Apa Itu Deteksi Homodyne?", "id": "Frekuensi Osilator Lokal Sama Dengan Pembawa." },
  { "en": "Apa Itu Deteksi Heterodyne?", "id": "Frekuensi Osilator Lokal Berbeda Dari Pembawa." },
  { "en": "Apa Keuntungan Komunikasi Koheren?", "id": "Sensitivitas Lebih Tinggi Dan Kapasitas Lebih Besar." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Prosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Bagaimana DSP Digunakan Dalam Komunikasi Koheren?", "id": "Untuk Mengoreksi Gangguan Kanal." },
  { "en": "Apa Itu Kompensasi Dispersi Elektronik?", "id": "Menggunakan DSP Untuk Membatalkan Efek Dispersi." },
  { "en": "Apa Itu Jaringan Optik Pasif (Passive Optical Network)?", "id": "Jaringan Fiber-to-the-Home Tanpa Komponen Aktif." },
  { "en": "Apa Itu Optical Line Terminal (OLT)?", "id": "Titik Akhir Penyedia Layanan Dalam PON." },
  { "en": "Apa Itu Optical Network Unit (ONU)?", "id": "Perangkat Di Sisi Pengguna Dalam PON." },
  { "en": "Apa Itu Optical Distribution Network (ODN)?", "id": "Serat Dan Splitter Antara OLT Dan ONU." },
  { "en": "Apa Itu Splitter Optik?", "id": "Membagi Sinyal Optik Menjadi Beberapa Jalur." },
  { "en": "Apa Itu Skema Akses Jamak Dalam PON?", "id": "TDMA Untuk Arah Hulu." },
  { "en": "Apa Itu Efek Fototermal?", "id": "Efek Pemanasan Akibat Penyerapan Cahaya." },
  { "en": "Apa Itu Terapi Fotodinamik?", "id": "Pengobatan Medis Menggunakan Cahaya Dan Fotosensitizer." },
  { "en": "Apa Itu Ablasi Laser?", "id": "Menghilangkan Material Dari Permukaan Menggunakan Laser." },
  { "en": "Apa Itu Pengelasan Laser?", "id": "Menggunakan Laser Untuk Menyambung Material." },
  { "en": "Apa Itu Pemotongan Laser?", "id": "Menggunakan Laser Untuk Memotong Material." },
  { "en": "Apa Itu Penandaan Laser (Laser Marking)?", "id": "Membuat Tanda Permanen Menggunakan Laser." },
  { "en": "Apa Itu Litografi Laser?", "id": "Teknik Fabrikasi Mikro Menggunakan Laser." },
  { "en": "Apa Itu Laser Annealing?", "id": "Proses Pemanasan Material Menggunakan Laser." },
  { "en": "Apa Itu Laser Pendingin?", "id": "Mendinginkan Benda Padat Menggunakan Laser." },
  { "en": "Apa Itu Laser Fusi Inersia?", "id": "Penelitian Fusi Nuklir Menggunakan Laser." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Konversi Langsung Perbedaan Suhu Ke Tegangan." },
  { "en": "Apa Itu Pendingin Termoelektrik (TEC)?", "id": "Perangkat Yang Mendinginkan Menggunakan Efek Peltier." },
  { "en": "Apa Itu Efek Peltier?", "id": "Pemanasan Atau Pendinginan Pada Sambungan Listrik." },
  { "en": "Bagaimana TEC Digunakan Dalam Fotonika?", "id": "Untuk Menstabilkan Suhu Dioda Laser." },
  { "en": "Apa Itu LED Ultraviolet (UV-LED)?", "id": "LED Yang Memancarkan Radiasi UV." },
  { "en": "Apa Aplikasi UV-LED?", "id": "Sterilisasi, Curing, Dan Analisis Fluoresensi." },
  { "en": "Apa Itu LED Inframerah (IR-LED)?", "id": "LED Yang Memancarkan Radiasi Inframerah." },
  { "en": "Apa Aplikasi IR-LED?", "id": "Pengendali Jarak Jauh, Komunikasi Optik." },
  { "en": "Apa Itu Layar Tujuh Segmen?", "id": "Bentuk Tampilan Elektronik Untuk Angka Desimal." },
  { "en": "Apa Itu Dot Matrix Display?", "id": "Tampilan Yang Menggunakan Matriks Cahaya." },
  { "en": "Apa Itu Efek Elektroforesis?", "id": "Gerakan Partikel Akibat Medan Listrik." },
  { "en": "Apa Itu Tampilan Kertas Elektronik (E-Paper)?", "id": "Teknologi Tampilan Yang Meniru Tinta." },
  { "en": "Bagaimana E-Paper Bekerja?", "id": "Mengatur Ulang Partikel Berpigmen." },
  { "en": "Apa Keuntungan Utama E-Paper?", "id": "Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Itu Efek Fotokromik?", "id": "Perubahan Warna Reversibel Akibat Paparan Cahaya." },
  { "en": "Apa Itu Lensa Fotokromik?", "id": "Lensa Kacamata Yang Menjadi Gelap." },
  { "en": "Apa Itu Kaca Cerdas (Smart Glass)?", "id": "Kaca Yang Sifat Transmisinya Dapat Diubah." },
  { "en": "Apa Itu Suspended Particle Device (SPD)?", "id": "Jenis Teknologi Kaca Cerdas." },
  { "en": "Apa Itu Laser Speckle?", "id": "Pola Acak Yang Dihasilkan Oleh Laser." },
  { "en": "Apa Itu Optik Difusif?", "id": "Studi Perambatan Cahaya Dalam Media Hamburan." },
  { "en": "Apa Itu Tomografi Optik Difusif?", "id": "Teknik Pencitraan Untuk Objek Hamburan." },
  { "en": "Apa Itu Fotonika Gelombang Mikro?", "id": "Interaksi Antara Gelombang Mikro Dan Cahaya." },
  { "en": "Apa Itu Pemrosesan Sinyal Gelombang Mikro Optik?", "id": "Memproses Sinyal Gelombang Mikro Dalam Domain Optik." },
  { "en": "Apa Keuntungan Fotonika Gelombang Mikro?", "id": "Bandwidth Besar, Kerugian Rendah, Kekebalan EMI." },
  { "en": "Apa Itu Filter Gelombang Mikro Fotonik?", "id": "Filter Frekuensi Radio Yang Dibuat Dengan Fotonika." },
  { "en": "Apa Itu Osilator Optoelektronik?", "id": "Osilator Yang Menghasilkan Sinyal Gelombang Mikro Stabil." },
  { "en": "Apa Itu Fotomixer?", "id": "Mencampur Dua Laser Untuk Menghasilkan Frekuensi Terahertz." },
  { "en": "Apa Itu Radiasi Terahertz (THz)?", "id": "Wilayah Spektrum Antara Gelombang Mikro Dan Inframerah." },
  { "en": "Apa Aplikasi Dari Radiasi Terahertz?", "id": "Pencitraan Keamanan, Spektroskopi, Dan Komunikasi." },
  { "en": "Apa Itu Optik Medan Dekat?", "id": "Mempelajari Medan Optik Dekat Sumber." },
  { "en": "Apa Itu Batas Difraksi?", "id": "Batas Fundamental Pada Resolusi Sistem Optik." },
  { "en": "Bagaimana Cara Mengatasi Batas Difraksi?", "id": "Menggunakan Teknik Seperti Optik Medan Dekat." },
  { "en": "Apa Itu Superlensa?", "id": "Lensa Yang Dibuat Dari Metamaterial." },
  { "en": "Apa Kemampuan Superlensa?", "id": "Dapat Mencitrakan Objek Lebih Kecil Dari Batas Difraksi." },
  { "en": "Apa Itu Jubah Gaib (Invisibility Cloak)?", "id": "Alat Hipotetis Untuk Membuat Objek Tak Terlihat." },
  { "en": "Bagaimana Metamaterial Berhubungan Dengan Jubah Gaib?", "id": "Metamaterial Digunakan Untuk Membelokkan Cahaya." },
  { "en": "Apa Itu Transformasi Optik?", "id": "Metode Desain Untuk Mengontrol Cahaya." },
  { "en": "Apa Itu Koefisien Fresnel?", "id": "Menjelaskan Amplitudo Cahaya Yang Dipantulkan Dan Dibiaskan." },
  { "en": "Apa Itu Elipsometri Spektroskopi?", "id": "Mengukur Sifat Optik Pada Rentang Panjang Gelombang." },
  { "en": "Apa Itu Persamaan Maxwell?", "id": "Kumpulan Persamaan Dasar Elektromagnetisme." },
  { "en": "Apa Itu Vektor Poynting?", "id": "Mewakili Arah Aliran Energi Gelombang." },
  { "en": "Apa Itu Impedansi Optik?", "id": "Analogi Impedansi Listrik Untuk Gelombang Optik." },
  { "en": "Apa Itu Efek Goos-Hänchen?", "id": "Pergeseran Lateral Sinar Saat Pemantulan Total." },
  { "en": "Apa Itu Efek Imbert-Fedorov?", "id": "Pergeseran Transversal Sinar Saat Pemantulan." },
  { "en": "Apa Itu Cahaya Lambat (Slow Light)?", "id": "Perambatan Pulsa Optik Dengan Kecepatan Grup Rendah." },
  { "en": "Apa Itu Cahaya Cepat (Fast Light)?", "id": "Perambatan Dengan Kecepatan Grup Melebihi C." },
  { "en": "Apakah Cahaya Cepat Melanggar Kausalitas?", "id": "Tidak, Karena Informasi Tidak Bergerak Lebih Cepat." },
  { "en": "Apa Itu Kecepatan Grup?", "id": "Kecepatan Perambatan Amplitudo Pulsa." },
  { "en": "Apa Itu Kecepatan Fasa?", "id": "Kecepatan Perambatan Fasa Gelombang." },
  { "en": "Apa Itu Electromagnetically Induced Transparency (EIT)?", "id": "Membuat Medium Opak Menjadi Transparan." },
  { "en": "Apa Itu Laser Pendingin Optik?", "id": "Mendinginkan Benda Makroskopik Menggunakan Radiasi." },
  { "en": "Apa Itu Levitasi Optik?", "id": "Menggunakan Tekanan Radiasi Untuk Mengangkat Objek." },
  { "en": "Apa Itu Tekanan Radiasi?", "id": "Tekanan Yang Diberikan Oleh Radiasi Elektromagnetik." },
  { "en": "Apa Itu Perangkap Magneto-Optik (MOT)?", "id": "Perangkap Atom Menggunakan Laser Dan Medan Magnet." },
  { "en": "Apa Itu Sisir Frekuensi Astro (Astro-comb)?", "id": "Sisir Frekuensi Untuk Kalibrasi Spektrograf Astronomi." },
  { "en": "Apa Itu Spektrograf?", "id": "Alat Yang Memisahkan Cahaya Menjadi Spektrum." },
  { "en": "Apa Itu Kaca Fused Silica?", "id": "Bentuk Kaca Berkualitas Tinggi Yang Sangat Murni." },
  { "en": "Apa Itu Safir?", "id": "Kristal Aluminium Oksida Untuk Komponen Optik." },
  { "en": "Apa Itu Zinc Selenide (ZnSe)?", "id": "Bahan Transparan Untuk Optik Inframerah." },
  { "en": "Apa Itu Germanium (Ge)?", "id": "Bahan Semikonduktor Untuk Lensa Inframerah." },
  { "en": "Apa Itu Fluorit Kalsium (CaF2)?", "id": "Bahan Optik Untuk Daerah UV Dan Inframerah." },
  { "en": "Apa Itu Optik Biner?", "id": "Elemen Optik Difraksi Dengan Dua Tingkat Permukaan." },
  { "en": "Apa Itu Hologram Yang Dihasilkan Komputer (CGH)?", "id": "Hologram Yang Dibuat Dengan Perhitungan Komputer." },
  { "en": "Apa Itu Spatial Light Modulator (SLM)?", "id": "Perangkat Yang Memodulasi Sinar Cahaya." },
  { "en": "Apa Itu Digital Micromirror Device (DMD)?", "id": "Contoh SLM Yang Digunakan Proyektor DLP." },
  { "en": "Apa Itu Liquid Crystal on Silicon (LCoS)?", "id": "Teknologi Tampilan Reflektif." },
  { "en": "Apa Itu Korelator Optik?", "id": "Sistem Yang Melakukan Korelasi Menggunakan Optik." },
  { "en": "Apa Itu Pengenalan Pola Optik?", "id": "Mengidentifikasi Pola Dalam Gambar Menggunakan Optik." },
  { "en": "Apa Itu Filter Van der Lugt?", "id": "Filter Kompleks Untuk Pengenalan Pola." },
  { "en": "Apa Itu Joint Transform Correlator (JTC)?", "id": "Arsitektur Korelator Optik Lainnya." },
  { "en": "Apa Itu Sistem Bayangan Inkoheren?", "id": "Sistem Pencitraan Yang Menggunakan Cahaya Inkoheren." },
  { "en": "Apa Itu Optical Transfer Function (OTF)?", "id": "Transformasi Fourier Dari Respon Impuls." },
  { "en": "Apa Itu Modulation Transfer Function (MTF)?", "id": "Amplitudo Dari Optical Transfer Function." },
  { "en": "Apa Itu Phase Transfer Function (PTF)?", "id": "Fasa Dari Optical Transfer Function." },
  { "en": "Apa Itu Point Spread Function (PSF)?", "id": "Respon Sistem Pencitraan Terhadap Sumber Titik." },
  { "en": "Apa Itu Dekonvolusi?", "id": "Proses Untuk Mengurangi Keburaman Dalam Gambar." },
  { "en": "Apa Itu Optik Atmosfer?", "id": "Studi Tentang Sifat Optik Atmosfer." },
  { "en": "Apa Itu Pelangi?", "id": "Fenomena Optik Akibat Dispersi Cahaya." },
  { "en": "Apa Itu Halo?", "id": "Cincin Cahaya Di Sekitar Matahari Atau Bulan." },
  { "en": "Apa Itu Fatamorgana (Mirage)?", "id": "Fenomena Optik Akibat Pembiasan Cahaya." },
  { "en": "Apa Itu Kilau (Scintillation)?", "id": "Fluktuasi Cahaya Bintang Akibat Turbulensi." },
  { "en": "Apa Itu Seeing Dalam Astronomi?", "id": "Ukuran Efek Buram Atmosfer." },
  { "en": "Apa Itu Laser Guide Star?", "id": "Bintang Buatan Untuk Sistem Optik Adaptif." },
  { "en": "Apa Itu Oseanografi Optik?", "id": "Studi Sifat Optik Air Laut." },
  { "en": "Apa Itu Warna Laut?", "id": "Warna Air Laut Akibat Hamburan Cahaya." },
  { "en": "Apa Itu Disk Secchi?", "id": "Alat Untuk Mengukur Kekeruhan Air." },
  { "en": "Apa Itu LED Putih?", "id": "LED Yang Menghasilkan Cahaya Tampak Putih." },
  { "en": "Bagaimana LED Putih Dibuat?", "id": "Menggunakan LED Biru Dengan Lapisan Fosfor." },
  { "en": "Apa Itu Color Rendering Index (CRI)?", "id": "Ukuran Kemampuan Sumber Cahaya." },
  { "en": "Apa Itu Correlated Color Temperature (CCT)?", "id": "Ukuran Warna Cahaya Yang Dipancarkan." },
  { "en": "Apa Itu Efisiensi Kuantum Eksternal?", "id": "Rasio Foton Dipancarkan Terhadap Elektron Diinjeksikan." },
  { "en": "Apa Itu Efisiensi Kuantum Internal?", "id": "Rasio Foton Dihasilkan Terhadap Elektron Di Lapisan." },
  { "en": "Apa Itu Efisiensi Ekstraksi Cahaya?", "id": "Rasio Foton Dipancarkan Terhadap Foton Dihasilkan." },
  { "en": "Apa Itu LED Droop?", "id": "Penurunan Efisiensi LED Pada Arus Tinggi." },
  { "en": "Apa Itu Solid-State Lighting (SSL)?", "id": "Teknologi Pencahayaan Menggunakan LED Dan OLED." },
  { "en": "Apa Itu Luminance?", "id": "Intensitas Cahaya Per Satuan Luas." },
  { "en": "Apa Itu Illuminance?", "id": "Total Fluks Cahaya Yang Mengenai Permukaan." },
  { "en": "Apa Itu Radiance?", "id": "Fluks Radiasi Yang Dipancarkan Per Satuan." },
  { "en": "Apa Itu Irradiance?", "id": "Fluks Radiasi Yang Diterima Permukaan." },
  { "en": "Apa Itu Hukum Kosinus Lambert?", "id": "Menjelaskan Intensitas Cahaya Dari Permukaan Difus." },
  { "en": "Apa Itu Permukaan Lambertian?", "id": "Permukaan Pemantul Difus Yang Ideal." },
  { "en": "Apa Itu Sphere Integrasi?", "id": "Alat Untuk Mengukur Fluks Optik." },
  { "en": "Apa Itu Bidirectional Reflectance Distribution Function (BRDF)?", "id": "Fungsi Yang Mendefinisikan Bagaimana Cahaya Dipantulkan." },
  { "en": "Apa Itu Albedo?", "id": "Ukuran Daya Pantul Difus Suatu Permukaan." },
  { "en": "Apa Itu Fotografi Komputasi?", "id": "Menggunakan Pemrosesan Digital Alih-alih Optik." },
  { "en": "Apa Itu High Dynamic Range (HDR) Imaging?", "id": "Teknik Untuk Mereproduksi Rentang Luminositas." },
  { "en": "Apa Itu Plenoptic Camera?", "id": "Kamera Bidang Cahaya Yang Menangkap Informasi." },
  { "en": "Apa Itu Bidang Cahaya (Light Field)?", "id": "Fungsi Yang Menjelaskan Jumlah Cahaya." },
  { "en": "Apa Itu Time-of-Flight (ToF) Camera?", "id": "Kamera Yang Mengukur Jarak Berdasarkan Kecepatan." },
  { "en": "Apa Itu Structured Light Scanning?", "id": "Memproyeksikan Pola Cahaya Untuk Pencitraan 3D." },
  { "en": "Apa Itu Array Pandu Gelombang Optik?", "id": "Kumpulan Pandu Gelombang Untuk Memanipulasi Sinar." },
  { "en": "Apa Itu Optical Phased Array?", "id": "Mengontrol Fasa Cahaya Untuk Mengarahkan Sinar." },
  { "en": "Apa Itu Optogenetika?", "id": "Mengontrol Neuron Menggunakan Cahaya Dan Rekayasa Genetika." },
  { "en": "Apa Itu Channelrhodopsin?", "id": "Protein Peka Cahaya Yang Digunakan Dalam Optogenetika." },
  { "en": "Apa Itu Two-Photon Microscopy?", "id": "Teknik Mikroskopi Fluoresensi Untuk Pencitraan Dalam." },
  { "en": "Apa Keuntungan Two-Photon Microscopy?", "id": "Penetrasi Lebih Dalam Dan Kerusakan Lebih Rendah." },
  { "en": "Apa Itu Super-Resolution Microscopy?", "id": "Teknik Mikroskopi Yang Melebihi Batas Difraksi." },
  { "en": "Apa Itu Stimulated Emission Depletion (STED) Microscopy?", "id": "Contoh Mikroskopi Super-Resolusi." },
  { "en": "Apa Itu Photoactivated Localization Microscopy (PALM)?", "id": "Teknik Mikroskopi Super-Resolusi Lainnya." },
  { "en": "Apa Itu Stochastic Optical Reconstruction Microscopy (STORM)?", "id": "Teknik Serupa Dengan PALM." },
  { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Menggunakan Sirkuit Analog Meniru Arsitektur Saraf." },
  { "en": "Apa Itu Fotonika Neuromorfik?", "id": "Komputasi Neuromorfik Menggunakan Komponen Fotonik." },
  { "en": "Apa Itu Sirkuit Terintegrasi Fotonik?", "id": "Nama Lain Untuk Photonic Integrated Circuit (PIC)." },
  { "en": "Apa Itu Optik Gelombang Terpandu?", "id": "Mempelajari Perambatan Cahaya Dalam Pandu Gelombang." },
  { "en": "Apa Itu Mode Pandu Gelombang?", "id": "Pola Medan Elektromagnetik Dalam Pandu Gelombang." },
  { "en": "Apa Itu Mode Fundamental?", "id": "Mode Orde Terendah Dalam Pandu Gelombang." },
  { "en": "Apa Itu Cutoff Wavelength?", "id": "Panjang Gelombang Di Atas Mana Mode Tidak Merambat." },
  { "en": "Apa Itu Kopling Pandu Gelombang?", "id": "Transfer Daya Optik Antara Pandu Gelombang." },
  { "en": "Apa Itu Directional Coupler?", "id": "Komponen Empat Port Untuk Membagi Cahaya." },
  { "en": "Apa Itu Rasio Kopling?", "id": "Persentase Daya Yang Ditransfer." },
  { "en": "Apa Itu Multi-Mode Interference (MMI) Coupler?", "id": "Coupler Berbasis Interferensi Antar Mode." },
  { "en": "Apa Itu Arrayed Waveguide Grating (AWG)?", "id": "Komponen Untuk Multiplexing Panjang Gelombang." },
  { "en": "Apa Itu Fotonika Kuantum?", "id": "Mempelajari Efek Kuantum Yang Melibatkan Foton." },
  { "en": "Apa Itu Sumber Foton Tunggal?", "id": "Sumber Cahaya Yang Memancarkan Satu Foton." },
  { "en": "Mengapa Sumber Foton Tunggal Penting?", "id": "Untuk Komputasi Kuantum Dan Kriptografi." },
  { "en": "Apa Itu Pusat Warna (Color Center)?", "id": "Cacat Titik Kristal Yang Dapat Memancarkan Foton." },
  { "en": "Apa Itu Nitrogen-Vacancy (NV) Center in Diamond?", "id": "Pusat Warna Yang Populer Untuk Fotonika Kuantum." },
  { "en": "Apa Itu Komputasi Kuantum Optik Linier?", "id": "Komputasi Kuantum Menggunakan Komponen Optik Pasif." },
  { "en": "Apa Itu Gerbang CNOT?", "id": "Gerbang Logika Kuantum Fundamental." },
  { "en": "Apa Itu Efek Fotolistrik Terbalik?", "id": "Emisi Foton Akibat Tumbukan Elektron." },
  { "en": "Apa Itu Katodoluminesensi?", "id": "Emisi Cahaya Saat Elektron Menumbuk Bahan." },
  { "en": "Apa Itu Termoluminesensi?", "id": "Emisi Cahaya Saat Bahan Dipanaskan." },
  { "en": "Apa Itu Dosimeter Termoluminesen?", "id": "Mengukur Paparan Radiasi Pengion." },
  { "en": "Apa Itu Sonoluminesensi?", "id": "Emisi Cahaya Dari Gelembung Yang Berimplosi." },
  { "en": "Apa Itu Triboluminesensi?", "id": "Cahaya Yang Dihasilkan Dari Fraktur Mekanis." },
  { "en": "Apa Itu Laser Pendingin Doppler?", "id": "Tahap Pertama Dalam Pendinginan Laser." },
  { "en": "Apa Itu Pendinginan Sisyphus?", "id": "Mekanisme Pendinginan Laser Di Bawah Batas Doppler." },
  { "en": "Apa Itu Laser Semikonduktor Oksida?", "id": "Jenis Dioda Laser Yang Baru Dikembangkan." },
  { "en": "Apa Itu Laser Polimer?", "id": "Laser Solid-State Yang Menggunakan Polimer." },
  { "en": "Apa Itu Bahan Scintillator?", "id": "Bahan Yang Berpendar Saat Terkena Radiasi." },
  { "en": "Apa Aplikasi Scintillator?", "id": "Deteksi Partikel, Pencitraan Medis, Dan Keamanan." },
  { "en": "Apa Itu Positron Emission Tomography (PET)?", "id": "Teknik Pencitraan Medis Menggunakan Positron." },
  { "en": "Bagaimana Scintillator Digunakan Dalam PET?", "id": "Untuk Mendeteksi Sinar Gamma Dari Anihilasi." },
  { "en": "Apa Itu Spektroskopi Korelasi Fluoresensi?", "id": "Menganalisis Fluktuasi Intensitas Fluoresensi." },
  { "en": "Apa Itu Förster Resonance Energy Transfer (FRET)?", "id": "Mekanisme Transfer Energi Antara Molekul." },
  { "en": "Apa Itu Fluorescence Lifetime Imaging Microscopy (FLIM)?", "id": "Pencitraan Berdasarkan Waktu Hidup Fluoresensi." },
  { "en": "Apa Itu Waktu Hidup Fluoresensi?", "id": "Waktu Rata-rata Molekul Tetap Terekstasi." },
  { "en": "Apa Itu Laser Pemindaian Konfokal?", "id": "Teknik Mikroskopi Untuk Meningkatkan Resolusi Optik." },
  { "en": "Apa Itu Pinhole?", "id": "Apertur Kecil Yang Penting Dalam Mikroskop Konfokal." },
  { "en": "Apa Itu Optika Terabuka?", "id": "Penggunaan Ruang Bebas Untuk Transmisi Optik." },
  { "en": "Apa Tantangan Komunikasi Optik Ruang Bebas?", "id": "Penyerapan Atmosfer, Hamburan, Dan Turbulensi." },
  { "en": "Apa Itu Laser Inter-Satelit?", "id": "Komunikasi Optik Antara Satelit." },
  { "en": "Apa Itu Komunikasi Optik Bawah Air?", "id": "Menggunakan Cahaya Untuk Komunikasi Di Bawah Air." },
  { "en": "Warna Cahaya Apa Yang Terbaik Untuk Bawah Air?", "id": "Biru Dan Hijau." },
  { "en": "Apa Itu Efek Cherenkov?", "id": "Radiasi Saat Partikel Melebihi Kecepatan Cahaya." },
  { "en": "Apa Itu Detektor Cherenkov?", "id": "Detektor Partikel Berbasis Radiasi Cherenkov." },
  { "en": "Apa Itu Aerogel?", "id": "Bahan Padat Ultraringan Berpori." },
  { "en": "Bagaimana Aerogel Digunakan Dalam Detektor Cherenkov?", "id": "Sebagai Medium Radiator." },
  { "en": "Apa Itu Optik Sinar-X?", "id": "Mempelajari Manipulasi Sinar-X." },
  { "en": "Apa Itu Lensa Fresnel Sinar-X?", "id": "Lensa Difraksi Untuk Memfokuskan Sinar-X." },
  { "en": "Apa Itu Cermin Sinar-X?", "id": "Cermin Yang Bekerja Pada Sudut Datang Sangat Kecil." },
  { "en": "Apa Itu Grazing Incidence?", "id": "Sudut Datang Yang Sangat Dekat Dengan Permukaan." },
  { "en": "Apa Itu Radiasi Synchrotron?", "id": "Sumber Sinar-X Intensitas Tinggi." },
  { "en": "Apa Itu Laser Elektron Bebas Sinar-X (XFEL)?", "id": "Laser Yang Menghasilkan Pulsa Sinar-X Koheren." },
  { "en": "Apa Itu Cahaya Terstruktur (Structured Light)?", "id": "Sinar Cahaya Dengan Pola Spasial Khusus." },
  { "en": "Apa Itu Interferometri Speckle?", "id": "Teknik Untuk Mengukur Deformasi Objek." },
  { "en": "Apa Itu Shearografi?", "id": "Teknik Mirip Interferometri Speckle." },
  { "en": "Apa Itu Fotogrametri?", "id": "Ilmu Pengukuran Dari Foto." },
  { "en": "Apa Itu Pemrosesan Sinyal Analog?", "id": "Memproses Sinyal Dalam Bentuk Analognya." },
  { "en": "Apa Itu Teori Difraksi Skalar?", "id": "Pendekatan Sederhana Untuk Memodelkan Difraksi." },
  { "en": "Apa Itu Teori Difraksi Vektor?", "id": "Pendekatan Lebih Akurat Yang Mempertimbangkan Polarisasi." },
  { "en": "Apa Itu Prinsip Huygens-Fresnel?", "id": "Setiap Titik Di Muka Gelombang Adalah Sumber." },
  { "en": "Apa Itu Integral Difraksi Kirchhoff?", "id": "Rumus Matematis Untuk Medan Terdifraksi." },
  { "en": "Apa Itu Difraksi Fraunhofer?", "id": "Difraksi Medan Jauh." },
  { "en": "Apa Itu Difraksi Fresnel?", "id": "Difraksi Medan Dekat." },
  { "en": "Apa Itu Bilangan Fresnel?", "id": "Parameter Untuk Membedakan Difraksi Fresnel/Fraunhofer." },
  { "en": "Apa Itu Optik Isi Ulang (Rechargeable Optics)?", "id": "Komponen Optik Dengan Sifat Dapat Diubah." },
  { "en": "Apa Itu Material Fotorefraktif?", "id": "Material Yang Mengubah Indeks Biasnya." },
  { "en": "Apa Itu Konjugasi Fasa Optik?", "id": "Menghasilkan Gelombang Dengan Fasa Terbalik." },
  { "en": "Apa Manfaat Dari Konjugasi Fasa?", "id": "Dapat Mengoreksi Distorsi Dalam Sinar Optik." },
  { "en": "Apa Itu Cermin Konjugasi Fasa?", "id": "Perangkat Yang Melakukan Konjugasi Fasa." },
  { "en": "Apa Itu Elektroluminesensi Anorganik?", "id": "Emisi Cahaya Dari Fosfor Anorganik." },
  { "en": "Apa Itu Tampilan Elektroluminesen?", "id": "Tampilan Datar Berbasis Elektroluminesensi." },
  { "en": "Apa Itu Saklar Fotonik?", "id": "Saklar Yang Menggunakan Foton Untuk Operasi." },
  { "en": "Apa Itu Jaringan Semua-Optik (All-Optical Network)?", "id": "Jaringan Dimana Sinyal Tetap Dalam Domain Optik." },
  { "en": "Apa Itu Regenerasi 3R?", "id": "Re-amplifying, Re-shaping, Dan Re-timing." },
  { "en": "Apa Itu Regenerasi Semua-Optik?", "id": "Melakukan Regenerasi 3R Tanpa Konversi Elektronik." },
  { "en": "Apa Itu Serat Optik Aktif?", "id": "Serat Optik Yang Didoping Dengan Elemen Aktif." },
  { "en": "Apa Itu Kaca Kalkogenida?", "id": "Kaca Berbasis Belerang, Selenium, Atau Telurium." },
  { "en": "Apa Sifat Utama Kaca Kalkogenida?", "id": "Transparan Di Inframerah Dan Non-Linieritas Tinggi." },
  { "en": "Apa Itu Kaca ZBLAN?", "id": "Jenis Kaca Fluorida Untuk Aplikasi Inframerah." },
  { "en": "Apa Itu Kristal Cair Feroelektrik?", "id": "Jenis Kristal Cair Dengan Waktu Respon Cepat." },
  { "en": "Apa Itu Modulator Magneto-Optik?", "id": "Memodulasi Cahaya Menggunakan Efek Magneto-Optik." },
  { "en": "Apa Itu Sensor Fotonik Kristal?", "id": "Sensor Berbasis Struktur Kristal Fotonik." },
  { "en": "Apa Itu Lab-on-a-chip?", "id": "Perangkat Yang Mengintegrasikan Fungsi Laboratorium." },
  { "en": "Bagaimana Optik Terintegrasi Mendukung Lab-on-a-chip?", "id": "Menyediakan Kemampuan Deteksi Dan Manipulasi Optik." },
  { "en": "Apa Itu Tampilan Medan Cahaya?", "id": "Tampilan Yang Mereproduksi Bidang Cahaya." },
  { "en": "Apa Itu Tampilan Volumetrik?", "id": "Tampilan Grafis Yang Menciptakan Representasi 3D." },
  { "en": "Apa Itu Tampilan Retina Virtual?", "id": "Memproyeksikan Gambar Langsung Ke Retina." },
  { "en": "Apa Itu Head-Up Display (HUD)?", "id": "Tampilan Transparan Yang Menampilkan Data." },
  { "en": "Apa Itu Head-Mounted Display (HMD)?", "id": "Perangkat Tampilan Yang Dipakai Di Kepala." },
  { "en": "Apa Itu Realitas Tertambah (Augmented Reality)?", "id": "Menggabungkan Grafik Komputer Dengan Dunia Nyata." },
  { "en": "Apa Itu Realitas Maya (Virtual Reality)?", "id": "Simulasi Imersif Lingkungan Buatan." },
  { "en": "Apa Itu Foveated Rendering?", "id": "Teknik Rendering Yang Mengurangi Beban Kerja." },
  { "en": "Apa Itu Pelacakan Mata (Eye Tracking)?", "id": "Mengukur Titik Pandang Atau Gerakan Mata." },
  { "en": "Apa Itu Kedokteran Optik?", "id": "Penggunaan Optik Dan Fotonika Dalam Kedokteran." },
  { "en": "Apa Itu Pulse Oximeter?", "id": "Mengukur Saturasi Oksigen Dalam Darah." },
  { "en": "Bagaimana Pulse Oximeter Bekerja?", "id": "Mengukur Penyerapan Cahaya Merah Dan Inframerah." },
  { "en": "Apa Itu Endoskopi?", "id": "Prosedur Medis Menggunakan Endoskop." },
  { "en": "Apa Itu Endoskop?", "id": "Instrumen Optik Untuk Melihat Bagian Dalam Tubuh." },
  { "en": "Apa Itu Kapsul Endoskopi?", "id": "Kamera Seukuran Pil Yang Ditelan." },
  { "en": "Apa Itu Spektroskopi Dekat Inframerah?", "id": "Menggunakan Wilayah Spektrum Dekat Inframerah." },
  { "en": "Apa Itu Pemrosesan Material Laser?", "id": "Menggunakan Laser Untuk Memodifikasi Sifat Material." },
  { "en": "Apa Itu Laser Induced Breakdown Spectroscopy (LIBS)?", "id": "Teknik Analisis Komposisi Kimia." },
  { "en": "Apa Itu Pengerasan Permukaan Laser?", "id": "Meningkatkan Kekerasan Permukaan Logam." },
  { "en": "Apa Itu Laser Cladding?", "id": "Menambahkan Lapisan Material Menggunakan Laser." },
  { "en": "Apa Itu Matrix-Assisted Laser Desorption/Ionization (MALDI)?", "id": "Teknik Ionisasi Lunak Dalam Spektrometri Massa." },
  { "en": "Apa Itu Optik Atmosfer Terpadu?", "id": "Studi Tentang Penjalaran Sinar Optik." },
  { "en": "Apa Itu Angka Strehl?", "id": "Ukuran Kualitas Kinerja Pencitraan Optik." },
  { "en": "Apa Itu Interferometri Bintik (Speckle Interferometry)?", "id": "Teknik Resolusi Tinggi Dalam Astronomi." },
  { "en": "Apa Itu Fotometri Apertur?", "id": "Mengukur Kecerahan Objek Astronomi." },
  { "en": "Apa Itu Fotometer?", "id": "Instrumen Untuk Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Polarimeter?", "id": "Instrumen Untuk Mengukur Polarisasi Cahaya." },
  { "en": "Apa Itu Sakarimeter?", "id": "Jenis Polarimeter Untuk Mengukur Konsentrasi Gula." },
  { "en": "Apa Itu Refraktometer?", "id": "Instrumen Untuk Mengukur Indeks Bias." },
  { "en": "Apa Itu Nilai Abbe?", "id": "Ukuran Dispersi Material Optik." },
  { "en": "Nilai Abbe Tinggi Berarti Apa?", "id": "Dispersi Kromatik Yang Rendah." },
  { "en": "Apa Itu Kaca Crown?", "id": "Jenis Kaca Optik Dengan Dispersi Rendah." },
  { "en": "Apa Itu Kaca Flint?", "id": "Jenis Kaca Optik Dengan Dispersi Tinggi." },
  { "en": "Apa Itu Lensa Akromatik?", "id": "Lensa Yang Dirancang Mengurangi Aberasi Kromatik." },
  { "en": "Apa Itu Lensa Apokromatik?", "id": "Lensa Dengan Koreksi Aberasi Kromatik Lebih Baik." },
  { "en": "Apa Itu Desain Optik?", "id": "Proses Merancang Komponen Dan Sistem Optik." },
  { "en": "Apa Itu Perangkat Lunak Ray Tracing?", "id": "Alat Untuk Simulasi Dan Desain Sistem Optik." },
  { "en": "Sebutkan Contoh Perangkat Lunak Desain Optik?", "id": "Zemax, CODE V, Dan OSLO." },
  { "en": "Apa Itu Permukaan Optik Bebas (Freeform)?", "id": "Permukaan Tanpa Sumbu Simetri Rotasi." },
  { "en": "Apa Itu Metrologi Optik?", "id": "Ilmu Pengukuran Menggunakan Cahaya." },
  { "en": "Apa Itu Interferometri Laser?", "id": "Menggunakan Laser Untuk Pengukuran Jarak Presisi." },
  { "en": "Apa Itu Profilometer Optik?", "id": "Mengukur Topografi Permukaan Secara Non-kontak." },
  { "en": "Apa Itu Teodolit?", "id": "Instrumen Presisi Untuk Mengukur Sudut." },
  { "en": "Apa Itu Total Station?", "id": "Teodolit Elektronik Terintegrasi Dengan Pengukur Jarak." },
  { "en": "Apa Itu Fotokimia?", "id": "Cabang Kimia Yang Mempelajari Reaksi Kimia." },
  { "en": "Apa Itu Hasil Kuantum?", "id": "Ukuran Efisiensi Proses Yang Diinduksi Foton." },
  { "en": "Apa Itu Hukum Stark-Einstein?", "id": "Setiap Foton Hanya Dapat Mengeksitasi Satu Molekul." },
  { "en": "Apa Itu Fotosintesis?", "id": "Proses Mengubah Energi Cahaya Menjadi Energi Kimia." },
  { "en": "Apa Itu Klorofil?", "id": "Pigmen Hijau Yang Penting Untuk Fotosintesis." },
  { "en": "Apa Itu Visi Komputer?", "id": "Bidang Yang Membuat Komputer Dapat Melihat." },
  { "en": "Apa Itu Segmentasi Gambar?", "id": "Mempartisi Gambar Digital Menjadi Segmen." },
  { "en": "Apa Itu Deteksi Tepi?", "id": "Teknik Mengidentifikasi Titik Dengan Perubahan Kecerahan." },
  { "en": "Apa Itu Detektor Tepi Canny?", "id": "Algoritma Populer Untuk Deteksi Tepi." },
  { "en": "Apa Itu Transformasi Hough?", "id": "Teknik Ekstraksi Fitur Untuk Mendeteksi Bentuk." },
  { "en": "Apa Itu Optical Character Recognition (OCR)?", "id": "Mengubah Gambar Teks Menjadi Teks Mesin." },
  { "en": "Apa Itu Kode Batang (Barcode)?", "id": "Representasi Data Optik Yang Dapat Dibaca Mesin." },
  { "en": "Apa Itu Kode QR (Quick Response)?", "id": "Jenis Kode Batang Matriks Dua Dimensi." },
  { "en": "Apa Itu Biometrik?", "id": "Pengenalan Manusia Berdasarkan Karakteristik." },
  { "en": "Apa Itu Pengenalan Wajah?", "id": "Teknologi Biometrik Yang Mengidentifikasi Wajah." },
  { "en": "Apa Itu Pemindaian Iris?", "id": "Teknik Biometrik Menggunakan Pola Iris Mata." },
  { "en": "Apa Itu Pemindaian Retina?", "id": "Biometrik Yang Memetakan Pola Pembuluh Darah." },
  { "en": "Apa Itu Pengenalan Sidik Jari?", "id": "Menggunakan Pola Unik Pada Ujung Jari." },
  { "en": "Apa Itu Pencahayaan Terstruktur?", "id": "Nama Lain Untuk Structured Light." },
  { "en": "Apa Itu Kamera Plenoptik?", "id": "Nama Lain Untuk Light Field Camera." },
  { "en": "Apa Itu Fotonika Silikon?", "id": "Nama Lain Untuk Silicon Photonics." },
  { "en": "Apa Itu Komputasi Optik?", "id": "Nama Lain Untuk Perhitungan Optik." },
  { "en": "Apa Itu Pemrosesan Sinyal All-Optical?", "id": "Memproses Sinyal Sepenuhnya Dalam Domain Optik." },
  { "en": "Apa Itu Transistor Optik?", "id": "Saklar Optik Yang Menguatkan Sinyal Optik." },
  { "en": "Apa Itu Bistabilitas?", "id": "Memiliki Dua Keadaan Output Stabil." },
  { "en": "Apa Itu Efek Fotovoltaik Lateral?", "id": "Pembangkitan Tegangan Lateral Pada Sambungan." },
  { "en": "Apa Itu Position Sensitive Detector (PSD)?", "id": "Sensor Optoelektronik Yang Mengukur Posisi." },
  { "en": "Apa Itu Kuadran Fotodioda?", "id": "Fotodioda Yang Dibagi Menjadi Empat Kuadran." },
  { "en": "Apa Aplikasi Kuadran Fotodioda?", "id": "Pelacakan Sinar, Penjajaran, Dan Pengukuran Posisi." },
  { "en": "Apa Itu Optik Terintegrasi Lithium Niobate?", "id": "Platform Populer Untuk Modulator Berkecepatan Tinggi." },
  { "en": "Apa Itu Lithium Niobate (LiNbO3)?", "id": "Bahan Buatan Dengan Sifat Elektro-Optik Kuat." },
  { "en": "Apa Itu Modulator Mach-Zehnder?", "id": "Modulator Intensitas Berbasis Interferometer." },
  { "en": "Apa Itu Tegangan Setengah Gelombang (Vπ)?", "id": "Tegangan Untuk Pergeseran Fasa 180 Derajat." },
  { "en": "Apa Itu Rasio Ekstingsi?", "id": "Rasio Daya Optik Maksimum Dan Minimum." },
  { "en": "Apa Itu Chirp?", "id": "Perubahan Frekuensi Sesaat Selama Modulasi." },
  { "en": "Apa Itu Optik Medan Terahertz?", "id": "Mempelajari Interaksi Materi Dengan Pulsa Terahertz." },
  { "en": "Apa Itu Time-Domain Spectroscopy (TDS)?", "id": "Teknik Spektroskopi Menggunakan Pulsa Pendek." },
  { "en": "Apa Itu Terahertz TDS?", "id": "Spektroskopi Dalam Domain Waktu Di Daerah Terahertz." },
  { "en": "Apa Itu Deteksi Elektro-Optik?", "id": "Teknik Untuk Mendeteksi Pulsa Terahertz." },
  { "en": "Apa Itu Laser Femtosekon?", "id": "Laser Yang Menghasilkan Pulsa Sangat Pendek." },
  { "en": "Apa Itu Spektroskopi Pump-Probe?", "id": "Teknik Untuk Mempelajari Dinamika Ultrafast." },
  { "en": "Apa Itu Saturable Absorber?", "id": "Material Yang Transmisinya Meningkat Dengan Intensitas." },
  { "en": "Bagaimana Saturable Absorber Digunakan Dalam Laser?", "id": "Untuk Memulai Proses Mode-Locking." },
  { "en": "Apa Itu Kerr Lens Mode-locking (KLM)?", "id": "Mekanisme Mode-Locking Berbasis Efek Kerr." },
  { "en": "Apa Itu Dispersi Kecepatan Grup (GVD)?", "id": "Ketergantungan Kecepatan Grup Pada Frekuensi." },
  { "en": "Apa Efek Dispersi Pada Pulsa Pendek?", "id": "Menyebabkan Penyebaran Atau Pelebaran Pulsa." },
  { "en": "Apa Itu Kompresi Pulsa?", "id": "Membuat Pulsa Optik Menjadi Lebih Pendek." },
  { "en": "Apa Itu Cermin Chirped?", "id": "Cermin Dielektrik Untuk Kompensasi Dispersi." },
  { "en": "Apa Itu Phase-Matching?", "id": "Syarat Untuk Interaksi Non-Linier Efisien." },
  { "en": "Apa Itu Quasi-Phase-Matching (QPM)?", "id": "Teknik Untuk Mencapai Phase-Matching." },
  { "en": "Apa Itu Periodically Poled Lithium Niobate (PPLN)?", "id": "Material QPM Yang Umum Digunakan." },
  { "en": "Apa Itu Optical Rectification?", "id": "Menghasilkan Medan Listrik DC Dari Cahaya." },
  { "en": "Apa Itu Emisi Termionik?", "id": "Emisi Elektron Dari Permukaan Yang Dipanaskan." },
  { "en": "Apa Itu Tabung Vakum?", "id": "Perangkat Elektronik Berbasis Aliran Elektron." },
  { "en": "Apa Itu Cathode Ray Tube (CRT)?", "id": "Tabung Vakum Yang Menghasilkan Gambar." },
  { "en": "Apa Itu Fosfor?", "id": "Bahan Yang Memancarkan Cahaya Saat Dikenai Radiasi." },
  { "en": "Apa Itu Tampilan Elektroluminesen Film Tipis?", "id": "Jenis Tampilan Datar." },
  { "en": "Apa Itu Tampilan Plasma?", "id": "Menggunakan Sel Gas Kecil Untuk Menghasilkan Cahaya." },
  { "en": "Apa Itu Efek Zeeman?", "id": "Pemisahan Garis Spektrum Akibat Medan Magnet." },
  { "en": "Apa Itu Efek Stark?", "id": "Pergeseran Garis Spektrum Akibat Medan Listrik." },
  { "en": "Apa Itu Mekanika Kuantum?", "id": "Teori Fisika Fundamental Pada Skala Atom." },
  { "en": "Apa Itu Fungsi Gelombang?", "id": "Deskripsi Matematis Dari Keadaan Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger?", "id": "Menggambarkan Bagaimana Keadaan Kuantum Berevolusi." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
