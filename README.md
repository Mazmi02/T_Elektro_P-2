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


  { "en": "Apa Itu Konvolusi Sinyal?", "id": "Penggabungan Dua Sinyal Dalam Waktu." },
  { "en": "Apa Itu Korelasi Sinyal?", "id": "Mengukur Kemiripan Antara Dua Sinyal." },
  { "en": "Apa Itu Autokorelasi?", "id": "Korelasi Sinyal Dengan Dirinya Sendiri." },
  { "en": "Apa Fungsi Filter FIR?", "id": "Filter Respon Impuls Terbatas." },
  { "en": "Apa Fungsi Filter IIR?", "id": "Filter Respon Impuls Tak Terbatas." },
  { "en": "Apa Itu Decimation Pada DSP?", "id": "Pengurangan Laju Sampling Sinyal." },
  { "en": "Apa Itu Interpolation Pada DSP?", "id": "Penambahan Laju Sampling Sinyal." },
  { "en": "Apa Itu Fenomena Gibbs?", "id": "Osilasi Pada Tepi Sinyal Kotak." },
  { "en": "Apa Itu Windowing Pada FFT?", "id": "Mengurangi Kebocoran Spektrum Frekuensi." },
  { "en": "Apa Jenis Window FFT Umum?", "id": "Hamming, Hanning, Dan Blackman." },
  { "en": "Apa Itu Zero Padding?", "id": "Menambah Nol Untuk Resolusi Frekuensi." },
  { "en": "Apa Itu Busbar Tembaga?", "id": "Batang Konduktor Distribusi Listrik Utama." },
  { "en": "Apa Warna Busbar Ground?", "id": "Kuning, Strip Hijau." },
  { "en": "Apa Standar Arus Busbar?", "id": "Tergantung Luas Penampang, Dan Suhu." },
  { "en": "Apa Itu Cable Gland?", "id": "Pengunci Kabel Masuk Panel." },
  { "en": "Apa Fungsi Cable Skun Bimetal?", "id": "Menyambung Kabel Alumunium, Ke Tembaga." },
  { "en": "Apa Itu Isolator Tumpu?", "id": "Isolator Duduk, Penyangga Busbar." },
  { "en": "Apa Itu Panel LVMDP?", "id": "Low Voltage Main Distribution Panel." },
  { "en": "Apa Itu Panel SDP?", "id": "Sub Distribution Panel." },
  { "en": "Apa Itu Panel Capacitor Bank?", "id": "Panel Perbaikan Faktor Daya." },
  { "en": "Apa Fungsi Exhaust Fan Panel?", "id": "Membuang Panas Dari Dalam Panel." },
  { "en": "Apa Itu Heater Panel?", "id": "Mencegah Kondensasi Embun Dalam Panel." },
  { "en": "Apa Fungsi Thermostat Panel?", "id": "Mengatur Suhu Kerja Heater Fan." },
  { "en": "Apa Itu Relay Omron MY2?", "id": "Relay Kecil, Dua Kontak Tukar." },
  { "en": "Apa Itu Relay Omron MY4?", "id": "Relay Kecil, Empat Kontak Tukar." },
  { "en": "Apa Itu Base Relay?", "id": "Soket Dudukan Relay." },
  { "en": "Apa Tegangan Koil Relay Umum?", "id": "220 VAC, Atau 24 VDC." },
  { "en": "Apa Itu Kelas Isolasi Motor F?", "id": "Tahan Panas Hingga 155 Derajat." },
  { "en": "Apa Itu Kelas Isolasi Motor H?", "id": "Tahan Panas Hingga 180 Derajat." },
  { "en": "Apa Itu Service Factor Motor?", "id": "Kemampuan Motor Menahan Beban Lebih." },
  { "en": "Apa Arti Duty Cycle S1?", "id": "Motor Bekerja Terus Menerus." },
  { "en": "Apa Arti Duty Cycle S2?", "id": "Motor Bekerja Waktu Singkat." },
  { "en": "Apa Itu Nameplate Motor?", "id": "Plat Identitas Spesifikasi Motor." },
  { "en": "Apa Itu Frame Size Motor?", "id": "Ukuran Dimensi Fisik Standar Motor." },
  { "en": "Apa Itu Poros Motor?", "id": "Batang Besi Pemutar Beban." },
  { "en": "Apa Itu Keyway Poros?", "id": "Parit Kunci Pengikat Puli." },
  { "en": "Apa Itu Coupling Motor?", "id": "Penyambung Poros Motor Ke Beban." },
  { "en": "Apa Itu Alignment Motor?", "id": "Meneluruskan Poros Motor Dan Beban." },
  { "en": "Apa Akibat Misalignment?", "id": "Getaran Tinggi, Dan Bearing Rusak." },
  { "en": "Apa Itu Soft Foot Motor?", "id": "Kaki Motor Tidak Rata Lantai." },
  { "en": "Apa Itu Early Effect?", "id": "Modulasi Lebar Basis Transistor Bipolar." },
  { "en": "Apa Itu Body Effect MOSFET?", "id": "Perubahan Threshold Akibat Tegangan Source." },
  { "en": "Apa Itu Channel Length Modulation?", "id": "Efek Pendekan Kanal Pada MOSFET." },
  { "en": "Apa Itu Subthreshold Conduction?", "id": "Arus Bocor Saat MOSFET Mati." },
  { "en": "Apa Itu Electromigration?", "id": "Perpindahan Atom Logam Akibat Arus." },
  { "en": "Apa Bahaya Electromigration?", "id": "Jalur IC Putus, Atau Short." },
  { "en": "Apa Itu Latch-Up Pada CMOS?", "id": "Short Circuit Internal Parasitik Thyristor." },
  { "en": "Apa Penyebab Latch-Up?", "id": "Tegangan Input Melebihi Suplai." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Peluahan Listrik Statis Tegangan Tinggi." },
  { "en": "Apa Alat Pencegah ESD?", "id": "Gelang Antistatis, Dan Meja Grounding." },
  { "en": "Apa Itu Protokol MQTT?", "id": "Message Queuing Telemetry Transport." },
  { "en": "Apa Sifat Protokol MQTT?", "id": "Ringan Hemat Bandwidth Untuk IoT." },
  { "en": "Apa Itu MQTT Broker?", "id": "Server Perantara Pesan MQTT." },
  { "en": "Apa Itu Topic MQTT?", "id": "Alamat Judul Pesan Data." },
  { "en": "Apa Itu Publish Subscribe?", "id": "Metode Komunikasi Protokol MQTT." },
  { "en": "Apa Itu QoS Level 0?", "id": "Kirim Sekali, Tanpa Jaminan." },
  { "en": "Apa Itu QoS Level 1?", "id": "Kirim Minimal Sekali, Pasti Sampai." },
  { "en": "Apa Itu QoS Level 2?", "id": "Kirim Tepat Sekali, Tanpa Duplikat." },
  { "en": "Apa Itu JSON Format?", "id": "JavaScript Object Notation." },
  { "en": "Apa Kegunaan Format JSON?", "id": "Pertukaran Data Teks Ringan." },
  { "en": "Apa Itu XML Format?", "id": "Extensible Markup Language." },
  { "en": "Apa Itu Base Load Pembangkit?", "id": "Pembangkit Pemikul Beban Dasar." },
  { "en": "Apa Itu Peaker Plant?", "id": "Pembangkit Pemikul Beban Puncak." },
  { "en": "Apa Jenis Pembangkit Peaker?", "id": "PLTG, Atau PLTD." },
  { "en": "Apa Itu Ramp Rate Pembangkit?", "id": "Kecepatan Naik Turun Daya Output." },
  { "en": "Apa Itu Spinning Reserve?", "id": "Cadangan Daya Pembangkit Berputar." },
  { "en": "Apa Itu Cold Reserve?", "id": "Cadangan Pembangkit Kondisi Mati." },
  { "en": "Apa Itu Heat Rate Pembangkit?", "id": "Efisiensi Konversi Bahan Bakar." },
  { "en": "Apa Satuan Heat Rate?", "id": "Btu Per Kilowatt Hour." },
  { "en": "Apa Itu Combined Cycle?", "id": "Gabungan Turbin Gas Dan Uap." },
  { "en": "Apa Kelebihan PLTGU?", "id": "Efisiensi Termal Sangat Tinggi." },
  { "en": "Apa Itu HRSG?", "id": "Heat Recovery Steam Generator." },
  { "en": "Apa Fungsi HRSG?", "id": "Memanfaatkan Gas Buang Memanaskan Air." },
  { "en": "Apa Itu Coal Pulverizer?", "id": "Penggiling Batubara Menjadi Serbuk." },
  { "en": "Apa Itu Fly Ash?", "id": "Abu Terbang Sisa Pembakaran Batubara." },
  { "en": "Apa Itu Bottom Ash?", "id": "Abu Berat Jatuh Di Tungku." },
  { "en": "Apa Itu Electrostatic Precipitator?", "id": "Penyaring Debu Asap Listrik Statis." },
  { "en": "Apa Itu Desulfurization?", "id": "Pengurangan Sulfur Emisi Gas Buang." },
  { "en": "Apa Rumus Arus Efektif Sinus?", "id": "Irms = Imax / √2." },
  { "en": "Apa Rumus Tegangan Rata-Rata Sinus?", "id": "Nol." },
  { "en": "Apa Rumus Daya Reaktif Kapasitor?", "id": "Qc = V^2 / Xc." },
  { "en": "Apa Rumus Frekuensi Gelombang Radio?", "id": "f = c / λ." },
  { "en": "Apa Nilai Impedansi Ruang Hampa?", "id": "377 Ohm." },
  { "en": "Apa Rumus Gaya Lorentz?", "id": "F = B x I x L." },
  { "en": "Apa Rumus GGL Induksi Diri?", "id": "ε = -L x (dI / dt)." },
  { "en": "Apa Itu Satuan Electron Volt?", "id": "Energi Satu Elektron Beda 1V." },
  { "en": "Apa Nilai 1 Electron Volt?", "id": "1.6 x 10^-19 Joule." },
  { "en": "Apa Itu Load Balancer Server?", "id": "Pembagi Beban Trafik Jaringan." },
  { "en": "Apa Itu Proxy Server?", "id": "Perantara Klien Dan Internet." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Jaringan Pribadi Melalui Internet." },
  { "en": "Apa Itu Firewall Jaringan?", "id": "Tembok Keamanan Paket Data." },
  { "en": "Apa Itu DDOS Attack?", "id": "Serangan Membanjiri Trafik Server." },
  { "en": "Apa Itu Phishing?", "id": "Pencurian Data Dengan Situs Palsu." },
  { "en": "Apa Itu Malware?", "id": "Perangkat Lunak Perusak Sistem." },
  { "en": "Apa Itu Ransomware?", "id": "Malware Pengunci Data Minta Tebusan." },
  { "en": "Apa Itu Enkripsi End To End?", "id": "Hanya Pengirim Penerima Bisa Baca." },
  { "en": "Apa Itu Public Key?", "id": "Kunci Enkripsi Yang Disebarluaskan." },
  { "en": "Apa Itu Private Key?", "id": "Kunci Dekripsi Yang Dirahasiakan." },
  { "en": "Apa Itu Generator Kutub Salient?", "id": "Generator Dengan Rotor Kutub Menonjol." },
  { "en": "Apa Itu Generator Kutub Silindris?", "id": "Generator Dengan Rotor Kutub Rata." },
  { "en": "Apa Aplikasi Generator Kutub Salient?", "id": "Pembangkit Listrik Tenaga Air." },
  { "en": "Apa Aplikasi Generator Kutub Silindris?", "id": "Pembangkit Listrik Tenaga Uap." },
  { "en": "Apa Kecepatan Putar Turbin Uap?", "id": "Sangat Tinggi, Hingga 3000 RPM." },
  { "en": "Apa Kecepatan Putar Turbin Air?", "id": "Rendah, Sekitar 500 RPM." },
  { "en": "Apa Fungsi Damper Winding Generator?", "id": "Meredam Osilasi Hunting Rotor." },
  { "en": "Apa Itu Hunting Pada Motor?", "id": "Osilasi Kecepatan, Sekitar Kecepatan Sinkron." },
  { "en": "Apa Penyebab Hunting Motor Sinkron?", "id": "Perubahan Beban Secara Mendadak." },
  { "en": "Apa Fungsi Sistem Eksitasi Generator?", "id": "Menyuplai Arus DC Ke Rotor." },
  { "en": "Apa Itu Brushless Excitation System?", "id": "Sistem Eksitasi Tanpa Sikat Arang." },
  { "en": "Apa Itu Pilot Exciter?", "id": "Generator Magnet Permanen Kecil." },
  { "en": "Apa Itu Indeks Polarisasi Isolasi?", "id": "Rasio Tahanan, 10 Menit 1 Menit." },
  { "en": "Apa Nilai Indeks Polarisasi Baik?", "id": "Di Atas Dua Koma Nol." },
  { "en": "Apa Itu Dielectric Absorption Ratio?", "id": "Rasio Tahanan, 60 Detik 30 Detik." },
  { "en": "Apa Fungsi Kapasitor Shunt Jaringan?", "id": "Menyuplai Daya Reaktif, Perbaiki Tegangan." },
  { "en": "Apa Fungsi Kapasitor Seri Jaringan?", "id": "Mengurangi Reaktansi Saluran Transmisi." },
  { "en": "Apa Bahaya Kapasitor Seri Transmisi?", "id": "Resonansi Sub Sinkron." },
  { "en": "Apa Kepanjangan FACTS Sistem Tenaga?", "id": "Flexible AC Transmission Systems." },
  { "en": "Apa Fungsi SVC (Static Var Compensator)?", "id": "Kompensasi Daya Reaktif Dinamis." },
  { "en": "Apa Fungsi STATCOM?", "id": "Kompensator Statis, Berbasis Konverter Tegangan." },
  { "en": "Apa Kelebihan STATCOM Dibanding SVC?", "id": "Respon Lebih Cepat, Dan Stabil." },
  { "en": "Apa Itu UPFC Sistem Tenaga?", "id": "Unified Power Flow Controller." },
  { "en": "Apa Itu HVDC LCC?", "id": "Line Commutated Converter." },
  { "en": "Apa Itu HVDC VSC?", "id": "Voltage Source Converter." },
  { "en": "Apa Kelebihan HVDC VSC?", "id": "Bisa Black Start, Kontrol Aktif." },
  { "en": "Apa Isolasi Kabel Laut HVDC?", "id": "Kertas Minyak, Atau XLPE." },
  { "en": "Apa Itu Korosi Galvanik?", "id": "Korosi Kontak Dua Logam Berbeda." },
  { "en": "Apa Itu Anoda Tumbal?", "id": "Logam Korban Pencegah Korosi." },
  { "en": "Apa Logam Anoda Tumbal Umum?", "id": "Seng, Magnesium, Atau Aluminium." },
  { "en": "Apa Itu Proteksi Katodik?", "id": "Teknik Mencegah Korosi Pipa Logam." },
  { "en": "Apa Itu Impressed Current Protection?", "id": "Proteksi Katodik, Dengan Sumber DC." },
  { "en": "Apa Itu Floating Ground?", "id": "Ground Tidak Terhubung Ke Bumi." },
  { "en": "Apa Itu Chassis Ground?", "id": "Ground Terhubung Ke Rangka Logam." },
  { "en": "Apa Itu Signal Ground?", "id": "Referensi Nol Volt, Sinyal Elektronik." },
  { "en": "Apa Bahaya Ground Loop?", "id": "Arus Putar Menimbulkan Noise Sinyal." },
  { "en": "Apa Solusi Masalah Ground Loop?", "id": "Satu Titik Sambungan Ground." },
  { "en": "Apa Itu Star Grounding?", "id": "Semua Ground Menuju Satu Titik." },
  { "en": "Apa Itu Daisy Chain Grounding?", "id": "Ground Seri Berurutan Antar Alat." },
  { "en": "Apa Kelemahan Daisy Chain Grounding?", "id": "Impedansi Ground Bertingkat." },
  { "en": "Apa Rumus Desibel Tegangan?", "id": "dB = 20 x Log(Vout / Vin)." },
  { "en": "Apa Rumus Desibel Daya?", "id": "dB = 10 x Log(Pout / Pin)." },
  { "en": "Apa Itu Titik 3dB?", "id": "Titik Daya Turun Setengah." },
  { "en": "Apa Itu Half Power Bandwidth?", "id": "Lebar Pita Frekuensi Daya Setengah." },
  { "en": "Apa Itu Unity Gain Bandwidth?", "id": "Frekuensi Saat Penguatan Bernilai Satu." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Perkalian Gain Dan Bandwidth Konstan." },
  { "en": "Apa Itu Thermal Noise?", "id": "Noise Akibat Gerakan Acak Elektron." },
  { "en": "Apa Itu Shot Noise?", "id": "Noise Akibat Aliran Partikel Diskrit." },
  { "en": "Apa Itu Flicker Noise 1/f?", "id": "Noise Dominan Pada Frekuensi Rendah." },
  { "en": "Apa Itu Burst Noise?", "id": "Noise Popcorn Yang Meletup." },
  { "en": "Apa Itu Avalanche Noise?", "id": "Noise Akibat Breakdown Dioda Zener." },
  { "en": "Apa Fungsi Shielding Kabel?", "id": "Melindungi Sinyal Dari Interferensi Luar." },
  { "en": "Apa Fungsi Braided Shield?", "id": "Pelindung Anyaman Kawat Tembaga." },
  { "en": "Apa Fungsi Foil Shield?", "id": "Pelindung Lembaran Aluminium Tipis." },
  { "en": "Apa Itu Drain Wire Kabel?", "id": "Kawat Ground Pada Kabel Shield." },
  { "en": "Apa Kategori Kabel LAN Gigabit?", "id": "Cat5e, Atau Cat6." },
  { "en": "Apa Kecepatan Kabel Cat6?", "id": "Hingga 10 Gigabit Per Detik." },
  { "en": "Apa Kecepatan Kabel Cat5e?", "id": "Hingga 1 Gigabit Per Detik." },
  { "en": "Apa Itu Kabel Crossover?", "id": "Kabel LAN Silang Antar Komputer." },
  { "en": "Apa Itu Kabel Straight Through?", "id": "Kabel LAN Lurus Ke Switch." },
  { "en": "Apa Urutan Warna T568B?", "id": "Putih Oranye, Oranye, Putih Hijau." },
  { "en": "Apa Urutan Warna T568A?", "id": "Putih Hijau, Hijau, Putih Oranye." },
  { "en": "Apa Fungsi Fitur Auto MDI-X?", "id": "Deteksi Otomatis Tipe Kabel LAN." },
  { "en": "Apa Itu Fiber Graded Index?", "id": "Indeks Bias Inti Berubah Bertahap." },
  { "en": "Apa Itu Fiber Step Index?", "id": "Indeks Bias Inti Berubah Tajam." },
  { "en": "Apa Itu Modal Dispersion Fiber?", "id": "Penyebaran Pulsa Akibat Mode Berbeda." },
  { "en": "Apa Itu Chromatic Dispersion Fiber?", "id": "Penyebaran Pulsa, Akibat Panjang Gelombang." },
  { "en": "Apa Itu WDM Telekomunikasi?", "id": "Wavelength Division Multiplexing." },
  { "en": "Apa Itu DWDM Telekomunikasi?", "id": "Dense Wavelength Division Multiplexing." },
  { "en": "Apa Itu Biasing Transistor?", "id": "Pemberian Tegangan Kerja DC Awal." },
  { "en": "Apa Itu Fixed Bias Transistor?", "id": "Bias Basis, Menggunakan Resistor Tetap." },
  { "en": "Apa Itu Voltage Divider Bias?", "id": "Bias Pembagi Tegangan Paling Stabil." },
  { "en": "Apa Itu Titik Q Transistor?", "id": "Titik Kerja Operasi Transistor." },
  { "en": "Apa Itu Garis Beban DC?", "id": "Garis Karakteristik Output, Kondisi Statis." },
  { "en": "Apa Itu Garis Beban AC?", "id": "Garis Karakteristik Sinyal, Kondisi Dinamis." },
  { "en": "Apa Itu Thermal Resistance Heatsink?", "id": "Hambatan Aliran Panas Pendingin." },
  { "en": "Apa Satuan Thermal Resistance?", "id": "Derajat Celcius Per Watt." },
  { "en": "Apa Rumus Suhu Junction Transistor?", "id": "Tj = Ta + (Pd x Rth)." },
  { "en": "Apa Itu Derating Factor Komponen?", "id": "Penurunan Kemampuan, Akibat Kenaikan Suhu." },
  { "en": "Apa Itu Safe Operating Area?", "id": "Area Aman, Tegangan Dan Arus." },
  { "en": "Apa Itu Second Breakdown Transistor?", "id": "Kerusakan Lokal, Akibat Hotspot Termal." },
  { "en": "Apa Fungsi Kapasitor Snubber?", "id": "Meredam Spike Tegangan Saat Switching." },
  { "en": "Apa Itu Reverse Recovery Charge?", "id": "Muatan Tersimpan Saat Dioda Mati." },
  { "en": "Apa Itu Soft Recovery Diode?", "id": "Dioda Dengan Pemulihan Arus Halus." },
  { "en": "Apa Itu Hard Recovery Diode?", "id": "Dioda Dengan Pemulihan Tiba-Tiba." },
  { "en": "Apa Rumus Efisiensi Rectifier?", "id": "Eta = Pdc / Pac." },
  { "en": "Apa Nilai Efisiensi Rectifier Setengah?", "id": "Maksimal 40.6 Persen." },
  { "en": "Apa Nilai Efisiensi Rectifier Penuh?", "id": "Maksimal 81.2 Persen." },
  { "en": "Apa Itu Ripple Factor?", "id": "Rasio Tegangan AC Dan DC." },
  { "en": "Apa Nilai Ripple Factor Setengah?", "id": "1.21." },
  { "en": "Apa Nilai Ripple Factor Penuh?", "id": "0.48." },
  { "en": "Apa Itu Form Factor Gelombang?", "id": "Rasio Vrms Dan Vavg." },
  { "en": "Apa Nilai Form Factor Sinus?", "id": "1.11." },
  { "en": "Apa Itu Crest Factor Gelombang?", "id": "Rasio Vpuncak Dan Vrms." },
  { "en": "Apa Nilai Crest Factor Sinus?", "id": "1.414." },
  { "en": "Apa Itu Permeabilitas Vakum?", "id": "Konstanta Magnetik Ruang Hampa." },
  { "en": "Apa Itu Permitivitas Vakum?", "id": "Konstanta Listrik Ruang Hampa." },
  { "en": "Apa Satuan Fluks Bercahaya?", "id": "Lumen." },
  { "en": "Apa Satuan Iluminasi Cahaya?", "id": "Lux." },
  { "en": "Apa Satuan Luminansi Cahaya?", "id": "Candela Per Meter Persegi." },
  { "en": "Apa Kepanjangan SAIDI Keandalan Listrik?", "id": "System Average, Interruption Duration Index." },
  { "en": "Apa Definisi Indeks SAIDI?", "id": "Rata-Rata Durasi Padam, Per Pelanggan." },
  { "en": "Apa Kepanjangan SAIFI Keandalan Listrik?", "id": "System Average, Interruption Frequency Index." },
  { "en": "Apa Definisi Indeks SAIFI?", "id": "Rata-Rata Frekuensi Padam, Per Pelanggan." },
  { "en": "Apa Kepanjangan CAIDI?", "id": "Customer Average, Interruption Duration Index." },
  { "en": "Apa Itu Smart Grid?", "id": "Jaringan Listrik Cerdas, Terintegrasi." },
  { "en": "Apa Itu Microgrid?", "id": "Jaringan Listrik Skala Kecil, Mandiri." },
  { "en": "Apa Itu Distributed Generation?", "id": "Pembangkit Listrik Tersebar, Dekat Beban." },
  { "en": "Apa Itu Virtual Power Plant?", "id": "Gabungan Pembangkit Kecil, Terkoordinasi." },
  { "en": "Apa Itu Demand Response?", "id": "Pengaturan Beban, Sesuai Kondisi Grid." },
  { "en": "Apa Itu Peak Shaving?", "id": "Memangkas Beban Puncak Listrik." },
  { "en": "Apa Itu Load Shifting?", "id": "Menggeser Pemakaian Listrik, Ke Waktu Lain." },
  { "en": "Apa Itu Net Metering PLTS?", "id": "Ekspor Impor Listrik, Pelanggan PLN." },
  { "en": "Apa Itu Smart Meter?", "id": "Meteran Listrik Digital, Komunikasi Dua Arah." },
  { "en": "Apa Kepanjangan AMI Metering?", "id": "Advanced Metering Infrastructure." },
  { "en": "Apa Kepanjangan PMU (Phasor Measurement Unit)?", "id": "Phasor Measurement Unit." },
  { "en": "Apa Fungsi PMU Pada Grid?", "id": "Mengukur Fasor Tegangan Arus, Sinkron." },
  { "en": "Apa Itu Wide Area Monitoring System?", "id": "Sistem Pemantauan Grid, Area Luas." },
  { "en": "Apa Protokol IEC 61850 GOOSE?", "id": "Komunikasi Cepat, Antar Relay Proteksi." },
  { "en": "Apa Protokol DNP3?", "id": "Distributed Network Protocol, Standar Amerika." },
  { "en": "Apa Protokol Modbus TCP?", "id": "Komunikasi Modbus, Melalui Jaringan Ethernet." },
  { "en": "Apa Kepanjangan OPC UA?", "id": "Open Platform Communications, Unified Architecture." },
  { "en": "Apa Kelebihan OPC UA?", "id": "Keamanan Tinggi, Dan Lintas Platform." },
  { "en": "Apa Itu Microstrip Line PCB?", "id": "Jalur Transmisi, Di Permukaan PCB." },
  { "en": "Apa Itu Stripline PCB?", "id": "Jalur Transmisi, Di Dalam Lapisan PCB." },
  { "en": "Apa Keuntungan Stripline Dibanding Microstrip?", "id": "Radiasi Sinyal Keluar, Lebih Rendah." },
  { "en": "Apa Fungsi Via Stitching?", "id": "Menghubungkan Ground Plane, Berbagai Sisi." },
  { "en": "Apa Fungsi Via Fencing?", "id": "Memagar Jalur RF, Dengan Via Ground." },
  { "en": "Apa Itu Impedansi Diferensial USB?", "id": "90 Ohm." },
  { "en": "Apa Itu Impedansi Diferensial Ethernet?", "id": "100 Ohm." },
  { "en": "Apa Itu Impedansi Single Ended Memori?", "id": "Biasanya 50 Ohm." },
  { "en": "Apa Itu Length Matching PCB?", "id": "Menyamakan Panjang Jalur, Sinyal Paralel." },
  { "en": "Apa Itu Meandering Trace?", "id": "Jalur Berkelok, Untuk Menambah Panjang." },
  { "en": "Apa Itu Solder Paste?", "id": "Campuran Serbuk Timah, Dan Flux." },
  { "en": "Apa Fungsi Stencil Printer?", "id": "Mencetak Solder Paste, Pada PCB." },
  { "en": "Apa Fungsi Reflow Oven?", "id": "Memanaskan PCB, Hingga Timah Cair." },
  { "en": "Apa Itu Profil Suhu Reflow?", "id": "Grafik Suhu, Terhadap Waktu Pemanasan." },
  { "en": "Apa Fase Soak Pada Reflow?", "id": "Pemanasan Merata, Aktivasi Flux." },
  { "en": "Apa Itu Tombstoning Komponen?", "id": "Komponen Berdiri Tegak, Saat Disolder." },
  { "en": "Apa Penyebab Tombstoning?", "id": "Pemanasan Pad Tidak Seimbang." },
  { "en": "Apa Itu Cold Solder Joint?", "id": "Sambungan Solder Buruk, Dan Rapuh." },
  { "en": "Apa Itu Solder Bridge?", "id": "Hubung Singkat, Antar Kaki Komponen." },
  { "en": "Apa Itu 5G NR (New Radio)?", "id": "Standar Radio Baru, Jaringan 5G." },
  { "en": "Apa Itu Frekuensi Sub-6 GHz?", "id": "Frekuensi 5G, Di Bawah 6GHz." },
  { "en": "Apa Itu mmWave 5G?", "id": "Gelombang Millimeter, Frekuensi Sangat Tinggi." },
  { "en": "Apa Kelebihan mmWave?", "id": "Bandwidth Sangat Besar, Kecepatan Tinggi." },
  { "en": "Apa Kelemahan mmWave?", "id": "Jangkauan Pendek, Tidak Tembus Dinding." },
  { "en": "Apa Itu Network Slicing 5G?", "id": "Memotong Jaringan Virtual, Sesuai Layanan." },
  { "en": "Apa Itu URLLC 5G?", "id": "Ultra Reliable, Low Latency Communications." },
  { "en": "Apa Itu mMTC 5G?", "id": "Massive Machine Type Communications." },
  { "en": "Apa Itu eMBB 5G?", "id": "Enhanced Mobile Broadband." },
  { "en": "Apa Itu Carrier Aggregation?", "id": "Menggabungkan Beberapa Frekuensi Pembawa." },
  { "en": "Apa Keuntungan Carrier Aggregation?", "id": "Meningkatkan Kecepatan Data Total." },
  { "en": "Apa Itu Beamforming 5G?", "id": "Mengarahkan Sinyal, Ke Pengguna Spesifik." },
  { "en": "Apa Itu Massive MIMO?", "id": "Menggunakan Ratusan Antena, Base Station." },
  { "en": "Apa Itu NB-IoT (Narrowband IoT)?", "id": "Jaringan Seluler, Hemat Daya IoT." },
  { "en": "Apa Itu LTE Cat-M1?", "id": "Standar LTE, Untuk Mesin Ke Mesin." },
  { "en": "Apa Itu LoRaWAN Class A?", "id": "Paling Hemat Daya, Komunikasi Dua Arah." },
  { "en": "Apa Itu LoRaWAN Class C?", "id": "Selalu Aktif Mendengar Pesan." },
  { "en": "Apa Itu Edge Computing?", "id": "Pemrosesan Data, Di Tepi Jaringan." },
  { "en": "Apa Keuntungan Edge Computing?", "id": "Mengurangi Latency, Dan Beban Cloud." },
  { "en": "Apa Itu Digital Twin?", "id": "Replika Digital Dari Sistem Fisik." },
  { "en": "Apa Manfaat Digital Twin Elektro?", "id": "Simulasi, Dan Prediksi Pemeliharaan." },
  { "en": "Apa Itu Predictive Maintenance?", "id": "Pemeliharaan, Berdasarkan Prediksi Kondisi." },
  { "en": "Apa Itu AI Di Sistem Tenaga?", "id": "Kecerdasan Buatan, Untuk Optimasi Grid." },
  { "en": "Apa Itu Machine Learning?", "id": "Mesin Belajar Pola, Dari Data." },
  { "en": "Apa Aplikasi ML Di Proteksi?", "id": "Deteksi Gangguan Impedansi Tinggi." },
  { "en": "Apa Rumus Impedansi Karakteristik Saluran?", "id": "Z0 = √(L / C)." },
  { "en": "Apa Rumus Kecepatan Gelombang Kabel?", "id": "v = 1 / √(L x C)." },
  { "en": "Apa Itu Velocity Factor?", "id": "Rasio Kecepatan, Sinyal Dan Cahaya." },
  { "en": "Apa Nilai Velocity Factor Coaxial RG58?", "id": "0.66, Atau 66 Persen." },
  { "en": "Apa Itu Standing Wave?", "id": "Gelombang Diam, Akibat Interferensi Pantulan." },
  { "en": "Apa Itu Titik Perut Gelombang?", "id": "Amplitudo Maksimum, Gelombang Stasioner." },
  { "en": "Apa Itu Titik Simpul Gelombang?", "id": "Amplitudo Minimum, Gelombang Stasioner." },
  { "en": "Apa Jarak Antar Simpul Gelombang?", "id": "Setengah Panjang Gelombang." },
  { "en": "Apa Itu Stub Matching?", "id": "Potongan Kabel, Penyesuai Impedansi." },
  { "en": "Apa Fungsi Short Circuit Stub?", "id": "Menghasilkan Reaktansi Induktif, Atau Kapasitif." },
  { "en": "Apa Itu Antenna Array?", "id": "Gabungan Beberapa Antena, Bekerja Sama." },
  { "en": "Apa Itu Main Lobe Antena?", "id": "Arah Pancaran Utama Antena." },
  { "en": "Apa Itu Side Lobe Antena?", "id": "Pancaran Samping, Yang Tidak Diinginkan." },
  { "en": "Apa Itu Back Lobe Antena?", "id": "Pancaran Belakang, Yang Tidak Diinginkan." },
  { "en": "Apa Itu Topologi Jaringan Star?", "id": "Semua Perangkat Terhubung Ke Pusat." },
  { "en": "Apa Itu Topologi Jaringan Mesh?", "id": "Semua Perangkat Saling Terhubung." },
  { "en": "Apa Kelebihan Topologi Mesh?", "id": "Keandalan Tinggi, Jalur Alternatif." },
  { "en": "Apa Itu Topologi Jaringan Ring?", "id": "Perangkat Terhubung Membentuk Lingkaran." },
  { "en": "Apa Itu Topologi Jaringan Bus?", "id": "Semua Perangkat, Pada Satu Kabel." },
  { "en": "Apa Tegangan Logika RS232?", "id": "Positif Dan Negatif, 3 Hingga 15V." },
  { "en": "Apa Tegangan Logika RS485?", "id": "Beda Potensial, 1.5 Hingga 6 Volt." },
  { "en": "Apa Hambatan Terminasi CAN Bus?", "id": "120 Ohm, Pada Kedua Ujung." },
  { "en": "Apa Hambatan Terminasi RS485?", "id": "120 Ohm, Pada Awal Dan Akhir." },
  { "en": "Apa Itu Half Duplex?", "id": "Komunikasi Dua Arah, Bergantian." },
  { "en": "Apa Itu Simplex?", "id": "Komunikasi Satu Arah Saja." },
  { "en": "Apa Itu Full Duplex?", "id": "Komunikasi Dua Arah, Bersamaan." },
  { "en": "Apa Itu Baud Rate?", "id": "Kecepatan Perubahan, Simbol Sinyal." },
  { "en": "Apa Itu Parity Bit Ganjil?", "id": "Jumlah Bit Satu, Harus Ganjil." },
  { "en": "Apa Itu Parity Bit Genap?", "id": "Jumlah Bit Satu, Harus Genap." },
  { "en": "Apa Itu Stop Bit UART?", "id": "Bit Penanda, Akhir Paket Data." },
  { "en": "Apa Itu Start Bit UART?", "id": "Bit Penanda, Awal Paket Data." },
  { "en": "Apa Itu Sistem Per Unit?", "id": "Normalisasi Besaran, Terhadap Nilai Basis." },
  { "en": "Apa Rumus Impedansi Basis Zb?", "id": "Zb = Vb^2 / Sb." },
  { "en": "Apa Itu Parameter ABCD Saluran?", "id": "Matriks Transmisi, Saluran Dua Port." },
  { "en": "Apa Syarat Resiprositas Parameter ABCD?", "id": "A x D - B x C = 1." },
  { "en": "Apa Syarat Simetri Parameter ABCD?", "id": "A = D." },
  { "en": "Apa Itu Konduktor Berkas (Bundle)?", "id": "Beberapa Konduktor Per Fasa." },
  { "en": "Apa Keuntungan Konduktor Berkas?", "id": "Mengurangi Corona, Dan Induktansi." },
  { "en": "Apa Itu Transposisi Saluran Transmisi?", "id": "Pertukaran Posisi Fasa Kabel." },
  { "en": "Apa Tujuan Transposisi Saluran?", "id": "Menyeimbangkan Impedansi Antar Fasa." },
  { "en": "Apa Itu Fill Factor Sel Surya?", "id": "Ukuran Kualitas Kotak Kurva I-V." },
  { "en": "Apa Rumus Fill Factor (FF)?", "id": "FF = Pmax / (Voc x Isc)." },
  { "en": "Apa Itu Tip Speed Ratio Turbin?", "id": "Rasio Kecepatan Ujung, Dan Angin." },
  { "en": "Apa Algoritma MPPT Paling Umum?", "id": "Perturb And Observe." },
  { "en": "Apa Itu Deteksi Islanding Inverter?", "id": "Deteksi Hilangnya, Jaringan Listrik Utama." },
  { "en": "Apa Itu Bahasa Verilog?", "id": "Bahasa Deskripsi Perangkat Keras." },
  { "en": "Apa Itu FPGA Lookup Table (LUT)?", "id": "Blok Logika Dasar FPGA." },
  { "en": "Apa Perbedaan CPLD Dan FPGA?", "id": "CPLD Non-Volatile, FPGA Volatile." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Debugging, Dan Testing Chip." },
  { "en": "Apa Fungsi Pin TCK JTAG?", "id": "Sinyal Clock Test." },
  { "en": "Apa Fungsi Pin TMS JTAG?", "id": "Test Mode Select." },
  { "en": "Apa Fungsi Pin TDI JTAG?", "id": "Test Data Input." },
  { "en": "Apa Fungsi Pin TDO JTAG?", "id": "Test Data Output." },
  { "en": "Apa Itu Boundary Scan JTAG?", "id": "Tes Interkoneksi Pin, Tanpa Probe." },
  { "en": "Apa Fungsi IC 7400?", "id": "Empat Gerbang NAND, 2 Input." },
  { "en": "Apa Fungsi IC 7404?", "id": "Enam Inverter, Atau NOT Gate." },
  { "en": "Apa Fungsi IC 7408?", "id": "Empat Gerbang AND, 2 Input." },
  { "en": "Apa Fungsi IC 7432?", "id": "Empat Gerbang OR, 2 Input." },
  { "en": "Apa Fungsi IC 7474?", "id": "Dua D Flip-Flop." },
  { "en": "Apa Rumus Waktu Monostabil 555?", "id": "t = 1.1 x R x C." },
  { "en": "Apa Rumus Frekuensi Astabil 555?", "id": "f = 1.44 / ((R1 + 2R2)C)." },
  { "en": "Apa Satuan Slew Rate Op-Amp?", "id": "Volt Per Mikro Detik." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Bandwidth Saat Penguatan Satu." },
  { "en": "Apa Itu Instrumentation Amplifier?", "id": "Penguat Diferensial Tiga Op-Amp." },
  { "en": "Apa Fungsi Lock-In Amplifier?", "id": "Mengukur Sinyal, Dalam Noise Tinggi." },
  { "en": "Apa Itu Sensitivitas Jembatan Wheatstone?", "id": "Perubahan Output, Per Perubahan Resistansi." },
  { "en": "Apa Itu Pengukuran Kelvin 4 Kabel?", "id": "Eliminasi Hambatan Kabel Probe." },
  { "en": "Apa Rumus Tegangan Hall?", "id": "Vh = (I x B) / (n x q x d)." },
  { "en": "Apa Itu Efek Piezoelektrik?", "id": "Tekanan Mekanik, Menjadi Tegangan Listrik." },
  { "en": "Apa Itu Efek Piezoelektrik Terbalik?", "id": "Tegangan Listrik, Menjadi Gerakan Mekanik." },
  { "en": "Apa Fungsi SAW Filter RF?", "id": "Filter Gelombang, Akustik Permukaan." },
  { "en": "Apa Prinsip MEMS Gyroscope?", "id": "Efek Coriolis." },
  { "en": "Apa Itu Antena Horn?", "id": "Antena Corong Waveguide." },
  { "en": "Apa Karakteristik Antena Parabola?", "id": "Gain Sangat Tinggi, Arah Fokus." },
  { "en": "Apa Itu Antena Microstrip Patch?", "id": "Antena Cetak Pada PCB." },
  { "en": "Apa Mode Radiasi Antena Helical?", "id": "Mode Normal, Dan Mode Axial." },
  { "en": "Apa Pusat Smith Chart?", "id": "Impedansi Termatch, 50 Ohm." },
  { "en": "Apa Lingkaran Luar Smith Chart?", "id": "Reaktansi Murni, Tanpa Resistansi." },
  { "en": "Apa Garis Datar Smith Chart?", "id": "Resistansi Murni." },
  { "en": "Apa Itu Rat Race Coupler?", "id": "Hybrid Coupler, 180 Derajat." },
  { "en": "Apa Itu Wilkinson Power Divider?", "id": "Pembagi Daya, Dengan Isolasi Port." },
  { "en": "Apa Fungsi Circulator RF?", "id": "Mengarahkan Sinyal, Ke Port Berikutnya." },
  { "en": "Apa Fungsi Isolator RF?", "id": "Meneruskan Sinyal Satu Arah Saja." },
  { "en": "Apa Itu Inverter VSI?", "id": "Voltage Source Inverter." },
  { "en": "Apa Itu Inverter CSI?", "id": "Current Source Inverter." },
  { "en": "Apa Itu Teknik SPWM?", "id": "Sinusoidal, Pulse Width Modulation." },
  { "en": "Apa Rumus Total Harmonic Distortion?", "id": "THD = √(ΣVn^2) / V1." },
  { "en": "Apa Frekuensi Ripple Rectifier 6 Pulse?", "id": "6 x Frekuensi Input." },
  { "en": "Apa Frekuensi Ripple Rectifier 12 Pulse?", "id": "12 x Frekuensi Input." },
  { "en": "Apa Kelebihan Rectifier 12 Pulse?", "id": "Harmonisa Lebih Rendah." },
  { "en": "Apa Itu Pengereman Rheostatic?", "id": "Energi Dibuang, Ke Resistor." },
  { "en": "Apa Itu Pengereman Plugging?", "id": "Membalik Urutan Fasa Motor." },
  { "en": "Apa Itu Cogging Torque Motor?", "id": "Torsi Denyut, Interaksi Slot Magnet." },
  { "en": "Cara Mengurangi Cogging Torque?", "id": "Memiringkan Slot Stator, Atau Rotor." },
  { "en": "Apa Itu Crawling Motor Induksi?", "id": "Putaran Lambat, Akibat Harmonisa Ganjil." },
  { "en": "Apa Itu Single Phasing Motor?", "id": "Hilangnya Satu Fasa Suplai." },
  { "en": "Apa Alat Proteksi Single Phasing?", "id": "Phase Failure Relay." },
  { "en": "Apa Itu Buchholz Relay Trip?", "id": "Pemutusan Akibat, Akumulasi Gas Cepat." },
  { "en": "Apa Isi Tabung Breather Trafo?", "id": "Silica Gel." },
  { "en": "Apa Itu Diverter Switch OLTC?", "id": "Sakelar Pemindah, Beban Tap Changer." },
  { "en": "Apa Bedanya OLTC Dan NLTC?", "id": "OLTC Beroperasi, Saat Berbeban." },
  { "en": "Apa Itu Vector Group Dyn11?", "id": "Sekunder Delta, Leading 30 Derajat." },
  { "en": "Apa Itu Vector Group YNd1?", "id": "Sekunder Netral, Lagging 30 Derajat." },
  { "en": "Apa Itu Fiducial Mark PCB?", "id": "Titik Referensi, Mesin Pick Place." },
  { "en": "Apa Itu Mouse Bites PCB?", "id": "Lubang Perforasi, Pemisah Panel." },
  { "en": "Apa Itu V-Score PCB?", "id": "Potongan V, Untuk Pemisah Board." },
  { "en": "Apa Itu Castellated Hole PCB?", "id": "Lubang Plating, Setengah Di Tepi." },
  { "en": "Apa Itu Rigid-Flex PCB?", "id": "Gabungan PCB Kaku, Dan Fleksibel." },
  { "en": "Apa Itu Finishing OSP PCB?", "id": "Organic Solderability Preservative." },
  { "en": "Apa Itu Finishing Immersion Silver?", "id": "Plating Perak, Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Finishing Hard Gold?", "id": "Emas Keras, Untuk Konektor Gesek." },
  { "en": "Apa Itu Finishing Soft Gold?", "id": "Emas Lunak, Untuk Wire Bonding." },
  { "en": "Apa Itu Flying Probe Test?", "id": "Pengujian PCB, Tanpa Fixture Paku." },
  { "en": "Apa Itu Bed Of Nails Test?", "id": "Pengujian PCB, Dengan Fixture Paku." },
  { "en": "Apa Itu AOI (Automated Optical Inspection)?", "id": "Inspeksi Visual PCB, Dengan Kamera." },
  { "en": "Apa Itu X-Ray Inspection PCB?", "id": "Inspeksi Solder BGA, Dengan Sinar-X." },
  { "en": "Apa Fungsi Thermal Relief Pad?", "id": "Memudahkan Solder Pad, Ground Besar." },
  { "en": "Apa Itu Annular Ring PCB?", "id": "Cincin Tembaga, Sekeliling Lubang Via." },
  { "en": "Apa Itu DRC (Design Rule Check)?", "id": "Pemeriksaan Aturan Desain PCB." },
  { "en": "Apa Itu ERC (Electrical Rule Check)?", "id": "Pemeriksaan Logika Skematik." },
  { "en": "Apa Itu Netlist?", "id": "Daftar Koneksi Antar Komponen." },
  { "en": "Apa Itu Footprint Komponen?", "id": "Pola Landasan Solder Di PCB." },
  { "en": "Apa Itu 3D Package Model?", "id": "Model Fisik Komponen, Untuk Visualisasi." },
  { "en": "Apa Itu Step Up Chopper?", "id": "Boost Converter." },
  { "en": "Apa Itu Step Down Chopper?", "id": "Buck Converter." },
  { "en": "Apa Itu Four Quadrant Chopper?", "id": "Bisa Mengalirkan Arus, Dua Arah." },
  { "en": "Apa Itu Cycloconverter?", "id": "Pengubah Frekuensi AC Langsung." },
  { "en": "Apa Itu AC Voltage Controller?", "id": "Pengatur Tegangan AC, Tanpa Frekuensi." },
  { "en": "Apa Komponen Utama AC Regulator?", "id": "Dua Thyristor Anti Paralel." },
  { "en": "Apa Itu Integral Cycle Control?", "id": "Mengatur Jumlah Gelombang, Penuh Lewat." },
  { "en": "Apa Itu Phase Angle Control?", "id": "Mengatur Sudut Penyalaan Thyristor." },
  { "en": "Apa Itu Slack Bus?", "id": "Bus Referensi, Tegangan Dan Sudut." },
  { "en": "Apa Parameter Yang Diketahui Di Slack Bus?", "id": "Tegangan, Dan Sudut Fasa." },
  { "en": "Apa Itu PV Bus Pada Aliran Daya?", "id": "Bus Generator Pengendali Tegangan." },
  { "en": "Apa Parameter Yang Diketahui Di PV Bus?", "id": "Daya Aktif, Dan Magnitudo Tegangan." },
  { "en": "Apa Itu PQ Bus Pada Aliran Daya?", "id": "Bus Beban." },
  { "en": "Apa Parameter Yang Diketahui Di PQ Bus?", "id": "Daya Aktif, Dan Daya Reaktif." },
  { "en": "Apa Itu Matriks Jacobian?", "id": "Matriks Turunan Parsial, Aliran Daya." },
  { "en": "Apa Metode Gauss Seidel?", "id": "Metode Iterasi, Penyelesaian Aliran Daya." },
  { "en": "Apa Metode Newton Raphson?", "id": "Metode Iterasi Konvergensi, Kuadratik Cepat." },
  { "en": "Apa Kelebihan Metode Newton Raphson?", "id": "Jumlah Iterasi Sedikit, Dan Cepat." },
  { "en": "Apa Kelebihan Metode Fast Decoupled?", "id": "Matriks Jacobian Konstan, Mempercepat Hitungan." },
  { "en": "Apa Itu Unit Commitment?", "id": "Jadwal Operasi Pembangkit, Paling Ekonomis." },
  { "en": "Apa Itu Economic Dispatch?", "id": "Pembagian Beban Pembangkit, Biaya Minimum." },
  { "en": "Apa Itu Penalty Factor Pembangkit?", "id": "Faktor Koreksi, Rugi Saluran Transmisi." },
  { "en": "Apa Itu Incremental Fuel Cost?", "id": "Biaya Bahan Bakar, Per Tambahan Daya." },
  { "en": "Apa Itu Sphere Gap?", "id": "Sela Bola, Standar Kalibrasi Tegangan." },
  { "en": "Apa Penggunaan Sphere Gap?", "id": "Mengukur Tegangan Puncak, Impuls Tinggi." },
  { "en": "Apa Itu Rogowski Coil?", "id": "Sensor Arus AC, Tanpa Inti Besi." },
  { "en": "Apa Kelebihan Rogowski Coil?", "id": "Linearitas Tinggi, Tidak Ada Saturasi." },
  { "en": "Apa Output Rogowski Coil?", "id": "Tegangan Proporsional, Turunan Arus." },
  { "en": "Apa Itu Impulse Ratio?", "id": "Rasio Tegangan Tembus, Impuls Statis." },
  { "en": "Apa Itu Basic Impulse Insulation Level?", "id": "Ketahanan Isolasi, Terhadap Tegangan Petir." },
  { "en": "Apa Itu Wet Flashover Voltage?", "id": "Tegangan Loncat Api, Saat Basah." },
  { "en": "Apa Itu Dry Flashover Voltage?", "id": "Tegangan Loncat Api, Saat Kering." },
  { "en": "Apa Itu CW Radar?", "id": "Continuous Wave Radar." },
  { "en": "Apa Prinsip CW Radar?", "id": "Memanfaatkan Efek Doppler, Deteksi Kecepatan." },
  { "en": "Apa Itu Efek Doppler Radar?", "id": "Pergeseran Frekuensi, Akibat Gerakan Target." },
  { "en": "Apa Itu Pulse Radar?", "id": "Radar Mengirim Pulsa Pendek, Berulang." },
  { "en": "Apa Itu Blind Speed Radar?", "id": "Kecepatan Target, Tidak Terdeteksi MTI." },
  { "en": "Apa Kepanjangan MTI Radar?", "id": "Moving Target Indicator." },
  { "en": "Apa Itu Radar Cross Section?", "id": "Ukuran Pantulan Radar, Suatu Objek." },
  { "en": "Apa Itu Phased Array Radar?", "id": "Radar Tanpa Antena, Berputar Mekanik." },
  { "en": "Apa Itu Synthetic Aperture Radar (SAR)?", "id": "Radar Pencitraan, Resolusi Tinggi." },
  { "en": "Apa Itu Varicap Diode?", "id": "Dioda Kapasitor Variabel, Tegangan Mundur." },
  { "en": "Apa Fungsi Varicap Diode?", "id": "Penala Frekuensi, Pada Rangkaian LC." },
  { "en": "Apa Itu Tunnel Diode?", "id": "Dioda Efek Terowongan Kuantum." },
  { "en": "Apa Karakteristik Tunnel Diode?", "id": "Resistansi Negatif, Kecepatan Tinggi." },
  { "en": "Apa Itu PIN Diode?", "id": "Dioda Dengan Lapisan, Intrinsic Lebar." },
  { "en": "Apa Fungsi PIN Diode?", "id": "Sakelar RF, Dan Attenuator Variabel." },
  { "en": "Apa Itu Step Recovery Diode?", "id": "Dioda Pembangkit Harmonisa, Orde Tinggi." },
  { "en": "Apa Itu Backward Diode?", "id": "Tunnel Diode, Tanpa Resistansi Negatif." },
  { "en": "Apa Material Termokopel Tipe K?", "id": "Chromel, Dan Alumel." },
  { "en": "Apa Warna Kabel Termokopel Tipe K?", "id": "Kuning, Dan Merah." },
  { "en": "Apa Material Termokopel Tipe J?", "id": "Iron, Dan Constantan." },
  { "en": "Apa Warna Kabel Termokopel Tipe J?", "id": "Putih, Dan Merah." },
  { "en": "Apa Material Termokopel Tipe T?", "id": "Copper, Dan Constantan." },
  { "en": "Apa Warna Kabel Termokopel Tipe T?", "id": "Biru, Dan Merah." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Kompensasi Suhu Sambungan, Referensi Termokopel." },
  { "en": "Apa Itu Thermopile Sensor?", "id": "Gabungan Seri, Banyak Termokopel." },
  { "en": "Apa Aplikasi Thermopile?", "id": "Deteksi Suhu Tanpa Kontak." },
  { "en": "Apa Itu Bolometer?", "id": "Sensor Daya, Radiasi Elektromagnetik." },
  { "en": "Apa Itu Pilot Wire Protection?", "id": "Proteksi Diferensial, Kabel Komunikasi." },
  { "en": "Apa Itu PLC Power Line Carrier?", "id": "Komunikasi Data, Lewat Kabel Listrik." },
  { "en": "Apa Fungsi Wave Trap?", "id": "Memblokir Sinyal Komunikasi, Frekuensi Tinggi." },
  { "en": "Apa Posisi Wave Trap?", "id": "Seri, Dengan Saluran Transmisi." },
  { "en": "Apa Fungsi Coupling Capacitor CVT?", "id": "Memasukkan Sinyal Komunikasi, Ke Jaringan." },
  { "en": "Apa Itu Distance Protection Zone 1?", "id": "Melindungi 80 Persen Saluran." },
  { "en": "Apa Waktu Operasi Zone 1?", "id": "Instan Tanpa Tunda Waktu." },
  { "en": "Apa Itu Distance Protection Zone 2?", "id": "Melindungi 120 Persen Saluran." },
  { "en": "Apa Waktu Operasi Zone 2?", "id": "Tunda Waktu, Sekitar 0.4 Detik." },
  { "en": "Apa Itu Auto Reclose Dead Time?", "id": "Waktu Tunggu, Sebelum Menutup Kembali." },
  { "en": "Apa Itu Lockout Tagout (LOTO)?", "id": "Prosedur Isolasi, Energi Berbahaya." },
  { "en": "Apa Itu Arc Blast?", "id": "Gelombang Tekanan Ledakan, Busur Listrik." },
  { "en": "Apa Itu Zero Sequence Impedance?", "id": "Impedansi Saat, Arus Gangguan Tanah." },
  { "en": "Apa Itu Negative Sequence Impedance?", "id": "Impedansi Saat, Beban Tidak Seimbang." },
  { "en": "Apa Itu Positive Sequence Impedance?", "id": "Impedansi Saat, Operasi Normal Seimbang." },
  { "en": "Apa Rumus Komponen Simetris Nol?", "id": "I0 = (Ia + Ib + Ic) / 3." },
  { "en": "Apa Itu Hysteresis Motor?", "id": "Motor Sinkron, Torsi Histeresis Rotor." },
  { "en": "Apa Kelebihan Hysteresis Motor?", "id": "Sangat Halus, Dan Tidak Bising." },
  { "en": "Apa Itu Reluctance Motor?", "id": "Motor Torsi Reluktansi, Tanpa Magnet." },
  { "en": "Apa Itu Universal Motor?", "id": "Motor Seri, Bisa AC DC." },
  { "en": "Apa Ciri Fisik Universal Motor?", "id": "Memiliki Komutator, Dan Kecepatan Tinggi." },
  { "en": "Apa Aplikasi Universal Motor?", "id": "Bor Listrik, Dan Blender." },
  { "en": "Apa Itu Linear Induction Motor?", "id": "Stator Dan Rotor, Terbentang Lurus." },
  { "en": "Apa Aplikasi Linear Motor?", "id": "Kereta Maglev, Dan Pintu Otomatis." },
  { "en": "Apa Itu Repulsion Motor?", "id": "Motor AC, Dengan Sikat Hubung Singkat." },
  { "en": "Apa Rumus Daya Aliran P?", "id": "P = (V1 x V2 / X) x Sin δ." },
  { "en": "Apa Itu Sudut Daya Delta?", "id": "Beda Sudut Tegangan, Kirim Terima." },
  { "en": "Apa Syarat Stabilitas Steady State?", "id": "dP / dδ > 0." },
  { "en": "Apa Batas Maksimum Daya Transmisi?", "id": "Saat Sudut Delta, 90 Derajat." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Analisis Kestabilan, Transien Generator." },
  { "en": "Apa Itu Critical Clearing Angle?", "id": "Sudut Maksimum, Pemutusan Gangguan Stabil." },
  { "en": "Apa Itu Fault Ride Through?", "id": "Pembangkit Tetap Koneksi, Saat Gangguan." },
  { "en": "Apa Itu Grid Code?", "id": "Aturan Teknis, Koneksi Ke Jaringan." },
  { "en": "Apa Fungsi Excitation System Limiter?", "id": "Mencegah Overheating, Rotor Atau Stator." },
  { "en": "Apa Itu Under Excitation Limiter?", "id": "Mencegah Hilang Sinkronisasi Generator." },
  { "en": "Apa Itu Over Excitation Limiter?", "id": "Mencegah Panas Berlebih, Belitan Medan." },
  { "en": "Apa Itu Power System Stabilizer?", "id": "Meredam Osilasi Frekuensi Rendah." },
  { "en": "Apa Input Sinyal PSS?", "id": "Kecepatan Rotor, Atau Daya Elektrik." },
  { "en": "Apa Itu Digital Twin Grid?", "id": "Simulasi Real Time, Sistem Tenaga." },
  { "en": "Apa Itu Phasor Data Concentrator?", "id": "Pengumpul Data, Dari Banyak PMU." },
  { "en": "Apa Itu Geomagnetic Induced Current?", "id": "Arus Tanah, Akibat Badai Matahari." },
  { "en": "Apa Bahaya GIC Pada Trafo?", "id": "Saturasi Inti, Dan Pemanasan Lebih." },
  { "en": "Apa Itu Subsynchronous Resonance?", "id": "Interaksi Turbin, Dan Kapasitor Seri." },
  { "en": "Apa Frekuensi Subsynchronous?", "id": "Di Bawah Frekuensi Jaringan 50Hz." },
  { "en": "Apa Alat Peredam SSR?", "id": "Torsional Stress Relay." },
  { "en": "Apa Itu Black Start Unit?", "id": "Pembangkit Penstart Awal, Pasca Blackout." },
  { "en": "Apa Rumus Kapasitas Kanal Shannon?", "id": "C = B x log2(1 + SNR)." },
  { "en": "Apa Itu Entropi Informasi?", "id": "Ukuran Ketidakpastian, Atau Keacakan Data." },
  { "en": "Apa Itu Huffman Coding?", "id": "Kompresi Data, Tanpa Kehilangan Informasi." },
  { "en": "Apa Itu Hamming Code?", "id": "Kode Koreksi Error, Satu Bit." },
  { "en": "Apa Itu Cyclic Redundancy Check (CRC)?", "id": "Deteksi Error, Menggunakan Pembagian Polinomial." },
  { "en": "Apa Itu DIBL Pada MOSFET?", "id": "Penurunan Barrier, Akibat Tegangan Drain." },
  { "en": "Apa Kepanjangan DIBL?", "id": "Drain Induced, Barrier Lowering." },
  { "en": "Apa Itu Velocity Saturation?", "id": "Kecepatan Elektron Mentok, Meski Medan Naik." },
  { "en": "Apa Itu Hot Carrier Injection?", "id": "Elektron Berenergi Tinggi, Merusak Oksida Gate." },
  { "en": "Apa Akibat Hot Carrier Injection?", "id": "Ambang Tegangan Bergeser, Umur Chip Pendek." },
  { "en": "Apa Itu FinFET Transistor?", "id": "Transistor 3D, Sirip Vertikal." },
  { "en": "Apa Kelebihan FinFET?", "id": "Kendali Gate Lebih Baik, Bocor Rendah." },
  { "en": "Apa Itu Gate Oxide Breakdown?", "id": "Lapisan Isolator Gate, Jebol Permanen." },
  { "en": "Apa Itu Observability Sistem Kendali?", "id": "Kemampuan Mengetahui State, Dari Output." },
  { "en": "Apa Itu Controllability Sistem Kendali?", "id": "Kemampuan Menggerakkan State, Ke Target." },
  { "en": "Apa Itu Kalman Filter?", "id": "Estimasi State, Dari Data Ber-Noise." },
  { "en": "Apa Fungsi Observer Pada Kendali?", "id": "Mengestimasi State, Yang Tidak Terukur." },
  { "en": "Apa Itu Robust Control?", "id": "Kendali Tahan Terhadap, Ketidakpastian Model." },
  { "en": "Apa Itu Deadbeat Control?", "id": "Mencapai Target, Dalam Jumlah Langkah Tetap." },
  { "en": "Apa Itu Sliding Mode Control?", "id": "Kendali Switching, Memaksa State Ke Garis." },
  { "en": "Apa Itu Pulse Oximetry?", "id": "Mengukur Saturasi Oksigen, Secara Non-Invasif." },
  { "en": "Apa Prinsip Pulse Oximeter?", "id": "Penyerapan Cahaya Merah, Dan Inframerah." },
  { "en": "Apa Itu Galvanic Skin Response?", "id": "Perubahan Konduktivitas Kulit, Akibat Keringat." },
  { "en": "Apa Bahaya Microshock Medis?", "id": "Arus Kecil Langsung, Ke Jantung." },
  { "en": "Apa Itu Defibrillator Biphasic?", "id": "Arus Kejut, Dua Arah Bolak-Balik." },
  { "en": "Apa Kelebihan Defibrillator Biphasic?", "id": "Energi Lebih Rendah, Lebih Efektif." },
  { "en": "Apa Itu MRI (Magnetic Resonance Imaging)?", "id": "Pencitraan Menggunakan, Medan Magnet Kuat." },
  { "en": "Apa Satuan Kuat Medan MRI?", "id": "Tesla." },
  { "en": "Apa Itu Flywheel Energy Storage?", "id": "Menyimpan Energi, Dalam Putaran Rotor." },
  { "en": "Apa Itu SMES Storage?", "id": "Penyimpanan Energi, Magnetik Superkonduktor." },
  { "en": "Apa Itu CAES Storage?", "id": "Compressed Air, Energy Storage." },
  { "en": "Apa Itu Pumped Hydro Storage?", "id": "Air Takungan Atas, Dilepas Ke Bawah." },
  { "en": "Apa Itu Solid State Battery?", "id": "Baterai Dengan, Elektrolit Padat." },
  { "en": "Apa Kelebihan Solid State Battery?", "id": "Lebih Aman, Dan Densitas Energi Tinggi." },
  { "en": "Apa Itu Flow Battery?", "id": "Elektrolit Cair, Disimpan Di Tangki Luar." },
  { "en": "Apa Kelebihan Flow Battery?", "id": "Kapasitas Mudah Ditambah, Umur Panjang." },
  { "en": "Apa Itu Link Budget Satelit?", "id": "Perhitungan Total Gain, Dan Loss Sinyal." },
  { "en": "Apa Itu Free Space Path Loss?", "id": "Redaman Sinyal, Di Ruang Hampa." },
  { "en": "Apa Rumus Free Space Path Loss?", "id": "FSPL = 20log(d) + 20log(f) + K." },
  { "en": "Apa Itu Transponder Bent Pipe?", "id": "Satelit Hanya Menguatkan, Dan Menggeser Frekuensi." },
  { "en": "Apa Itu Regenerative Transponder?", "id": "Satelit Memproses Sinyal, Sebelum Dikirim Ulang." },
  { "en": "Apa Itu VSAT (Very Small Aperture)?", "id": "Terminal Satelit Kecil, Di Bumi." },
  { "en": "Apa Itu Footprint Satelit?", "id": "Area Cakupan Sinyal, Di Permukaan Bumi." },
  { "en": "Apa Itu Telemetry Tracking Command?", "id": "Sistem Kendali, Operasional Satelit." },
  { "en": "Apa Itu Out Of Step Protection?", "id": "Proteksi Saat Generator, Hilang Sinkronisasi." },
  { "en": "Apa Itu Pole Slipping?", "id": "Pergeseran Kutub Magnet, Rotor Generator." },
  { "en": "Apa Itu Arc Horn Isolator?", "id": "Melindungi Isolator, Dari Busur Api." },
  { "en": "Apa Itu Grading Ring?", "id": "Meratakan Distribusi Tegangan, Isolator Gantung." },
  { "en": "Apa Itu Spacer Damper?", "id": "Pemisah Kabel, Sekaligus Peredam Getaran." },
  { "en": "Apa Itu Stockbridge Damper?", "id": "Peredam Getaran Kabel, Berbentuk Tulang." },
  { "en": "Apa Itu Galloping Conductor?", "id": "Getaran Kabel Amplitudo Besar, Frekuensi Rendah." },
  { "en": "Apa Penyebab Galloping Kabel?", "id": "Angin Kencang, Dan Lapisan Es." },
  { "en": "Apa Itu Photopic Vision?", "id": "Penglihatan Mata, Saat Cahaya Terang." },
  { "en": "Apa Itu Scotopic Vision?", "id": "Penglihatan Mata, Saat Cahaya Redup." },
  { "en": "Apa Itu Mesopic Vision?", "id": "Penglihatan Kondisi, Cahaya Menengah." },
  { "en": "Apa Warna Cahaya Natrium Tekanan Rendah?", "id": "Kuning Monokromatik." },
  { "en": "Apa Kelebihan Lampu Natrium?", "id": "Efisiensi Lumen Per Watt, Tertinggi." },
  { "en": "Apa Kekurangan Lampu Natrium?", "id": "Reproduksi Warna, Sangat Buruk." },
  { "en": "Apa Itu Unified Glare Rating (UGR)?", "id": "Indeks Silau, Pencahayaan Ruangan." },
  { "en": "Apa Itu Luminous Intensity Distribution?", "id": "Pola Sebaran Cahaya, Lampu." },
  { "en": "Apa Itu Ethernet Frame Preamble?", "id": "Pola Bit Sinkronisasi, Awal Paket." },
  { "en": "Apa Itu MAC Address Table?", "id": "Daftar Alamat Fisik, Di Switch." },
  { "en": "Apa Itu ARP (Address Resolution Protocol)?", "id": "Menerjemahkan IP Address, Ke MAC Address." },
  { "en": "Apa Itu VLAN (Virtual LAN)?", "id": "Memecah Jaringan Fisik, Secara Logika." },
  { "en": "Apa Itu Trunking VLAN?", "id": "Membawa Banyak VLAN, Dalam Satu Kabel." },
  { "en": "Apa Protokol Trunking Standar?", "id": "IEEE 802.1Q." },
  { "en": "Apa Itu Spanning Tree Protocol (STP)?", "id": "Mencegah Looping, Pada Jaringan Switch." },
  { "en": "Apa Itu PoE Injector?", "id": "Alat Menambah Daya, Ke Kabel LAN." },
  { "en": "Apa Itu PoE Splitter?", "id": "Memisahkan Daya, Dan Data Di Ujung." },
  { "en": "Apa Itu Rogowski Coil Output?", "id": "Tegangan AC, Pergeseran Fasa 90 Derajat." },
  { "en": "Apa Itu Fluxgate Sensor?", "id": "Sensor Magnetik Presisi, Saturasi Inti." },
  { "en": "Apa Itu SQUID Sensor?", "id": "Sensor Magnetik Kuantum, Paling Sensitif." },
  { "en": "Apa Fungsi Lock-In Amplifier?", "id": "Mendeteksi Sinyal Kecil, Dalam Noise." },
  { "en": "Apa Itu Virtual Instrument?", "id": "Alat Ukur Berbasis, Perangkat Lunak PC." },
  { "en": "Apa Itu LabVIEW?", "id": "Bahasa Pemrograman Grafis, Instrumentasi." },
  { "en": "Apa Itu GPIB Interface?", "id": "General Purpose Interface Bus, IEEE 488." },
  { "en": "Apa Itu Bus Arbitrator?", "id": "Pengatur Giliran Akses, Bus Data." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Tidak Ditemukan, Di Memori Cache." },
  { "en": "Apa Itu Cache Hit?", "id": "Data Ditemukan, Di Memori Cache." },
  { "en": "Apa Itu Branch Prediction CPU?", "id": "Menebak Arah Cabang, Instruksi Program." },
  { "en": "Apa Itu Superscalar Processor?", "id": "Mengeksekusi Banyak Instruksi, Per Siklus." },
  { "en": "Apa Itu Hyper-Threading?", "id": "Satu Inti Fisik, Dua Inti Logika." },
  { "en": "Apa Itu Dark Silicon?", "id": "Bagian Chip Mati, Hemat Daya." },
  { "en": "Apa Rumus Reaktansi Kapasitif?", "id": "XC = 1 / (2 x π x f x C)." },
  { "en": "Apa Rumus Reaktansi Induktif?", "id": "XL = 2 x π x f x L." },
  { "en": "Apa Rumus Impedansi Total Seri?", "id": "Z = √(R^2 + (XL - XC)^2)." },
  { "en": "Apa Rumus Faktor Kualitas L?", "id": "Q = (2 x π x f x L) / R." },
  { "en": "Apa Itu HVDC Back To Back?", "id": "Penyearah Dan Inverter, Satu Lokasi." },
  { "en": "Apa Fungsi HVDC Back To Back?", "id": "Menghubungkan Dua Grid, Frekuensi Beda." },
  { "en": "Apa Itu Monopolar HVDC?", "id": "Satu Konduktor, Kembali Lewat Tanah/Laut." },
  { "en": "Apa Itu Bipolar HVDC?", "id": "Dua Konduktor, Positif Dan Negatif." },
  { "en": "Apa Itu Homopolar HVDC?", "id": "Dua Konduktor, Polaritas Sama." },
  { "en": "Apa Itu Gas Insulated Line (GIL)?", "id": "Pipa Transmisi, Isolasi Gas SF6." },
  { "en": "Apa Kelebihan GIL?", "id": "Kapasitas Besar, Radiasi Magnet Rendah." },
  { "en": "Apa Itu Superconducting Cable?", "id": "Kabel Tanpa Hambatan, Suhu Dingin." },
  { "en": "Apa Bahan Superkonduktor Kabel?", "id": "HTS, High Temperature Superconductor." },
  { "en": "Apa Pendingin Kabel Superkonduktor?", "id": "Nitrogen Cair." },
  { "en": "Apa Itu Cryostat Kabel?", "id": "Selubung Termal, Kabel Superkonduktor." },
  { "en": "Apa Tegangan Listrik KRL Jabodetabek?", "id": "1500 Volt, Arus Searah DC." },
  { "en": "Apa Fungsi Pantograf Kereta Listrik?", "id": "Mengambil Arus, Dari Listrik Aliran Atas." },
  { "en": "Apa Itu Listrik Aliran Atas?", "id": "Kabel Suplai Daya, Di Atas Rel." },
  { "en": "Apa Itu Rel Ketiga Atau Third Rail?", "id": "Rel Tambahan, Penyuplai Listrik Di Bawah." },
  { "en": "Apa Fungsi Inverter VVVF Kereta?", "id": "Mengatur Kecepatan, Motor Traksi AC." },
  { "en": "Apa Itu Pengereman Regeneratif KRL?", "id": "Mengubah Energi Gerak, Menjadi Listrik Kembali." },
  { "en": "Apa Itu Traksi Motor Induksi?", "id": "Motor Penggerak Utama, Kereta Modern." },
  { "en": "Apa Itu Chopper Control Kereta?", "id": "Pengendali Motor DC, Kereta Lama." },
  { "en": "Apa Itu Bogie Kereta Api?", "id": "Rangka Roda, Penopang Gerbong Kereta." },
  { "en": "Apa Itu Deadman Switch Kereta?", "id": "Pedal Keselamatan, Masinis Kereta Api." },
  { "en": "Apa Kepanjangan GTO Thyristor?", "id": "Gate Turn Off, Thyristor." },
  { "en": "Apa Kelebihan GTO Dibanding SCR?", "id": "Bisa Dimatikan, Melalui Kaki Gate." },
  { "en": "Apa Kepanjangan IGCT Thyristor?", "id": "Integrated Gate, Commutated Thyristor." },
  { "en": "Apa Itu Logika DTL?", "id": "Diode Transistor Logic, Logika Lawas." },
  { "en": "Apa Itu Logika RTL?", "id": "Resistor Transistor Logic, Logika Lawas." },
  { "en": "Apa Itu Logika ECL?", "id": "Emitter Coupled Logic, Sangat Cepat." },
  { "en": "Apa Kelebihan Logika ECL?", "id": "Kecepatan Switching, Paling Tinggi." },
  { "en": "Apa Kekurangan Logika ECL?", "id": "Konsumsi Daya, Sangat Besar." },
  { "en": "Apa Itu Logika I2L?", "id": "Integrated Injection Logic." },
  { "en": "Apa Itu Fan Out Gerbang Logika?", "id": "Jumlah Beban Gerbang, Yang Bisa Digerakkan." },
  { "en": "Apa Itu Propagation Delay Time?", "id": "Waktu Tunda Sinyal, Input Ke Output." },
  { "en": "Apa Itu Noise Margin Logika?", "id": "Batas Toleransi, Gangguan Sinyal." },
  { "en": "Apa Itu Floating Input?", "id": "Kaki Input, Tidak Terhubung Apapun." },
  { "en": "Apa Bahaya Floating Input CMOS?", "id": "Osilasi, Dan Panas Berlebih." },
  { "en": "Apa Itu Economizer Pada Boiler?", "id": "Pemanas Awal Air, Sebelum Masuk Drum." },
  { "en": "Apa Itu Superheater Pada Boiler?", "id": "Pemanas Uap Basah, Menjadi Uap Kering." },
  { "en": "Apa Itu Steam Drum?", "id": "Tangki Pemisah, Air Dan Uap." },
  { "en": "Apa Itu Condenser Pembangkit?", "id": "Pendingin Uap Bekas, Menjadi Air." },
  { "en": "Apa Itu Cooling Tower?", "id": "Menara Pendingin Air, Sirkulasi Kondensor." },
  { "en": "Apa Itu Deaerator?", "id": "Penghilang Oksigen, Dari Air Umpan." },
  { "en": "Apa Bahaya Oksigen Dalam Boiler?", "id": "Menyebabkan Korosi, Pada Pipa." },
  { "en": "Apa Itu Coal Feeder?", "id": "Pengatur Jumlah Batubara, Masuk Gilingan." },
  { "en": "Apa Itu Soot Blower?", "id": "Pembersih Jelaga, Pada Pipa Boiler." },
  { "en": "Apa Itu Safety Valve Boiler?", "id": "Katup Pengaman, Tekanan Berlebih." },
  { "en": "Apa Itu Blowdown Boiler?", "id": "Membuang Endapan, Dari Dasar Boiler." },
  { "en": "Apa Itu Baseband Signal?", "id": "Sinyal Asli, Sebelum Dimodulasi." },
  { "en": "Apa Itu Carrier Signal?", "id": "Gelombang Pembawa, Frekuensi Tinggi." },
  { "en": "Apa Itu Single Sideband (SSB)?", "id": "Modulasi AM, Hemat Bandwidth." },
  { "en": "Apa Itu Lower Sideband (LSB)?", "id": "Pita Sisi Bawah, Modulasi AM." },
  { "en": "Apa Itu Upper Sideband (USB)?", "id": "Pita Sisi Atas, Modulasi AM." },
  { "en": "Apa Itu Vestigial Sideband (VSB)?", "id": "Modulasi AM, Untuk Sinyal Video TV." },
  { "en": "Apa Itu Index Modulasi AM?", "id": "Rasio Amplitudo Sinyal, Dan Carrier." },
  { "en": "Apa Akibat Overmodulasi AM?", "id": "Cacat Sinyal, Dan Interferensi." },
  { "en": "Apa Itu Capture Effect FM?", "id": "Sinyal Kuat, Menekan Sinyal Lemah." },
  { "en": "Apa Itu Pre-Emphasis?", "id": "Menguatkan Nada Tinggi, Sebelum Transmisi." },
  { "en": "Apa Itu De-Emphasis?", "id": "Melemahkan Nada Tinggi, Di Penerima." },
  { "en": "Apa Fungsi Phase Locked Loop?", "id": "Mengunci Fasa, Dan Frekuensi Sinyal." },
  { "en": "Apa Itu Transistor Efek Medan JFET?", "id": "Junction Field, Effect Transistor." },
  { "en": "Apa Saluran Pada JFET?", "id": "Kanal N, Atau Kanal P." },
  { "en": "Apa Sifat Input JFET?", "id": "Impedansi Input, Sangat Tinggi." },
  { "en": "Apa Itu Pinch Off Voltage?", "id": "Tegangan Saat, Arus Drain Berhenti." },
  { "en": "Apa Itu Transconductance Gm?", "id": "Perubahan Arus Output, Per Tegangan Input." },
  { "en": "Apa Rumus Transconductance JFET?", "id": "gm = ΔId / ΔVgs." },
  { "en": "Apa Itu Depletion Mode MOSFET?", "id": "Normal On, Perlu Tegangan Mematikan." },
  { "en": "Apa Itu Enhancement Mode MOSFET?", "id": "Normal Off, Perlu Tegangan Menghidupkan." },
  { "en": "Apa Itu VMOS Power MOSFET?", "id": "Struktur V, Untuk Daya Tinggi." },
  { "en": "Apa Kelebihan Power MOSFET?", "id": "Switching Cepat, Dan Mudah Diparalel." },
  { "en": "Apa Itu Fourier Transform?", "id": "Mengubah Domain Waktu, Ke Frekuensi." },
  { "en": "Apa Itu Laplace Transform?", "id": "Analisis Sistem, Domain S Kompleks." },
  { "en": "Apa Itu Z-Transform?", "id": "Analisis Sinyal, Diskrit Digital." },
  { "en": "Apa Itu Impulse Response?", "id": "Output Sistem, Saat Diberi Impuls." },
  { "en": "Apa Itu Step Response?", "id": "Output Sistem, Saat Diberi Tegangan DC." },
  { "en": "Apa Itu Transfer Function?", "id": "Perbandingan Output, Dan Input." },
  { "en": "Apa Itu Pole Sistem?", "id": "Nilai S, Penyebab Transfer Tak Hingga." },
  { "en": "Apa Itu Zero Sistem?", "id": "Nilai S, Penyebab Transfer Nol." },
  { "en": "Apa Syarat Sistem Stabil?", "id": "Pole Berada, Di Sebelah Kiri S-Plane." },
  { "en": "Apa Itu BIBO Stability?", "id": "Bounded Input, Bounded Output." },
  { "en": "Apa Kode ANSI Device 50BF?", "id": "Breaker Failure, Relay." },
  { "en": "Apa Fungsi Breaker Failure Relay?", "id": "Trip Backup, Jika Pemutus Macet." },
  { "en": "Apa Kode ANSI Device 79?", "id": "Reclosing Relay, Penutup Otomatis." },
  { "en": "Apa Kode ANSI Device 21?", "id": "Distance Relay, Proteksi Jarak." },
  { "en": "Apa Fungsi Relay Jarak?", "id": "Mengukur Impedansi, Ke Titik Gangguan." },
  { "en": "Apa Itu Zona 1 Relay Jarak?", "id": "Melindungi 80%, Panjang Saluran." },
  { "en": "Apa Waktu Kerja Zona 1?", "id": "Seketika, Tanpa Tunda Waktu." },
  { "en": "Apa Itu Power Swing Blocking?", "id": "Mencegah Trip, Saat Ayunan Daya." },
  { "en": "Apa Itu Switchgear Cubicle?", "id": "Lemari Panel, Pemutus Tegangan Menengah." },
  { "en": "Apa Itu Ring Main Unit (RMU)?", "id": "Panel Distribusi, Sistem Loop." },
  { "en": "Apa Media Isolasi RMU?", "id": "Gas SF6, Dalam Tangki Tertutup." },
  { "en": "Apa Itu Load Break Switch (LBS)?", "id": "Sakelar Pemutus, Saat Berbeban." },
  { "en": "Apa Itu Disconnecting Switch (DS)?", "id": "Sakelar Pemisah, Tanpa Beban." },
  { "en": "Apa Itu Earthing Switch?", "id": "Sakelar Pembumian, Untuk Keamanan." },
  { "en": "Apa Itu Interlock Mekanik?", "id": "Mencegah Kesalahan, Urutan Operasi." },
  { "en": "Apa Itu Partial Discharge Detector?", "id": "Alat Deteksi, Kebocoran Isolasi Dini." },
  { "en": "Apa Itu Tan Delta Test?", "id": "Menguji Kualitas, Isolasi Trafo." },
  { "en": "Apa Itu Thermovision Camera?", "id": "Kamera Deteksi, Titik Panas Sambungan." },
  { "en": "Apa Itu Corona Camera?", "id": "Kamera Deteksi, Pendaran UV Corona." },
  { "en": "Apa Rumus Arus Tiga Fasa?", "id": "I = P / (√3 x V x Cosφ)." },
  { "en": "Apa Rumus Drop Tegangan AC?", "id": "ΔV = I x (R cosφ + X sinφ)." },
  { "en": "Apa Rumus Daya Motor HP?", "id": "1 HP = 746 Watt." },
  { "en": "Apa Rumus Energi Tersimpan L?", "id": "E = 1/2 x L x I^2." },
  { "en": "Apa Rumus Energi Tersimpan C?", "id": "E = 1/2 x C x V^2." },
  { "en": "Apa Rumus Reaktansi Induktif XL?", "id": "XL = 2 x π x f x L." },
  { "en": "Apa Rumus Reaktansi Kapasitif XC?", "id": "XC = 1 / (2 x π x f x C)." },
  { "en": "Apa Rumus Frekuensi Resonansi Seri?", "id": "f = 1 / (2 x π x √LC)." },
  { "en": "Apa Itu Supernode?", "id": "Dua Node, Terhubung Sumber Tegangan." },
  { "en": "Apa Itu Supermesh?", "id": "Dua Loop, Terbagi Sumber Arus." },
  { "en": "Apa Itu Dependent Source?", "id": "Sumber Dikendalikan, Tegangan Atau Arus Lain." },
  { "en": "Apa Itu Voltage Controlled Voltage Source?", "id": "Sumber Tegangan, Kendali Tegangan (VCVS)." },
  { "en": "Apa Itu Current Controlled Voltage Source?", "id": "Sumber Tegangan, Kendali Arus (CCVS)." },
  { "en": "Apa Itu Voltage Controlled Current Source?", "id": "Sumber Arus, Kendali Tegangan (VCCS)." },
  { "en": "Apa Itu Efek Kulit Kabel?", "id": "Arus Mengalir, Di Permukaan Konduktor." },
  { "en": "Apa Itu Proximity Effect?", "id": "Gangguan Arus, Akibat Kabel Berdekatan." },
  { "en": "Apa Itu Transposisi Kabel?", "id": "Menukar Posisi Fasa, Sepanjang Jalur." },
  { "en": "Apa Fungsi Transposisi Kabel?", "id": "Menyeimbangkan Impedansi, Dan Induktansi Fasa." },
  { "en": "Apa Itu Bundled Conductor?", "id": "Beberapa Kabel, Dalam Satu Fasa." },
  { "en": "Apa Keuntungan Bundled Conductor?", "id": "Mengurangi Corona, Dan Reaktansi Induktif." },
  { "en": "Apa Itu Spacer Damper?", "id": "Pemisah Kabel, Dan Peredam Getaran." },
  { "en": "Apa Itu Galloping Kabel?", "id": "Getaran Amplitudo Besar, Frekuensi Rendah." },
  { "en": "Apa Itu Stockbridge Damper?", "id": "Peredam Getaran, Berbentuk Tulang Anjing." },
  { "en": "Apa Itu Isolator Pin Post?", "id": "Isolator Tumpu, Tegangan Menengah." },
  { "en": "Apa Itu Isolator Suspension?", "id": "Isolator Gantung, Tegangan Tinggi." },
  { "en": "Apa Itu Strain Clamp?", "id": "Klem Penarik Kabel, Pada Tiang." },
  { "en": "Apa Itu Compression Connector?", "id": "Sambungan Kabel, Sistem Pres Hidrolik." },
  { "en": "Apa Itu Jointing Kit?", "id": "Paket Material, Sambungan Kabel Tanah." },
  { "en": "Apa Itu Termination Kit?", "id": "Paket Material, Ujung Kabel." },
  { "en": "Apa Itu Stress Control Tube?", "id": "Meratakan Medan Listrik, Ujung Kabel." },
  { "en": "Apa Itu Semikonduktor Kabel?", "id": "Lapisan Hitam, Perata Medan Listrik." },
  { "en": "Apa Fungsi Screen Tembaga Kabel?", "id": "Mengalirkan Arus Bocor, Ke Tanah." },
  { "en": "Apa Itu XLPE Insulation?", "id": "Cross Linked, Polyethylene." },
  { "en": "Apa Suhu Kerja XLPE?", "id": "Maksimal 90, Derajat Celcius." },
  { "en": "Apa Suhu Hubung Singkat XLPE?", "id": "Maksimal 250, Derajat Celcius." },
  { "en": "Apa Itu PVC Insulation?", "id": "Poly Vinyl Chloride." },
  { "en": "Apa Suhu Kerja PVC?", "id": "Maksimal 70, Derajat Celcius." },
  { "en": "Apa Itu Low Smoke Zero Halogen?", "id": "Kabel Tahan Api, Asap Sedikit." },
  { "en": "Apa Itu Fire Resistant Cable?", "id": "Kabel Tetap Operasi, Saat Kebakaran." },
  { "en": "Apa Standar Kabel Fire Resistant?", "id": "IEC 60331." },
  { "en": "Apa Standar Kabel Flame Retardant?", "id": "IEC 60332." },
  { "en": "Apa Itu Busduct Aluminium?", "id": "Batang Penghantar, Dalam Casing Logam." },
  { "en": "Apa Kelebihan Busduct?", "id": "Rapi, Aman, Dan Kapasitas Besar." },
  { "en": "Apa Itu Sandwich Busduct?", "id": "Batang Konduktor, Disusun Rapat." },
  { "en": "Apa Itu Air Insulated Busduct?", "id": "Batang Konduktor, Berjarak Udara." },
  { "en": "Apa Itu IP Rating Busduct?", "id": "Tingkat Perlindungan, Terhadap Air Debu." },
  { "en": "Apa Itu Short Circuit Withstand?", "id": "Ketahanan Mekanik, Saat Hubung Singkat." },
  { "en": "Apa Satuan Short Circuit Capacity?", "id": "Kilo Ampere, Selama Satu Detik." },
  { "en": "Apa Itu Icu Breaker?", "id": "Ultimate Short Circuit, Breaking Capacity." },
  { "en": "Apa Itu Ics Breaker?", "id": "Service Short Circuit, Breaking Capacity." },
  { "en": "Apa Itu Icw Breaker?", "id": "Rated Short Time, Withstand Current." },
  { "en": "Apa Itu Category A Breaker?", "id": "Tanpa Tunda Waktu, Trip Instan." },
  { "en": "Apa Itu Category B Breaker?", "id": "Memiliki Tunda Waktu, Untuk Selektivitas." },
  { "en": "Apa Itu Selectivity Proteksi?", "id": "Hanya Breaker Terdekat, Yang Trip." },
  { "en": "Apa Itu Cascading Proteksi?", "id": "Breaker Hulu Membantu, Breaker Hilir." },
  { "en": "Apa Itu Current Limiting Breaker?", "id": "Membatasi Arus Gangguan, Dengan Cepat." },
  { "en": "Apa Itu Arc Chute Breaker?", "id": "Ruang Pemadam, Busur Api Listrik." },
  { "en": "Apa Fungsi Kontak Utama?", "id": "Mengalirkan Arus Beban, Terus Menerus." },
  { "en": "Apa Fungsi Kontak Arcing?", "id": "Menahan Busur Api, Saat Switching." },
  { "en": "Apa Itu Spring Charging Motor?", "id": "Motor Pengisi Pegas, Mekanik Breaker." },
  { "en": "Apa Itu Shunt Trip Coil?", "id": "Koill Pemutus Breaker, Jarak Jauh." },
  { "en": "Apa Itu Under Voltage Release?", "id": "Trip Otomatis, Saat Tegangan Hilang." },
  { "en": "Apa Itu Auxiliary Contact?", "id": "Kontak Bantu, Untuk Indikasi Status." },
  { "en": "Apa Itu Rack In Rack Out?", "id": "Posisi Masuk Keluar, Drawout Breaker." },
  { "en": "Apa Posisi Test Breaker?", "id": "Sirkuit Kontrol Aktif, Utama Putus." },
  { "en": "Apa Posisi Service Breaker?", "id": "Terhubung Penuh, Ke Busbar Utama." },
  { "en": "Apa Itu Gas SF6 Pressure Gauge?", "id": "Pengukur Tekanan, Gas Isolasi." },
  { "en": "Apa Itu Vacuum Interrupter?", "id": "Botol Hampa, Pemadam Busur VCB." },
  { "en": "Apa Indikator Keausan Kontak?", "id": "Contact Wear Indicator, Pada VCB." },
  { "en": "Apa Itu Pole Simultanity?", "id": "Keserentakan Kontak, Menutup Atau Membuka." },
  { "en": "Apa Batas Beda Waktu Kontak?", "id": "Biasanya Di Bawah, 10 Milidetik." },
  { "en": "Apa Itu Closing Time Breaker?", "id": "Waktu Dari Perintah, Hingga Menutup." },
  { "en": "Apa Itu Opening Time Breaker?", "id": "Waktu Dari Perintah, Hingga Membuka." },
  { "en": "Apa Itu Break Time Total?", "id": "Opening Time, Ditambah Arcing Time." },
  { "en": "Apa Itu Making Current?", "id": "Arus Puncak, Saat Menutup Breaker." },
  { "en": "Apa Rumus Making Current?", "id": "Im = 2.5 x Isc." },
  { "en": "Apa Itu Latching Current Relay?", "id": "Arus Minimum, Agar Relay Menahan." },
  { "en": "Apa Itu Drop Off Current Relay?", "id": "Arus Saat Relay, Kembali Reset." },
  { "en": "Apa Itu Hysteresis Relay?", "id": "Selisih Arus Pickup, Dan Dropout." },
  { "en": "Apa Fungsi Annunciator Panel?", "id": "Papan Alarm, Indikator Gangguan Visual." },
  { "en": "Apa Fungsi Tombol Acknowledge?", "id": "Mematikan Suara Alarm, Sirine." },
  { "en": "Apa Fungsi Tombol Reset Alarm?", "id": "Menghapus Indikasi, Setelah Normal." },
  { "en": "Apa Fungsi Lamp Test?", "id": "Mengecek Kondisi, Lampu Indikator Panel." },
  { "en": "Apa Itu Wiring Diagram?", "id": "Gambar Sambungan Kabel, Secara Fisik." },
  { "en": "Apa Itu Schematic Diagram?", "id": "Gambar Logika Rangkaian, Secara Fungsi." },
  { "en": "Apa Itu Terminal Block?", "id": "Blok Sambungan Kabel, Dalam Panel." },
  { "en": "Apa Itu Ferrule Kabel?", "id": "Selongsong Ujung Kabel, Serabut." },
  { "en": "Apa Itu Spiral Wrapping Band?", "id": "Pelindung Dan Pengikat, Kabel Wiring." },
  { "en": "Apa Itu Ducting Kabel Panel?", "id": "Jalur Kabel, Dalam Panel Listrik." },
  { "en": "Apa Itu Din Rail?", "id": "Rel Dudukan MCB, Dan Komponen." },
  { "en": "Apa Itu Busbar Support?", "id": "Isolator Penjepit, Batang Busbar." },
  { "en": "Apa Standar Warna Tombol Start?", "id": "Hijau." },
  { "en": "Apa Standar Warna Tombol Stop?", "id": "Merah." },
  { "en": "Apa Standar Warna Tombol Reset?", "id": "Biru, Atau Hitam." },
  { "en": "Apa Standar Warna Lampu Run?", "id": "Merah, (Standar PLN/PUIL Lama)." },
  { "en": "Apa Standar Warna Lampu Trip?", "id": "Kuning." },
  { "en": "Apa Standar Warna Lampu Off?", "id": "Hijau, (Standar PLN/PUIL Lama)." },
  { "en": "Apa Itu CT Polarity Test?", "id": "Menentukan Arah, Aliran Arus CT." },
  { "en": "Apa Itu CT Saturation Test?", "id": "Menentukan Titik Jenuh, Inti CT." },
  { "en": "Apa Itu CT Ratio Test?", "id": "Memverifikasi Perbandingan, Arus CT." },
  { "en": "Apa Itu PT Ratio Test?", "id": "Memverifikasi Perbandingan, Tegangan PT." },
  { "en": "Apa Itu Primary Injection Test?", "id": "Menginjeksikan Arus Besar, Ke Busbar." },
  { "en": "Apa Itu Secondary Injection Test?", "id": "Menginjeksikan Arus Kecil, Ke Relay." },
  { "en": "Apa Fungsi Mencegah Trip Palsu?", "id": "Menguji Kestabilan, Sistem Proteksi." },
  { "en": "Apa Itu Trip Circuit Supervision?", "id": "Memantau Kesiapan, Jalur Trip Coil." },
  { "en": "Apa Kode ANSI TCS?", "id": "Device 74." },
  { "en": "Apa Itu DC Ground Fault?", "id": "Kebocoran Arus DC, Ke Tanah." },
  { "en": "Apa Bahaya DC Ground Fault?", "id": "Relay Bisa Trip Sendiri, Atau Gagal." },
  { "en": "Apa Alat Deteksi Ground DC?", "id": "DC Earth Fault Locator." },
  { "en": "Apa Tegangan Baterai Gardu Induk?", "id": "110 VDC, Atau 48 VDC." },
  { "en": "Apa Tipe Baterai Gardu Induk?", "id": "NiCd, Atau Lead Acid VRLA." },
  { "en": "Apa Itu Rectifier Baterai?", "id": "Pengisi Daya, Dan Suplai DC." },
  { "en": "Apa Itu Equalizing Charge?", "id": "Pengisian Tegangan Tinggi, Menyamakan Sel." },
  { "en": "Apa Itu Factory Acceptance Test (FAT)?", "id": "Pengujian Alat, Di Pabrik Pembuat." },
  { "en": "Apa Itu Site Acceptance Test (SAT)?", "id": "Pengujian Alat, Di Lokasi Pemasangan." },
  { "en": "Apa Itu Cold Commissioning?", "id": "Pengujian Sistem, Tanpa Energi Listrik." },
  { "en": "Apa Itu Hot Commissioning?", "id": "Pengujian Sistem, Bertegangan Dan Berbeban." },
  { "en": "Apa Itu As-Built Drawing?", "id": "Gambar Teknik, Sesuai Kondisi Terpasang." },
  { "en": "Apa Itu Punch List Proyek?", "id": "Daftar Kekurangan, Yang Harus Diperbaiki." },
  { "en": "Apa Itu Handover Dokumen?", "id": "Serah Terima, Dokumen Teknis Proyek." },
  { "en": "Apa Itu Availability Factor Pembangkit?", "id": "Kesiapan Unit, Untuk Beroperasi." },
  { "en": "Apa Itu Forced Outage Rate?", "id": "Tingkat Gangguan Paksa, Pembangkit." },
  { "en": "Apa Itu Planned Outage?", "id": "Pemadaman Terencana, Untuk Pemeliharaan." },
  { "en": "Apa Katoda Baterai NMC?", "id": "Nickel, Manganese, Dan Cobalt." },
  { "en": "Apa Kelebihan Baterai NMC?", "id": "Densitas Energi Tinggi, Ukuran Kompak." },
  { "en": "Apa Katoda Baterai LFP?", "id": "Lithium, Iron, Phosphate." },
  { "en": "Apa Kelebihan Baterai LFP?", "id": "Siklus Hidup Panjang, Dan Aman." },
  { "en": "Apa Itu Thermal Runaway Propagation?", "id": "Penyebaran Panas, Antar Sel Baterai." },
  { "en": "Apa Itu Dendrite Growth Baterai?", "id": "Pertumbuhan Kristal Tajam, Penembus Separator." },
  { "en": "Apa Itu Solid Electrolyte Interface (SEI)?", "id": "Lapisan Pasivasi, Pada Anoda Baterai." },
  { "en": "Apa Itu Coulombic Efficiency?", "id": "Efisiensi Muatan, Saat Cas Discas." },
  { "en": "Apa Standar Keamanan Siber Industri?", "id": "IEC 62443." },
  { "en": "Apa Itu Air Gap Network?", "id": "Jaringan Terisolasi Fisik, Dari Internet." },
  { "en": "Apa Itu Data Diode?", "id": "Alat Komunikasi, Satu Arah Fisik." },
  { "en": "Apa Itu Industrial Demilitarized Zone (IDMZ)?", "id": "Zona Penyangga, Antara IT Dan OT." },
  { "en": "Apa Itu PLC Stuxnet?", "id": "Malware Terkenal, Penyerang Sistem SCADA." },
  { "en": "Apa Itu Man In The Middle Attack?", "id": "Peretas Menyadap, Komunikasi Dua Pihak." },
  { "en": "Apa Itu Spoofing Attack?", "id": "Menyamar Sebagai, Perangkat Terpercaya." },
  { "en": "Apa Itu Characteristic Impedance Surge?", "id": "Impedansi Saluran, Tanpa Rugi-Rugi." },
  { "en": "Apa Rumus Impedansi Surja Zc?", "id": "Zc = √(L / C)." },
  { "en": "Apa Itu Propagation Constant Gamma?", "id": "γ = α + jβ." },
  { "en": "Apa Itu Attenuation Constant Alpha?", "id": "Konstanta Redaman, Penurunan Amplitudo." },
  { "en": "Apa Itu Phase Constant Beta?", "id": "Konstanta Fasa, Perubahan Sudut." },
  { "en": "Apa Itu Saluran Transmisi Pendek?", "id": "Panjang Kurang Dari, 80 Kilometer." },
  { "en": "Apa Pengabaian Pada Saluran Pendek?", "id": "Efek Kapasitansi Shunt, Diabaikan." },
  { "en": "Apa Itu Saluran Transmisi Menengah?", "id": "Panjang Antara, 80 Hingga 250 Km." },
  { "en": "Apa Model Rangkaian Saluran Menengah?", "id": "Model Nominal T, Atau Pi." },
  { "en": "Apa Itu Saluran Transmisi Panjang?", "id": "Panjang Lebih Dari, 250 Kilometer." },
  { "en": "Apa Metode Hitung Saluran Panjang?", "id": "Parameter Terdistribusi, Rigorous Method." },
  { "en": "Apa Itu Digital Control System?", "id": "Sistem Kendali, Menggunakan Komputer/Mikro." },
  { "en": "Apa Itu Sample And Hold?", "id": "Mengambil Nilai Sinyal, Dan Menahannya." },
  { "en": "Apa Itu Quantization Error?", "id": "Selisih Nilai Asli, Dan Digital." },
  { "en": "Apa Itu Zero Order Hold (ZOH)?", "id": "Rekonstruksi Sinyal, Tangga Sederhana." },
  { "en": "Apa Itu Mapping Bidang S Ke Z?", "id": "Z = e^(sT)." },
  { "en": "Apa Syarat Kestabilan Bidang Z?", "id": "Pole Di Dalam, Lingkaran Satuan." },
  { "en": "Apa Itu Bilinear Transformation?", "id": "Metode Konversi, Analog Ke Digital." },
  { "en": "Apa Itu Warping Frequency?", "id": "Distorsi Frekuensi, Akibat Transformasi Bilinear." },
  { "en": "Apa Itu Deadbeat Response?", "id": "Output Mencapai Target, Tercepat Tanpa Osilasi." },
  { "en": "Apa Itu Ringing Effect?", "id": "Osilasi Frekuensi Tinggi, Pada Transisi." },
  { "en": "Apa Itu Input Bias Current Op-Amp?", "id": "Arus Masuk Basis, Transistor Input." },
  { "en": "Apa Itu Input Offset Voltage?", "id": "Tegangan Koreksi, Agar Output Nol." },
  { "en": "Apa Itu Common Mode Voltage?", "id": "Rata-Rata Tegangan, Dua Input." },
  { "en": "Apa Itu Bandgap Reference?", "id": "Referensi Tegangan Stabil, Suhu 1.25V." },
  { "en": "Apa Itu Widlar Current Source?", "id": "Sumber Arus Kecil, Resistor Rendah." },
  { "en": "Apa Itu Wilson Current Mirror?", "id": "Cermin Arus, Impedansi Output Tinggi." },
  { "en": "Apa Itu Gilbert Cell?", "id": "Rangkaian Mixer Analog, Empat Kuadran." },
  { "en": "Apa Itu Transimpedance Amplifier (TIA)?", "id": "Pengubah Arus, Menjadi Tegangan." },
  { "en": "Apa Aplikasi Utama TIA?", "id": "Penguat Sensor, Photodiode." },
  { "en": "Apa Itu Droop Control Inverter?", "id": "Berbagi Beban, Tanpa Komunikasi Kabel." },
  { "en": "Apa Itu Virtual Inertia?", "id": "Inverter Meniru Sifat, Massa Putar." },
  { "en": "Apa Itu Grid Forming Inverter?", "id": "Inverter Pembangkit, Tegangan Referensi." },
  { "en": "Apa Itu Grid Following Inverter?", "id": "Inverter Mengikuti, Tegangan Jaringan." },
  { "en": "Apa Itu Anti-Islanding Protection?", "id": "Mencegah Inverter Hidup, Saat Grid Mati." },
  { "en": "Apa Metode Deteksi Islanding Pasif?", "id": "Monitoring Tegangan, Dan Frekuensi." },
  { "en": "Apa Metode Deteksi Islanding Aktif?", "id": "Menginjeksikan Gangguan, Ke Jaringan." },
  { "en": "Apa Itu Power Line Communication (PLC)?", "id": "Kirim Data, Lewat Kabel Listrik." },
  { "en": "Apa Gangguan Utama PLC?", "id": "Noise Dan Redaman, Kabel Listrik." },
  { "en": "Apa Itu Silicon Controlled Switch (SCS)?", "id": "Thyristor Dengan, Dua Kaki Gate." },
  { "en": "Apa Itu Programmable UJT (PUT)?", "id": "UJT Dengan, Tegangan Pemicu Terprogram." },
  { "en": "Apa Itu Quadrac Component?", "id": "Gabungan TRIAC, Dan DIAC Terintegrasi." },
  { "en": "Apa Itu Sidac Component?", "id": "Sakelar Dua Arah, Tegangan Tinggi." },
  { "en": "Apa Itu Ignitron Tube?", "id": "Tabung Penyearah, Daya Sangat Besar." },
  { "en": "Apa Itu Thyratron Tube?", "id": "Tabung Gas, Kendali Sakelar." },
  { "en": "Apa Itu Klystron Tube?", "id": "Tabung Penguat, Gelombang Mikro Linear." },
  { "en": "Apa Itu Magnetron Tube?", "id": "Osilator Gelombang Mikro, Daya Tinggi." },
  { "en": "Apa Aplikasi Magnetron?", "id": "Oven Microwave, Dan Radar." },
  { "en": "Apa Itu Traveling Wave Tube (TWT)?", "id": "Penguat RF, Bandwidth Lebar." },
  { "en": "Apa Itu Isolasi Galvanis Digital?", "id": "Pemisah Sinyal, Menggunakan Optik/Magnetik." },
  { "en": "Apa Itu Digital Isolator Capacitive?", "id": "Isolasi Berbasis, Kapasitor SiO2." },
  { "en": "Apa Kelebihan Digital Isolator?", "id": "Lebih Cepat, Awet Dari Optocoupler." },
  { "en": "Apa Itu Signal Integrity?", "id": "Kualitas Sinyal Listrik, Pada PCB." },
  { "en": "Apa Itu Impedance Discontinuity?", "id": "Perubahan Impedansi, Penyebab Pantulan." },
  { "en": "Apa Itu Microvia PCB?", "id": "Via Diameter Kecil, Laser Drilled." },
  { "en": "Apa Itu Blind Via?", "id": "Via Lapisan Luar, Ke Dalam." },
  { "en": "Apa Itu Buried Via?", "id": "Via Terkubur, Di Lapisan Dalam." },
  { "en": "Apa Itu Stacked Via?", "id": "Via Tumpuk, Vertikal Lurus." },
  { "en": "Apa Itu Staggered Via?", "id": "Via Tangga, Tidak Satu Sumbu." },
  { "en": "Apa Itu Back Drilling PCB?", "id": "Membuang Sisa Plating, Lubang Via." },
  { "en": "Apa Tujuan Back Drilling?", "id": "Menghilangkan Stub, Via Resonansi." },
  { "en": "Apa Rumus Kapasitas Baterai?", "id": "E = V x Ah." },
  { "en": "Apa Rumus Arus Charging C-Rate?", "id": "I = C_rate x Kapasitas." },
  { "en": "Apa Rumus Waktu Charging?", "id": "t = Ah / I_charge." },
  { "en": "Apa Rumus Efisiensi Inverter?", "id": "η = P_ac / P_dc." },
  { "en": "Apa Rumus Disipasi Panas MOSFET?", "id": "P = I^2 x Rds(on)." },
  { "en": "Apa Rumus Tegangan Ripple Kapasitor?", "id": "Vr = I / (f x C)." },
  { "en": "Apa Rumus Frekuensi Osilasi LC?", "id": "f = 1 / (2π√LC)." },
  { "en": "Apa Rumus Arus Jenuh Transistor?", "id": "Ic_sat = Vcc / Rc." },
  { "en": "Apa Rumus Penguatan Tegangan Op-Amp?", "id": "Av = Vout / Vin." },
  { "en": "Apa Rumus Tegangan Efektif Vrms?", "id": "Vrms = Vp x 0.707." },
  { "en": "Apa Itu Resistivitas Tanah?", "id": "Tahanan Jenis Tanah, Satuan Ohm Meter." },
  { "en": "Apa Metode Wenner 4 Titik?", "id": "Metode Pengukuran, Resistivitas Tanah." },
  { "en": "Apa Itu Driven Rod Grounding?", "id": "Batang Grounding, Ditanam Tegak Lurus." },
  { "en": "Apa Itu Mesh Grounding?", "id": "Jaring Kawat Ground, Ditanam Horizontal." },
  { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Ground Radial, Pada Kaki Menara." },
  { "en": "Apa Fungsi Bentonit Grounding?", "id": "Menurunkan Resistivitas, Tanah Sekitar Elektroda." },
  { "en": "Apa Itu Soil Treatment?", "id": "Perlakuan Kimia, Untuk Menurunkan Tahanan." },
  { "en": "Apa Itu Exothermic Welding?", "id": "Las Kimia Tembaga, Sambungan Grounding." },
  { "en": "Apa Nama Lain Exothermic Welding?", "id": "Las Cadwelding." },
  { "en": "Apa Itu GEM Grounding?", "id": "Ground Enhancement Material, Semen Konduktif." },
  { "en": "Apa Itu Tegangan Sentuh Izin?", "id": "Batas Aman Tegangan, Saat Menyentuh Logam." },
  { "en": "Apa Itu Tegangan Langkah Izin?", "id": "Batas Aman Tegangan, Antara Dua Kaki." },
  { "en": "Apa Itu High Speed Circuit Breaker?", "id": "Pemutus Cepat, Untuk Listrik DC." },
  { "en": "Apa Itu Air Blast Circuit Breaker?", "id": "Pemutus Arus, Dengan Hembusan Udara." },
  { "en": "Apa Itu Minimum Oil Circuit Breaker?", "id": "Pemutus Arus, Minyak Volume Sedikit." },
  { "en": "Apa Itu Bulk Oil Circuit Breaker?", "id": "Pemutus Arus, Tangki Minyak Besar." },
  { "en": "Apa Masalah Utama OCB?", "id": "Risiko Kebakaran, Dan Perawatan Tinggi." },
  { "en": "Apa Itu Hybrid Switchgear?", "id": "Gabungan Isolasi Udara, Dan Gas SF6." },
  { "en": "Apa Itu Pass-Through Bushing?", "id": "Isolator Tembus, Dinding Atau Tangki." },
  { "en": "Apa Itu Corona Ring Isolator?", "id": "Cincin Perata, Distribusi Medan Listrik." },
  { "en": "Apa Itu Arcing Horn?", "id": "Tanduk Logam, Pengalih Busur Api." },
  { "en": "Apa Itu Post Insulator?", "id": "Isolator Duduk, Penyangga Rel Busbar." },
  { "en": "Apa Itu Pin Insulator?", "id": "Isolator Pasak, Untuk Jaringan Lurus." },
  { "en": "Apa Itu Shackle Insulator?", "id": "Isolator Belenggu, Tegangan Rendah." },
  { "en": "Apa Itu Guy Insulator?", "id": "Isolator Pada Kawat, Penahan Tiang." },
  { "en": "Apa Itu Strain Insulator?", "id": "Isolator Tarik, Tahan Gaya Mekanis." },
  { "en": "Apa Itu Glass Insulator?", "id": "Isolator Kaca, Tahan Cuaca Ekstrem." },
  { "en": "Apa Itu Polymer Insulator?", "id": "Isolator Karet Silikon, Ringan Hidrofobik." },
  { "en": "Apa Itu Hydrophobicity Isolator?", "id": "Sifat Menolak Air, Mencegah Arus Bocor." },
  { "en": "Apa Itu Leakage Distance?", "id": "Jarak Jalur Kebocoran, Permukaan Isolator." },
  { "en": "Apa Itu ESDD Isolator?", "id": "Equivalent Salt, Deposit Density." },
  { "en": "Apa Itu Polusi Isolator?", "id": "Debu Garam, Penyebab Flashover." },
  { "en": "Apa Itu Hot Stick?", "id": "Tongkat Berisolasi, Pekerjaan Bertegangan." },
  { "en": "Apa Itu Grounding Set?", "id": "Kabel Pembumian Sementara, Saat Perawatan." },
  { "en": "Apa Itu Voltage Detector Stick?", "id": "Tongkat Pendeteksi, Ada Tidaknya Tegangan." },
  { "en": "Apa Itu Phase Comparator?", "id": "Alat Cek Kesamaan Fasa, Tegangan Tinggi." },
  { "en": "Apa Itu Telescopic Stick?", "id": "Tongkat Isolasi, Yang Bisa Memanjang." },
  { "en": "Apa Itu Rubber Insulating Gloves?", "id": "Sarung Tangan Karet, Tahan Tegangan." },
  { "en": "Apa Kelas Sarung Tangan 20kV?", "id": "Kelas 2, Atau Kelas 3." },
  { "en": "Apa Itu Arc Flash Suit?", "id": "Baju Tahan Panas, Ledakan Listrik." },
  { "en": "Apa Satuan Energi Arc Flash?", "id": "Kalori, Per Sentimeter Persegi." },
  { "en": "Apa Itu Safety Padlock?", "id": "Gembok Pengaman, Prosedur LOTO." },
  { "en": "Apa Itu Danger Tag?", "id": "Label Peringatan, Bahaya Jangan Dioperasikan." },
  { "en": "Apa Itu Permit To Work?", "id": "Surat Izin, Melakukan Pekerjaan Berbahaya." },
  { "en": "Apa Itu Single Mode Fiber G.652?", "id": "Serat Optik Standar, Dispersi Normal." },
  { "en": "Apa Itu Single Mode Fiber G.655?", "id": "Serat Optik, Dispersi Bergeser Non-Nol." },
  { "en": "Apa Itu Fiber Bending Loss?", "id": "Rugi Daya, Akibat Tekukan Kabel." },
  { "en": "Apa Itu Macrobending Fiber?", "id": "Tekukan Besar, Terlihat Mata." },
  { "en": "Apa Itu Microbending Fiber?", "id": "Tekukan Mikroskopis, Pada Inti Fiber." },
  { "en": "Apa Itu Dispersion Compensating Fiber?", "id": "Kabel Khusus, Memperbaiki Cacat Sinyal." },
  { "en": "Apa Itu Erbium Doped Fiber Amplifier?", "id": "Penguat Sinyal Optik, Berbasis Erbium." },
  { "en": "Apa Panjang Gelombang EDFA?", "id": "Sekitar 1550, Nanometer." },
  { "en": "Apa Itu Raman Amplifier?", "id": "Penguat Optik, Memanfaatkan Efek Raman." },
  { "en": "Apa Itu Optical Time Domain Reflectometer?", "id": "Alat Ukur, Jarak Gangguan Fiber." },
  { "en": "Apa Itu Dead Zone OTDR?", "id": "Jarak Buta, Awal Pengukuran." },
  { "en": "Apa Itu Fusion Splicing?", "id": "Penyambungan Fiber, Dengan Peleburan Panas." },
  { "en": "Apa Itu Mechanical Splicing?", "id": "Penyambungan Fiber, Dengan Klem Mekanik." },
  { "en": "Apa Itu Cleaver Fiber Optik?", "id": "Alat Pemotong, Ujung Kaca Fiber." },
  { "en": "Apa Itu Stripper Fiber Optik?", "id": "Alat Pengupas, Pelindung Kabel Fiber." },
  { "en": "Apa Itu Visual Fault Locator?", "id": "Laser Merah, Deteksi Putus Visual." },
  { "en": "Apa Itu Power Meter Optik?", "id": "Alat Ukur, Kekuatan Cahaya Penerima." },
  { "en": "Apa Itu Light Source Optik?", "id": "Sumber Cahaya Stabil, Untuk Pengujian." },
  { "en": "Apa Rumus Indeks Bias n?", "id": "n = c / v." },
  { "en": "Apa Sudut Kritis Pemantulan Total?", "id": "Sin θc = n2 / n1." },
  { "en": "Apa Itu Superheterodyne Receiver?", "id": "Penerima Radio, Dengan Frekuensi Antara." },
  { "en": "Apa Itu Homodyne Receiver?", "id": "Penerima Radio, Langsung Ke Baseband." },
  { "en": "Apa Itu Image Frequency Rejection?", "id": "Kemampuan Menolak, Frekuensi Cermin." },
  { "en": "Apa Itu Automatic Frequency Control?", "id": "Menjaga Frekuensi Osilator, Tetap Stabil." },
  { "en": "Apa Itu Squelch Circuit?", "id": "Pemutus Audio, Saat Tidak Ada Sinyal." },
  { "en": "Apa Itu AGC Radio?", "id": "Automatic Gain Control, Penstabil Volume." },
  { "en": "Apa Itu RSSI Indikator?", "id": "Received Signal, Strength Indicator." },
  { "en": "Apa Satuan RSSI?", "id": "dBm." },
  { "en": "Apa Itu Sensitivity Receiver?", "id": "Level Sinyal Minimum, Yang Bisa Diterima." },
  { "en": "Apa Itu Selectivity Receiver?", "id": "Kemampuan Memilah, Frekuensi Yang Diinginkan." },
  { "en": "Apa Itu Fidelity Receiver?", "id": "Ketepatan Reproduksi, Sinyal Asli." },
  { "en": "Apa Itu Intermodulation Product?", "id": "Sinyal Palsu, Hasil Percampuran Frekuensi." },
  { "en": "Apa Itu Spurious Emission?", "id": "Pancaran Gelombang, Yang Tidak Diinginkan." },
  { "en": "Apa Itu Harmonic Emission?", "id": "Pancaran Kelipatan, Frekuensi Kerja." },
  { "en": "Apa Itu Bandwidth 3dB?", "id": "Lebar Pita, Pada Penurunan Daya Setengah." },
  { "en": "Apa Itu Bandwidth 60dB?", "id": "Lebar Pita, Pada Penurunan Sejuta Kali." },
  { "en": "Apa Itu Shape Factor Filter?", "id": "Rasio Bandwidth 60dB, Dan 6dB." },
  { "en": "Apa Itu Quartz Crystal Equivalent Circuit?", "id": "R L C Seri, Paralel C." },
  { "en": "Apa Itu Series Resonant Kristal?", "id": "Impedansi Minimum, Pada Kristal." },
  { "en": "Apa Itu Parallel Resonant Kristal?", "id": "Impedansi Maksimum, Pada Kristal." },
  { "en": "Apa Itu Overtone Crystal?", "id": "Kristal Bekerja, Pada Kelipatan Ganjil." },
  { "en": "Apa Itu PPM Stabilitas Frekuensi?", "id": "Parts Per Million." },
  { "en": "Apa Rumus Pergeseran Frekuensi Hz?", "id": "Δf = f x PPM / 10^6." },
  { "en": "Apa Itu Allan Variance?", "id": "Ukuran Stabilitas Frekuensi, Domain Waktu." },
  { "en": "Apa Itu Phase Noise?", "id": "Fluktuasi Fasa, Sinyal Jangka Pendek." },
  { "en": "Apa Satuan Phase Noise?", "id": "dBc Per Hertz." },
  { "en": "Apa Itu Jitter Clock?", "id": "Penyimpangan Waktu, Tepi Sinyal." },
  { "en": "Apa Itu Rise Time Tr?", "id": "Waktu Naik, 10% Ke 90%." },
  { "en": "Apa Itu Fall Time Tf?", "id": "Waktu Turun, 90% Ke 10%." },
  { "en": "Apa Rumus Bandwidth Rise Time?", "id": "BW = 0.35 / Tr." },
  { "en": "Apa Itu Overshoot Sinyal?", "id": "Lonjakan Sesaat, Melebihi Nilai Stabil." },
  { "en": "Apa Itu Undershoot Sinyal?", "id": "Lonjakan Ke Bawah, Melebihi Target." },
  { "en": "Apa Itu Ringing Sinyal?", "id": "Osilasi Teredam, Setelah Transisi." },
  { "en": "Apa Itu Settling Time?", "id": "Waktu Mencapai, Kestabilan Output." },
  { "en": "Apa Itu Slew Rate Limiting?", "id": "Distorsi Akibat, Op-Amp Terlalu Lambat." },
  { "en": "Apa Rumus Slew Rate SR?", "id": "SR = 2 x π x f x Vp." },
  { "en": "Apa Itu Topologi Flyback Converter?", "id": "Konverter Terisolasi, Berbasis Buck-Boost." },
  { "en": "Apa Fungsi Celah Udara Trafo Flyback?", "id": "Menyimpan Energi, Mencegah Saturasi Inti." },
  { "en": "Apa Itu Mode CCM Flyback?", "id": "Arus Induktor, Tidak Pernah Nol." },
  { "en": "Apa Itu Mode DCM Flyback?", "id": "Arus Induktor, Sempat Mencapai Nol." },
  { "en": "Apa Kelebihan Mode DCM Flyback?", "id": "Tanpa Rugi, Reverse Recovery Dioda." },
  { "en": "Apa Itu Topologi Forward Converter?", "id": "Konverter Terisolasi, Berbasis Buck." },
  { "en": "Apa Fungsi Lilitan Reset Forward?", "id": "Mengembalikan Fluks Magnet, Ke Nol." },
  { "en": "Apa Itu Push-Pull Converter?", "id": "Menggunakan Dua Transistor, Dan Trafo CT." },
  { "en": "Apa Masalah Utama Push-Pull?", "id": "Ketidakseimbangan Fluks, Menyebabkan Saturasi." },
  { "en": "Apa Itu Half-Bridge Converter?", "id": "Dua Transistor, Dengan Pembagi Kapasitor." },
  { "en": "Apa Itu Full-Bridge Converter?", "id": "Empat Transistor, Daya Sangat Besar." },
  { "en": "Apa Itu LLC Resonant Converter?", "id": "Konverter Efisiensi Tinggi, Switching Lunak." },
  { "en": "Apa Itu ZVS (Zero Voltage Switching)?", "id": "Sakelar Hidup, Saat Tegangan Nol." },
  { "en": "Apa Itu ZCS (Zero Current Switching)?", "id": "Sakelar Hidup, Saat Arus Nol." },
  { "en": "Apa Itu Miller Plateau MOSFET?", "id": "Tegangan Gate Datar, Saat Switching." },
  { "en": "Apa Pengaruh Kapasitansi Miller Crss?", "id": "Memperlambat Switching, Meningkatkan Rugi." },
  { "en": "Apa Itu Gate Charge Qg?", "id": "Muatan Total, Untuk Menghidupkan MOSFET." },
  { "en": "Apa Itu Rds(on) MOSFET?", "id": "Hambatan Drain Source, Saat On." },
  { "en": "Apa Hubungan Rds(on) Dan Suhu?", "id": "Hambatan Naik, Jika Suhu Naik." },
  { "en": "Apa Itu Body Diode MOSFET?", "id": "Dioda Parasitik, Antara Source Drain." },
  { "en": "Apa Itu Reverse Recovery Trr?", "id": "Waktu Dioda, Kembali Memblokir Arus." },
  { "en": "Apa Itu Ferrite Core?", "id": "Inti Magnetik, Keramik Oksida Besi." },
  { "en": "Apa Itu Iron Powder Core?", "id": "Inti Serbuk Besi, Saturasi Tinggi." },
  { "en": "Apa Itu Saturation Flux Density Bsat?", "id": "Batas Maksimum, Kerapatan Fluks Magnet." },
  { "en": "Apa Akibat Saturasi Inti Induktor?", "id": "Induktansi Turun, Arus Melonjak Tajam." },
  { "en": "Apa Itu Coercivity Hc?", "id": "Medan Magnet, Untuk Menghilangkan Fluks." },
  { "en": "Apa Itu Remanence Br?", "id": "Sisa Magnet, Saat Medan Hilang." },
  { "en": "Apa Itu Hysteresis Loop?", "id": "Kurva Siklus, Magnetisasi Bahan." },
  { "en": "Apa Itu Litz Wire?", "id": "Kabel Serabut Terisolasi, Frekuensi Tinggi." },
  { "en": "Apa Fungsi Litz Wire?", "id": "Mengurangi Rugi, Akibat Skin Effect." },
  { "en": "Apa Itu Planar Transformer?", "id": "Trafo Pipih, Menggunakan Jalur PCB." },
  { "en": "Apa Kelebihan Planar Transformer?", "id": "Profil Rendah, Dan Konsisten." },
  { "en": "Apa Itu Leakage Inductance?", "id": "Fluks Yang, Tidak Mengopel Sekunder." },
  { "en": "Apa Energi Leakage Inductance?", "id": "Menyebabkan Spike Tegangan, Saat Off." },
  { "en": "Apa Itu Active Clamp Circuit?", "id": "Rangkaian Penjepit, Spike Tegangan Aktif." },
  { "en": "Apa Itu Bootstrap Capacitor?", "id": "Suplai Daya, Driver Sisi Atas." },
  { "en": "Apa Itu High Side Driver?", "id": "Pengendali MOSFET, Yang Mengambang." },
  { "en": "Apa Itu Low Side Driver?", "id": "Pengendali MOSFET, Yang Terhubung Ground." },
  { "en": "Apa Itu Dead Time PWM?", "id": "Jeda Waktu, Antara Dua Sakelar." },
  { "en": "Apa Fungsi Dead Time?", "id": "Mencegah Hubung Singkat, Shoot Through." },
  { "en": "Apa Itu Shoot Through Current?", "id": "Arus Pendek, Melalui Dua Transistor." },
  { "en": "Apa Itu Desaturation Protection?", "id": "Proteksi Arus Lebih, Pada IGBT." },
  { "en": "Apa Itu UVLO (Under Voltage Lockout)?", "id": "Mencegah Operasi, Saat Tegangan Rendah." },
  { "en": "Apa Itu Thermal Shutdown?", "id": "Mematikan Chip, Saat Terlalu Panas." },
  { "en": "Apa Itu Current Mode Control?", "id": "Kendali Berdasarkan, Umpan Balik Arus." },
  { "en": "Apa Itu Voltage Mode Control?", "id": "Kendali Berdasarkan, Umpan Balik Tegangan." },
  { "en": "Apa Itu Slope Compensation?", "id": "Mencegah Osilasi, Sub-Harmonik." },
  { "en": "Apa Itu Right Half Plane Zero?", "id": "Fasa Berubah, Tapi Gain Naik." },
  { "en": "Apa Akibat RHP Zero?", "id": "Sistem Sulit Stabil, Bandwidth Rendah." },
  { "en": "Apa Itu Type II Compensator?", "id": "Kompensator Dengan, Satu Zero Dua Pole." },
  { "en": "Apa Itu Type III Compensator?", "id": "Kompensator Dengan, Dua Zero Tiga Pole." },
  { "en": "Apa Itu Optocoupler CTR?", "id": "Current Transfer Ratio, Rasio Arus." },
  { "en": "Apa Itu TL431?", "id": "Referensi Tegangan Presisi, Yang Dapat Diatur." },
  { "en": "Apa Itu Shunt Regulator?", "id": "Regulator Paralel, Membuang Arus Berlebih." },
  { "en": "Apa Itu Series Regulator?", "id": "Regulator Seri, Mengatur Resistansi Pass." },
  { "en": "Apa Itu Hold Up Time?", "id": "Waktu Output Bertahan, Saat Input Mati." },
  { "en": "Apa Itu Inrush Current NTC?", "id": "Thermistor Pembatas, Arus Awal Masuk." },
  { "en": "Apa Itu X Capacitor?", "id": "Kapasitor Filter, Antar Fasa Netral." },
  { "en": "Apa Itu Y Capacitor?", "id": "Kapasitor Filter, Ke Tanah Ground." },
  { "en": "Apa Syarat Y Capacitor?", "id": "Tidak Boleh Short, Demi Keamanan." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Ganda, Peredam Noise Bersama." },
  { "en": "Apa Itu Differential Mode Choke?", "id": "Induktor Tunggal, Peredam Noise Arus." },
  { "en": "Apa Itu EMI Filter?", "id": "Penyaring Gangguan, Elektromagnetik." },
  { "en": "Apa Standar CISPR 22?", "id": "Standar Emisi Radio, Perangkat IT." },
  { "en": "Apa Itu Conducted Emission?", "id": "Gangguan Yang Merambat, Lewat Kabel." },
  { "en": "Apa Itu Radiated Emission?", "id": "Gangguan Yang Memancar, Lewat Udara." },
  { "en": "Apa Itu LISN (Line Impedance Network)?", "id": "Jaringan Stabilisasi, Impedansi Saluran." },
  { "en": "Apa Fungsi LISN?", "id": "Alat Ukur, Emisi Konduksi." },
  { "en": "Apa Itu Quasi Peak Detector?", "id": "Detektor Puncak, Dengan Bobot Waktu." },
  { "en": "Apa Itu Average Detector?", "id": "Detektor Rata-Rata, Sinyal EMI." },
  { "en": "Apa Itu Shielding Effectiveness?", "id": "Kemampuan Pelindung, Meredam Gelombang." },
  { "en": "Apa Itu Faraday Cage?", "id": "Sangkar Logam, Penahan Medan Listrik." },
  { "en": "Apa Itu Kelvin Connection?", "id": "Sambungan 4 Kabel, Eliminasi Hambatan." },
  { "en": "Apa Itu Burden Voltage?", "id": "Drop Tegangan, Pada Alat Ukur Arus." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Masuk, Ke Terminal Op-Amp." },
  { "en": "Apa Itu Input Offset Current?", "id": "Selisih Arus Bias, Dua Input." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Penguat Terpisah, Secara Galvanis." },
  { "en": "Apa Fungsi Hall Effect Sensor?", "id": "Mengukur Arus, Tanpa Memotong Kabel." },
  { "en": "Apa Itu Shunt Resistor?", "id": "Resistor Presisi, Pengukur Arus." },
  { "en": "Apa Bahan Shunt Resistor?", "id": "Logam Manganin, Atau Constantan." },
  { "en": "Apa Kelebihan Manganin?", "id": "Koefisien Suhu, Sangat Rendah." },
  { "en": "Apa Itu Seebeck Coefficient?", "id": "Tegangan Per Beda Suhu, Termokopel." },
  { "en": "Apa Itu Peltier Effect?", "id": "Pendinginan Elektrik, Arus DC." },
  { "en": "Apa Itu Gauge Factor?", "id": "Sensitivitas Sensor, Strain Gauge." },
  { "en": "Apa Itu Quarter Bridge Strain?", "id": "Jembatan Wheatstone, Satu Sensor Aktif." },
  { "en": "Apa Itu Half Bridge Strain?", "id": "Jembatan Wheatstone, Dua Sensor Aktif." },
  { "en": "Apa Itu Full Bridge Strain?", "id": "Jembatan Wheatstone, Empat Sensor Aktif." },
  { "en": "Apa Itu Load Cell Creep?", "id": "Perubahan Output, Saat Beban Tetap." },
  { "en": "Apa Itu Hysteresis Error?", "id": "Beda Output, Saat Naik Turun." },
  { "en": "Apa Itu Linearity Error?", "id": "Penyimpangan Dari, Garis Lurus Ideal." },
  { "en": "Apa Rumus Skin Depth?", "id": "δ = √(ρ / (π x f x μ))." },
  { "en": "Apa Rumus Energi Kapasitor?", "id": "E = 0.5 x C x V^2." },
  { "en": "Apa Rumus Daya Semu?", "id": "S = V x I." },
  { "en": "Apa Rumus Daya Aktif?", "id": "P = S x Cos φ." },
  { "en": "Apa Rumus Daya Reaktif?", "id": "Q = S x Sin φ." },
  { "en": "Apa Rumus Efisiensi?", "id": "η = (Pout / Pin) x 100%." },
  { "en": "Apa Rumus Tegangan Zener?", "id": "Vz = Vin - (I x R)." },
  { "en": "Apa Itu Protokol LIN Bus?", "id": "Komunikasi Serial, Kecepatan Rendah Otomotif." },
  { "en": "Apa Kecepatan Maksimum LIN Bus?", "id": "20 Kilobit, Per Detik." },
  { "en": "Apa Itu Protokol FlexRay?", "id": "Komunikasi Otomotif, Cepat Dan Deterministik." },
  { "en": "Apa Kelebihan FlexRay Dibanding CAN?", "id": "Bandwidth Lebih Besar, Dan Toleransi Kesalahan." },
  { "en": "Apa Itu Protokol MOST?", "id": "Media Oriented, Systems Transport Multimedia." },
  { "en": "Apa Media Transmisi MOST Bus?", "id": "Serat Optik, Plastik Atau POF." },
  { "en": "Apa Itu SPI Mode 0?", "id": "Clock Polarity 0, Clock Phase 0." },
  { "en": "Apa Itu SPI Mode 1?", "id": "Clock Polarity 0, Clock Phase 1." },
  { "en": "Apa Itu SPI Mode 2?", "id": "Clock Polarity 1, Clock Phase 0." },
  { "en": "Apa Itu SPI Mode 3?", "id": "Clock Polarity 1, Clock Phase 1." },
  { "en": "Apa Itu Clock Stretching I2C?", "id": "Slave Menahan Clock, Agar Master Menunggu." },
  { "en": "Apa Itu Multi Master I2C?", "id": "Banyak Master, Dalam Satu Bus." },
  { "en": "Apa Itu Arbitration I2C?", "id": "Mencegah Tabrakan Data, Antar Master." },
  { "en": "Apa Itu Harmonisa Triplen?", "id": "Harmonisa Kelipatan Tiga, Frekuensi Dasar." },
  { "en": "Apa Dampak Harmonisa Triplen?", "id": "Arus Berlebih, Pada Kabel Netral." },
  { "en": "Apa Itu Interharmonics?", "id": "Frekuensi Bukan, Kelipatan Bulat Dasar." },
  { "en": "Apa Standar IEEE 519?", "id": "Batasan Harmonisa, Sistem Tenaga Listrik." },
  { "en": "Apa Itu Point Of Common Coupling?", "id": "Titik Sambung, Pelanggan Dan Jaringan." },
  { "en": "Apa Itu K-Factor Trafo?", "id": "Rating Trafo, Menangani Arus Harmonisa." },
  { "en": "Apa Itu Total Demand Distortion (TDD)?", "id": "Distorsi Arus, Terhadap Beban Puncak." },
  { "en": "Apa Itu Electron Mobility?", "id": "Kecepatan Elektron, Dalam Medan Listrik." },
  { "en": "Apa Itu Hole Mobility?", "id": "Kecepatan Hole, Dalam Medan Listrik." },
  { "en": "Mana Lebih Cepat Elektron Atau Hole?", "id": "Elektron Lebih Cepat, Daripada Hole." },
  { "en": "Apa Itu Recombination Carrier?", "id": "Elektron Dan Hole, Saling Meniadakan." },
  { "en": "Apa Itu Lifetime Carrier?", "id": "Rata-Rata Waktu, Sebelum Rekombinasi." },
  { "en": "Apa Itu Diffusion Length?", "id": "Jarak Tempuh Carrier, Sebelum Rekombinasi." },
  { "en": "Apa Itu Back EMF Trapezoidal?", "id": "Bentuk Gelombang Balik, Motor BLDC." },
  { "en": "Apa Itu Back EMF Sinusoidal?", "id": "Bentuk Gelombang Balik, Motor PMSM." },
  { "en": "Apa Itu Six Step Commutation?", "id": "Metode Komutasi, Motor BLDC Sederhana." },
  { "en": "Apa Itu Space Vector PWM?", "id": "Teknik Modulasi, Inverter Tiga Fasa." },
  { "en": "Apa Kelebihan Space Vector PWM?", "id": "Tegangan Output, Lebih Tinggi Dan Halus." },
  { "en": "Apa Itu Hall Effect Sensor Motor?", "id": "Deteksi Posisi Rotor, Untuk Komutasi." },
  { "en": "Apa Itu Resolver Motor?", "id": "Sensor Posisi Rotor, Analog Presisi." },
  { "en": "Apa Itu Trench MOSFET?", "id": "Gerbang Vertikal, Menurunkan Hambatan On." },
  { "en": "Apa Itu Super Junction MOSFET?", "id": "Struktur P-N, Tegangan Tinggi Efisien." },
  { "en": "Apa Itu IGBT Punch Through?", "id": "IGBT Dengan Lapisan, Buffer N-Plus." },
  { "en": "Apa Itu IGBT Non Punch Through?", "id": "IGBT Tanpa Lapisan, Buffer Tambahan." },
  { "en": "Apa Itu Reverse Conducting IGBT?", "id": "IGBT Dengan Dioda, Body Terintegrasi." },
  { "en": "Apa Itu Snapback Effect?", "id": "Karakteristik Breakdown, Negatif Pada ESD." },
  { "en": "Apa Itu Fiber Bragg Grating?", "id": "Sensor Regangan, Berbasis Serat Optik." },
  { "en": "Apa Aplikasi Fiber Bragg Grating?", "id": "Monitor Kesehatan Struktur, Jembatan Bendungan." },
  { "en": "Apa Itu Sagnac Effect?", "id": "Prinsip Kerja, Gyroscope Serat Optik." },
  { "en": "Apa Itu Mach-Zehnder Interferometer?", "id": "Modulator Cahaya, Berbasis Interferensi." },
  { "en": "Apa Itu Electro-Optic Effect?", "id": "Indeks Bias Berubah, Kena Medan Listrik." },
  { "en": "Apa Itu Kerr Effect?", "id": "Perubahan Indeks Bias, Kuadrat Medan." },
  { "en": "Apa Itu Pockels Effect?", "id": "Perubahan Indeks Bias, Linear Medan." },
  { "en": "Apa Itu Standard IEC 61131?", "id": "Standar Bahasa, Pemrograman PLC." },
  { "en": "Apa Itu Standard IEC 60076?", "id": "Standar Internasional, Transformator Daya." },
  { "en": "Apa Itu Standard IEC 60228?", "id": "Standar Konduktor, Kabel Terisolasi." },
  { "en": "Apa Itu Standard IEC 60529?", "id": "Standar Kode IP, (Ingress Protection)." },
  { "en": "Apa Itu Standard IEC 61000?", "id": "Standar Kompatibilitas, Elektromagnetik (EMC)." },
  { "en": "Apa Itu Standard IEEE 802.11?", "id": "Standar Komunikasi, Wireless LAN (Wi-Fi)." },
  { "en": "Apa Itu Standard IEEE 802.3?", "id": "Standar Komunikasi, Ethernet Kabel." },
  { "en": "Apa Itu Standard IEEE 802.15.1?", "id": "Standar Komunikasi, Bluetooth WPAN." },
  { "en": "Apa Itu Standard IEEE 802.15.4?", "id": "Standar Komunikasi, ZigBee Dan LR-WPAN." },
  { "en": "Apa Itu Crosstalk NEXT?", "id": "Near End Crosstalk, Gangguan Dekat." },
  { "en": "Apa Itu Crosstalk FEXT?", "id": "Far End Crosstalk, Gangguan Jauh." },
  { "en": "Apa Itu Attenuation To Crosstalk Ratio?", "id": "Rasio Sinyal Sisa, Terhadap Gangguan." },
  { "en": "Apa Itu Power Sum NEXT?", "id": "Total Gangguan, Dari Semua Pasangan." },
  { "en": "Apa Itu Delay Skew Kabel?", "id": "Beda Waktu Tiba, Antar Pasangan Kabel." },
  { "en": "Apa Itu Nominal Velocity Propagation?", "id": "Kecepatan Sinyal, Relatif Cahaya." },
  { "en": "Apa Itu Return Loss Kabel?", "id": "Rugi Pantulan, Akibat Impedansi Beda." },
  { "en": "Apa Itu Structured Cabling?", "id": "Sistem Pengkabelan, Gedung Terstandar." },
  { "en": "Apa Itu Patch Panel?", "id": "Terminal Penghubung, Kabel Jaringan." },
  { "en": "Apa Itu Rack Server Unit (U)?", "id": "Satuan Tinggi, Perangkat Rackmount." },
  { "en": "Apa Tinggi Satu Unit (1U)?", "id": "1.75 Inci, Atau 44.45 Milimeter." },
  { "en": "Apa Itu Hot Swappable?", "id": "Ganti Komponen, Saat Sistem Hidup." },
  { "en": "Apa Itu Redundant Power Supply?", "id": "Dua Power Supply, Saling Backup." },
  { "en": "Apa Itu RAID Controller?", "id": "Pengendali Array, Hard Disk Redundan." },
  { "en": "Apa Itu RAID 0?", "id": "Striping Data, Cepat Tanpa Redundansi." },
  { "en": "Apa Itu RAID 1?", "id": "Mirroring Data, Salinan Identik Aman." },
  { "en": "Apa Itu RAID 5?", "id": "Striping Dengan Parity, Hemat Aman." },
  { "en": "Apa Itu Through Hole Technology?", "id": "Kaki Komponen, Menembus PCB." },
  { "en": "Apa Itu Surface Mount Technology?", "id": "Komponen Ditempel, Di Permukaan PCB." },
  { "en": "Apa Itu Pitch Komponen?", "id": "Jarak Antar Pusat, Kaki Komponen." },
  { "en": "Apa Ukuran Resistor 0603?", "id": "0.06 Kali 0.03 Inci." },
  { "en": "Apa Ukuran Resistor 0402?", "id": "0.04 Kali 0.02 Inci." },
  { "en": "Apa Itu Ball Grid Array (BGA)?", "id": "Kaki Bola Timah, Di Bawah Chip." },
  { "en": "Apa Itu Quad Flat No-leads (QFN)?", "id": "Chip Tanpa Kaki, Pad Di Bawah." },
  { "en": "Apa Itu Small Outline Package (SOP)?", "id": "Kemasan IC, Kaki Sayap Burung." },
  { "en": "Apa Itu Dual Inline Package (DIP)?", "id": "Kemasan IC, Kaki Tusuk Dua Sisi." },
  { "en": "Apa Itu Flip Chip?", "id": "Chip Dibalik, Langsung Ke Substrat." },
  { "en": "Apa Itu COB (Chip On Board)?", "id": "Die Silikon, Ditempel Langsung PCB." },
  { "en": "Apa Itu Wire Bonding Emas?", "id": "Kawat Emas Halus, Koneksi Chip." },
  { "en": "Apa Rumus Reaktansi Induktif 60Hz?", "id": "XL = 377 x L." },
  { "en": "Apa Rumus Reaktansi Kapasitif 60Hz?", "id": "XC = 1 / (377 x C)." },
  { "en": "Apa Rumus Periode 60Hz?", "id": "T = 16.67 Milidetik." },
  { "en": "Apa Rumus Panjang Gelombang 2.4GHz?", "id": "λ = 12.5 Sentimeter." },
  { "en": "Apa Rumus Panjang Gelombang 5GHz?", "id": "λ = 6 Sentimeter." },
  { "en": "Apa Rumus Penguatan Daya dB?", "id": "G = 10 x Log(Pout / Pin)." },
  { "en": "Apa Rumus Penguatan Tegangan dB?", "id": "G = 20 x Log(Vout / Vin)." },
  { "en": "Apa Rumus Tegangan Termal Vt?", "id": "Vt = k x T / q." },
  { "en": "Apa Nilai Tegangan Termal Suhu Ruang?", "id": "Sekitar 26 Milivolt." },
  { "en": "Apa Rumus Resistansi Kawat?", "id": "R = ρ x L / A." },
  { "en": "Apa Itu Efisiensi Kuantum LED?", "id": "Rasio Foton Keluar, Per Elektron Masuk." },
  { "en": "Apa Itu Luminous Efficacy?", "id": "Lumen Cahaya, Per Watt Listrik." },
  { "en": "Apa Itu Color Rendering Index (CRI)?", "id": "Akurasi Warna, Dibanding Cahaya Matahari." },
  { "en": "Apa Itu Correlated Color Temperature (CCT)?", "id": "Warna Cahaya, Dalam Derajat Kelvin." },
  { "en": "Apa Itu VSWR Sempurna?", "id": "Nilai Satu, Banding Satu." },
  { "en": "Apa Itu Return Loss Buruk?", "id": "Nilai Mendekati, Nol Decibel." },
  { "en": "Apa Itu Impedansi Gelombang Bebas?", "id": "377 Ohm, Atau 120 Pi." },
  { "en": "Apa Itu Cutoff Wavelength?", "id": "Panjang Gelombang, Terbesar Yang Lewat." },
  { "en": "Apa Itu Mode Dominan Waveguide?", "id": "Mode Dengan, Frekuensi Cutoff Terendah." },
  { "en": "Apa Itu Microstrip Impedance?", "id": "Tergantung Lebar Jalur, Dan Dielektrik." },
  { "en": "Apa Itu Dielectric Constant Er?", "id": "Permitivitas Relatif, Bahan Isolator." },
  { "en": "Apa Itu Loss Tangent?", "id": "Ukuran Rugi Daya, Pada Dielektrik." },
  { "en": "Apa Itu NEMA Type 1?", "id": "Pelindung Debu, Penggunaan Dalam Ruangan." },
  { "en": "Apa Itu NEMA Type 3R?", "id": "Tahan Hujan, Dan Pembentukan Es." },
  { "en": "Apa Itu NEMA Type 4X?", "id": "Tahan Air Semprot, Dan Korosi." },
  { "en": "Apa Itu NEMA Type 12?", "id": "Tahan Debu Industri, Dan Tetesan." },
  { "en": "Apa Itu Explosion Proof Enclosure?", "id": "Tahan Ledakan, Dari Dalam Panel." },
  { "en": "Apa Itu Intrinsic Safety?", "id": "Membatasi Energi, Mencegah Percikan Api." },
  { "en": "Apa Itu Purged Enclosure?", "id": "Diberi Tekanan Udara, Mencegah Gas." },
  { "en": "Apa Itu Li-Po Battery?", "id": "Lithium Polymer, Casing Plastik Fleksibel." },
  { "en": "Apa Kelebihan Baterai Li-Po?", "id": "Bentuk Fleksibel, Dan Ringan." },
  { "en": "Apa Bahaya Baterai Li-Po?", "id": "Mudah Kembung, Dan Terbakar." },
  { "en": "Apa Itu Memory Effect NiCd?", "id": "Kapasitas Turun, Jika Cas Tanggung." },
  { "en": "Apa Itu Self Discharge NiMH?", "id": "Cepat Habis, Saat Disimpan Lama." },
  { "en": "Apa Itu LSD NiMH?", "id": "Low Self Discharge, Tahan Lama." },
  { "en": "Apa Itu Primary Cell?", "id": "Baterai Sekali Pakai, Tidak Dicas." },
  { "en": "Apa Itu Secondary Cell?", "id": "Baterai Isi Ulang." },
  { "en": "Apa Itu Photolithography?", "id": "Mencetak Pola Sirkuit, Dengan Cahaya." },
  { "en": "Apa Itu Photoresist?", "id": "Bahan Peka Cahaya, Pada Wafer." },
  { "en": "Apa Itu Etching Process?", "id": "Mengikis Material, Yang Tidak Dilindungi." },
  { "en": "Apa Itu Doping Ion Implantation?", "id": "Menembakkan Ion, Ke Dalam Silikon." },
  { "en": "Apa Itu Wafer Yield?", "id": "Persentase Chip Bagus, Per Wafer." },
  { "en": "Apa Itu Clean Room?", "id": "Ruangan Bebas Debu, Pabrik Chip." },
  { "en": "Apa Itu Class 100 Cleanroom?", "id": "Maksimal 100 Partikel, Per Kaki." },
  { "en": "Apa Output XOR Input Sama?", "id": "Nol." },
  { "en": "Apa Output XOR Input Beda?", "id": "Satu." },
  { "en": "Apa Output XNOR Input Sama?", "id": "Satu." },
  { "en": "Apa Output XNOR Input Beda?", "id": "Nol." },
  { "en": "Apa Itu Half Adder?", "id": "Penjumlah Dua Bit, Tanpa Carry." },
  { "en": "Apa Itu Full Adder?", "id": "Penjumlah Tiga Bit, Dengan Carry." },
  { "en": "Apa Itu Ripple Carry Adder?", "id": "Full Adder, Disusun Seri." },
  { "en": "Apa Itu Look Ahead Carry?", "id": "Adder Cepat, Menghitung Carry Paralel." },
  { "en": "Apa Itu Superposition Theorem?", "id": "Analisis Satu Sumber, Bergantian." },
  { "en": "Apa Syarat Teorema Superposisi?", "id": "Rangkaian Linear, Dan Bilateral." },
  { "en": "Apa Itu Reciprocity Theorem?", "id": "Posisi Sumber Dan Ukur, Ditukar." },
  { "en": "Apa Itu Millman's Theorem?", "id": "Paralel Sumber Tegangan, Jadi Satu." },
  { "en": "Apa Itu Maximum Power Transfer?", "id": "Impedansi Beban, Sama Dengan Sumber." },
  { "en": "Apa Efisiensi Saat Daya Maksimum?", "id": "Lima Puluh Persen." },
  { "en": "Apa Itu Commutator Motor DC?", "id": "Pembalik Arah Arus, Pada Rotor." },
  { "en": "Apa Fungsi Sikat Karbon?", "id": "Menghubungkan Arus, Ke Komutator Putar." },
  { "en": "Apa Itu Armature Reaction?", "id": "Distorsi Medan Magnet, Akibat Beban." },
  { "en": "Apa Akibat Armature Reaction?", "id": "Percikan Api, Pada Sikat Komutator." },
  { "en": "Apa Itu Interpole Motor DC?", "id": "Kutub Bantu, Meredam Percikan Api." },
  { "en": "Apa Itu Back EMF Motor?", "id": "GGL Lawan, Sebanding Kecepatan Putar." },
  { "en": "Apa Rumus Kecepatan Motor DC?", "id": "N = (V - IaRa) / (kΦ)." },
  { "en": "Apa Itu Transformer Oil Breakdown?", "id": "Tegangan Tembus, Minyak Trafo." },
  { "en": "Apa Itu Dissolved Gas Analysis?", "id": "Analisis Gas Terlarut, Minyak Trafo." },
  { "en": "Apa Gas Indikasi Arcing?", "id": "Acetylene." },
  { "en": "Apa Gas Indikasi Overheating?", "id": "Ethylene, Dan Ethane." },
  { "en": "Apa Gas Indikasi Partial Discharge?", "id": "Hydrogen." },
  { "en": "Apa Itu Flash Point Minyak?", "id": "Suhu Uap Minyak, Bisa Terbakar." },
  { "en": "Apa Itu Pour Point Minyak?", "id": "Suhu Minyak, Mulai Membeku." },
  { "en": "Apa Itu Arc Flash Boundary?", "id": "Batas Jarak Aman, Tanpa APD." },
  { "en": "Apa Itu Incident Energy?", "id": "Energi Panas, Saat Terjadi Busur." },
  { "en": "Apa Kategori APD Level 1?", "id": "Tahan Energi, 4 Kalori." },
  { "en": "Apa Kategori APD Level 2?", "id": "Tahan Energi, 8 Kalori." },
  { "en": "Apa Kategori APD Level 4?", "id": "Tahan Energi, 40 Kalori." },
  { "en": "Apa Itu Ground Fault Interrupter?", "id": "Pemutus Arus, Saat Bocor Tanah." },
  { "en": "Apa Beda GFCI Dan ELCB?", "id": "Fungsi Sama, Istilah Beda Wilayah." },
  { "en": "Apa Itu Lumen Maintenance?", "id": "Penurunan Cahaya Lampu, Seiring Waktu." },
  { "en": "Apa Itu L70 Lifetime?", "id": "Waktu Hingga Cahaya, Sisa 70%." },
  { "en": "Apa Itu Ballast Factor?", "id": "Rasio Cahaya Ballast, Terhadap Standar." },
  { "en": "Apa Itu Stroboscopic Effect?", "id": "Kedipan Lampu, Membuat Gerakan Diam." },
  { "en": "Apa Itu Unified Glare Rating?", "id": "Ukuran Ketidaknyamanan, Silau Cahaya." },
  { "en": "Apa Itu Beam Angle?", "id": "Sudut Pancaran, Intensitas 50%." },
  { "en": "Apa Itu Feedforward Control?", "id": "Antisipasi Gangguan, Sebelum Kena Output." },
  { "en": "Apa Itu Feedback Control?", "id": "Koreksi Berdasarkan, Kesalahan Output." },
  { "en": "Apa Itu Positive Feedback?", "id": "Memperkuat Input, Menyebabkan Osilasi." },
  { "en": "Apa Itu Negative Feedback?", "id": "Mengurangi Input, Menstabilkan Sistem." },
  { "en": "Apa Itu Gain Margin?", "id": "Batas Penguatan, Sebelum Tidak Stabil." },
  { "en": "Apa Itu Phase Margin?", "id": "Batas Keterlambatan Fasa, Sebelum Osilasi." },
  { "en": "Apa Itu Analog Signal?", "id": "Kontinu Dalam Waktu, Dan Amplitudo." },
  { "en": "Apa Itu Digital Signal?", "id": "Diskrit Dalam Waktu, Dan Nilai." },
  { "en": "Apa Itu Quantization Noise?", "id": "Error Akibat Pembulatan, Nilai Digital." },
  { "en": "Apa Itu Signal To Noise Ratio?", "id": "Perbandingan Daya Sinyal, Dan Noise." },
  { "en": "Apa Satuan SNR?", "id": "Decibel." },
  { "en": "Apa Itu Bandwidth Sinyal?", "id": "Lebar Pita Frekuensi, Yang Ditempati." },
  { "en": "Apa Itu Baseband Transmission?", "id": "Kirim Sinyal Asli, Tanpa Modulasi." },
  { "en": "Apa Itu Broadband Transmission?", "id": "Kirim Sinyal Modulasi, Frekuensi Tinggi." },
  { "en": "Apa Itu Impedansi Speaker?", "id": "Hambatan AC, Coil Speaker." },
  { "en": "Apa Nilai Impedansi Speaker Umum?", "id": "4 Ohm, 8 Ohm, Atau 16 Ohm." },
  { "en": "Apa Itu Sensitivity Speaker?", "id": "Keras Suara, Per 1 Watt." },
  { "en": "Apa Satuan Sensitivity Speaker?", "id": "dB SPL." },
  { "en": "Apa Itu Crossover Network?", "id": "Pemisah Frekuensi, Ke Driver Speaker." },
  { "en": "Apa Itu Crossover Pasif?", "id": "Komponen L C R, Setelah Amplifier." },
  { "en": "Apa Itu Crossover Aktif?", "id": "Sirkuit Elektronik, Sebelum Amplifier." },
  { "en": "Apa Itu Bi-Amping?", "id": "Dua Amplifier, Untuk Satu Speaker." },
  { "en": "Apa Itu Tri-Amping?", "id": "Tiga Amplifier, Untuk Satu Speaker." },
  { "en": "Apa Itu Total Harmonic Distortion?", "id": "Ukuran Cacat Sinyal, Akibat Harmonisa." },
  { "en": "Apa Itu Slew Rate Audio?", "id": "Kecepatan Respon, Perubahan Tegangan." },
  { "en": "Apa Itu Damping Factor Amp?", "id": "Kemampuan Mengontrol, Gerakan Speaker." },
  { "en": "Apa Rumus Damping Factor?", "id": "DF = Z_load / Z_source." },
  { "en": "Apa Itu Headroom Audio?", "id": "Cadangan Daya, Sebelum Clipping." },
  { "en": "Apa Itu Pink Noise?", "id": "Energi Sama, Per Oktaf." },
  { "en": "Apa Itu White Noise?", "id": "Energi Sama, Per Frekuensi." },
  { "en": "Apa Itu Ground Loop Hum?", "id": "Dengung 50Hz, Akibat Beda Ground." },
  { "en": "Apa Itu V2G Pada Mobil Listrik?", "id": "Vehicle To Grid, Transfer Energi Balik." },
  { "en": "Apa Itu V2L Pada Mobil Listrik?", "id": "Vehicle To Load, Sumber Listrik Perangkat." },
  { "en": "Apa Itu V2H Pada Mobil Listrik?", "id": "Vehicle To Home, Cadangan Listrik Rumah." },
  { "en": "Apa Itu Regenerative Braking Efficiency?", "id": "Persentase Energi, Yang Kembali Ke Baterai." },
  { "en": "Apa Itu Wireless Charging EV?", "id": "Pengisian Induktif, Tanpa Kabel Fisik." },
  { "en": "Apa Standar Konektor Type 1?", "id": "Konektor AC, Lima Pin SAE J1772." },
  { "en": "Apa Standar Konektor Type 2?", "id": "Konektor AC, Tujuh Pin IEC 62196." },
  { "en": "Apa Standar Konektor CCS?", "id": "Combined Charging System, AC Dan DC." },
  { "en": "Apa Standar Konektor GB/T?", "id": "Standar Pengisian, Kendaraan Listrik China." },
  { "en": "Apa Itu Range Anxiety?", "id": "Kecemasan Pengemudi, Akan Habis Baterai." },
  { "en": "Apa Itu Bourdon Tube?", "id": "Sensor Tekanan, Mekanik Pipa Lengkung." },
  { "en": "Apa Itu Orifice Plate?", "id": "Plat Berlubang, Pengukur Aliran Fluida." },
  { "en": "Apa Itu Venturi Tube?", "id": "Pipa Menyempit, Pengukur Beda Tekanan." },
  { "en": "Apa Itu Pitot Tube?", "id": "Tabung Pengukur, Kecepatan Aliran Udara." },
  { "en": "Apa Itu Rotameter?", "id": "Tabung Kaca, Pengukur Laju Aliran." },
  { "en": "Apa Itu Ultrasonic Flow Meter?", "id": "Mengukur Aliran, Menggunakan Gelombang Suara." },
  { "en": "Apa Itu Magnetic Flow Meter?", "id": "Mengukur Aliran Cairan, Konduktif Listrik." },
  { "en": "Apa Itu Vortex Flow Meter?", "id": "Mengukur Pusaran, Akibat Halangan Aliran." },
  { "en": "Apa Itu Coriolis Mass Flow Meter?", "id": "Mengukur Massa Fluida, Secara Langsung." },
  { "en": "Apa Itu Level Transmitter Radar?", "id": "Mengukur Ketinggian Tangki, Dengan Gelombang." },
  { "en": "Apa Itu IPC Class 1?", "id": "Standar Elektronik, Umum Konsumen Biasa." },
  { "en": "Apa Itu IPC Class 2?", "id": "Standar Elektronik, Layanan Industri Dedikasi." },
  { "en": "Apa Itu IPC Class 3?", "id": "Standar Elektronik, Kinerja Tinggi Kritis." },
  { "en": "Apa Itu Solder Mask Dam?", "id": "Sekat Tinta, Antara Pad Solder." },
  { "en": "Apa Itu Thermal Relief Pad?", "id": "Pad Terhubung Jalur, Dengan Jari-Jari." },
  { "en": "Apa Fungsi Thermal Relief?", "id": "Memudahkan Solder, Pada Bidang Tembaga." },
  { "en": "Apa Itu Fiducial Mark?", "id": "Titik Referensi Optik, Mesin Assembly." },
  { "en": "Apa Itu Panelized PCB?", "id": "Beberapa PCB, Digabung Satu Papan." },
  { "en": "Apa Itu V-Cut PCB?", "id": "Garis Potong V, Pemisah Panel." },
  { "en": "Apa Itu Breakaway Tab?", "id": "Bagian Pinggir PCB, Yang Dipatahkan." },
  { "en": "Apa Itu Konstanta Boltzmann k?", "id": "1.38 x 10^-23, Joule Per Kelvin." },
  { "en": "Apa Itu Konstanta Planck h?", "id": "6.626 x 10^-34, Joule Detik." },
  { "en": "Apa Itu Massa Elektron?", "id": "9.11 x 10^-31, Kilogram." },
  { "en": "Apa Itu Kecepatan Drift?", "id": "Kecepatan Rata-Rata, Elektron Dalam Kawat." },
  { "en": "Apa Itu Mean Free Path?", "id": "Jarak Rata-Rata, Tumbukan Antar Elektron." },
  { "en": "Apa Itu Efek Photovoltaic?", "id": "Cahaya Menjadi, Tegangan Listrik." },
  { "en": "Apa Itu Efek Photoelectric?", "id": "Cahaya Melepaskan, Elektron Dari Logam." },
  { "en": "Apa Itu Work Function Logam?", "id": "Energi Minimal, Melepas Elektron Permukaan." },
  { "en": "Apa Itu Fermi-Dirac Distribution?", "id": "Probabilitas Elektron, Menempati Tingkat Energi." },
  { "en": "Apa Itu Schrödinger Equation?", "id": "Persamaan Gelombang, Mekanika Kuantum." },
  { "en": "Apa Itu FDMA Access?", "id": "Pembagian Frekuensi, Untuk Banyak Pengguna." },
  { "en": "Apa Itu TDMA Access?", "id": "Pembagian Waktu, Untuk Banyak Pengguna." },
  { "en": "Apa Itu CDMA Access?", "id": "Pembagian Kode, Untuk Banyak Pengguna." },
  { "en": "Apa Itu OFDMA Access?", "id": "Pembagian Frekuensi Ortogonal, Banyak Pengguna." },
  { "en": "Apa Itu GSM Technology?", "id": "Global System, For Mobile Communications." },
  { "en": "Apa Itu HSPA Technology?", "id": "High Speed, Packet Access." },
  { "en": "Apa Itu VoLTE?", "id": "Voice Over, Long Term Evolution." },
  { "en": "Apa Itu Dark Fiber?", "id": "Kabel Optik Terpasang, Belum Digunakan." },
  { "en": "Apa Itu Last Mile Connection?", "id": "Jaringan Akhir, Ke Rumah Pelanggan." },
  { "en": "Apa Itu Backhaul Network?", "id": "Jaringan Penghubung, Inti Dan Tepi." },
  { "en": "Apa Itu Corona Inception Voltage?", "id": "Tegangan Awal, Munculnya Efek Corona." },
  { "en": "Apa Itu Visual Corona?", "id": "Cahaya Ungu, Di Sekitar Konduktor." },
  { "en": "Apa Itu Audible Noise Corona?", "id": "Suara Desis, Akibat Pecahnya Udara." },
  { "en": "Apa Itu Isokeraunic Level?", "id": "Tingkat Kejadian, Badai Petir Tahunan." },
  { "en": "Apa Itu Shield Wire?", "id": "Kawat Tanah Atas, Pelindung Petir." },
  { "en": "Apa Itu Tower Footing Resistance?", "id": "Tahanan Kaki Menara, Ke Tanah." },
  { "en": "Apa Itu Surge Impedance Zc?", "id": "Impedansi Karakteristik, Tanpa Rugi." },
  { "en": "Apa Rumus Surge Impedance?", "id": "Zc = √(L / C)." },
  { "en": "Apa Itu SIL (Surge Impedance Loading)?", "id": "Daya Alami, Saluran Transmisi." },
  { "en": "Apa Itu Ferranti Effect?", "id": "Tegangan Ujung, Lebih Tinggi Dari Sumber." },
  { "en": "Apa Itu Flux Weakening Motor?", "id": "Mengurangi Fluks, Menambah Kecepatan." },
  { "en": "Apa Itu Constant Power Region?", "id": "Area Operasi, Di Atas Kecepatan Dasar." },
  { "en": "Apa Itu Constant Torque Region?", "id": "Area Operasi, Di Bawah Kecepatan Dasar." },
  { "en": "Apa Itu Slip Ring Motor?", "id": "Motor Induksi, Dengan Rotor Belitan." },
  { "en": "Apa Fungsi Rheostat Rotor?", "id": "Menambah Tahanan, Start Motor Slip Ring." },
  { "en": "Apa Itu Brushless DC Motor?", "id": "Motor Magnet Permanen, Komutasi Elektronik." },
  { "en": "Apa Itu Switched Reluctance Motor?", "id": "Motor Tanpa Magnet, Torsi Reluktansi." },
  { "en": "Apa Kelebihan Motor SRM?", "id": "Konstruksi Sederhana, Tahan Panas Tinggi." },
  { "en": "Apa Itu Stepper Motor Detent?", "id": "Torsi Tahanan, Saat Motor Mati." },
  { "en": "Apa Itu Microstepping Driver?", "id": "Membagi Langkah Motor, Lebih Halus." },
  { "en": "Apa Itu Thermocouple Type E?", "id": "Chromel Dan Constantan, Output Tinggi." },
  { "en": "Apa Itu Thermocouple Type N?", "id": "Nicrosil Dan Nisil, Stabil Suhu Tinggi." },
  { "en": "Apa Itu Thermocouple Type R?", "id": "Platinum Rhodium, Untuk Suhu Sangat Tinggi." },
  { "en": "Apa Itu Thermocouple Type S?", "id": "Platinum Rhodium, Standar Laboratorium." },
  { "en": "Apa Itu Thermowell?", "id": "Selongsong Pelindung, Sensor Suhu." },
  { "en": "Apa Itu Transmitter 4-20mA?", "id": "Mengirim Data, Berupa Arus Standar." },
  { "en": "Apa Kelebihan Sinyal 4-20mA?", "id": "Tahan Noise, Dan Deteksi Kabel Putus." },
  { "en": "Apa Itu Zero Adjustment?", "id": "Mengatur Output, Saat Input Minimum." },
  { "en": "Apa Itu Span Adjustment?", "id": "Mengatur Rentang, Skala Pengukuran." },
  { "en": "Apa Itu Deadband?", "id": "Rentang Input, Tanpa Perubahan Output." },
  { "en": "Apa Itu Hysteresis Loop Magnet?", "id": "Kurva B-H, Siklus Magnetisasi." },
  { "en": "Apa Itu Retentivity Magnet?", "id": "Kemampuan Menyimpan, Kemagnetan Sisa." },
  { "en": "Apa Itu Coercivity Magnet?", "id": "Gaya Untuk, Menghilangkan Kemagnetan." },
  { "en": "Apa Itu Soft Magnetic Material?", "id": "Mudah Dimagnetisasi, Dan Demagnetisasi." },
  { "en": "Apa Itu Hard Magnetic Material?", "id": "Sulit Demagnetisasi, Magnet Permanen." },
  { "en": "Apa Itu Eddy Current Loss?", "id": "Panas Akibat, Arus Pusar Di Inti." },
  { "en": "Apa Rumus Rugi Eddy Current?", "id": "Pe = Ke x f^2 x B^2." },
  { "en": "Apa Itu Lamination Core?", "id": "Plat Besi Tipis, Mengurangi Eddy Current." },
  { "en": "Apa Itu Grain Oriented Steel?", "id": "Baja Silikon, Arah Butiran Teratur." },
  { "en": "Apa Itu Amorphous Core?", "id": "Inti Logam Kaca, Rugi Sangat Rendah." },
  { "en": "Apa Rumus Energi Potensial W?", "id": "W = V x I x t." },
  { "en": "Apa Rumus Muatan Q Kapasitor?", "id": "Q = C x V." },
  { "en": "Apa Rumus Kuat Medan Listrik E?", "id": "E = F / q." },
  { "en": "Apa Rumus Gaya Coulomb?", "id": "F = k x q1 x q2 / r^2." },
  { "en": "Apa Rumus Resistansi Paralel?", "id": "Rp = (R1 x R2) / (R1 + R2)." },
  { "en": "Apa Rumus Pembagi Tegangan?", "id": "Vout = Vin x R2 / (R1 + R2)." },
  { "en": "Apa Rumus Pembagi Arus?", "id": "I1 = Itotal x R2 / (R1 + R2)." },
  { "en": "Apa Rumus Daya Disipasi I2R?", "id": "P = I^2 x R." },
  { "en": "Apa Rumus Daya Disipasi V2/R?", "id": "P = V^2 / R." },
  { "en": "Apa Itu Cut-In Speed Turbin Angin?", "id": "Kecepatan Angin Minimum, Mulai Berputar." },
  { "en": "Apa Itu Cut-Out Speed Turbin?", "id": "Kecepatan Maksimum, Turbin Harus Berhenti." },
  { "en": "Apa Itu Rated Wind Speed?", "id": "Kecepatan Angin, Menghasilkan Daya Nominal." },
  { "en": "Apa Itu Yaw Mechanism Turbin?", "id": "Memutar Turbin, Menghadap Arah Angin." },
  { "en": "Apa Fungsi Gearbox Turbin Angin?", "id": "Meningkatkan Putaran, Poros Generator." },
  { "en": "Apa Itu Direct Drive Turbine?", "id": "Turbin Tanpa Gearbox, Efisiensi Tinggi." },
  { "en": "Apa Itu Doubly Fed Induction Generator?", "id": "Generator Induksi, Rotor Terhubung Inverter." },
  { "en": "Apa Kepanjangan DFIG Turbin Angin?", "id": "Doubly Fed, Induction Generator." },
  { "en": "Apa Itu Betz Limit?", "id": "Efisiensi Maksimum Teoritis, Turbin Angin." },
  { "en": "Berapa Nilai Betz Limit?", "id": "Sekitar 59.3 Persen." },
  { "en": "Apa Rumus Daya Angin P?", "id": "P = 0.5 x ρ x A x v^3." },
  { "en": "Apa Itu Tip Speed Ratio?", "id": "Rasio Kecepatan Ujung, Terhadap Angin." },
  { "en": "Apa Itu Solidity Turbin Angin?", "id": "Rasio Luas Sudu, Terhadap Sapuan." },
  { "en": "Apa Itu Stall Control?", "id": "Mengurangi Lift, Saat Angin Kencang." },
  { "en": "Apa Itu Pitch Control Aktif?", "id": "Mengubah Sudut Sudu, Secara Mekanis." },
  { "en": "Apa Itu Anemometer Cup?", "id": "Sensor Kecepatan Angin, Tipe Mangkok." },
  { "en": "Apa Itu Wind Vane?", "id": "Sensor Arah Angin, Sirip Ekor." },
  { "en": "Apa Itu Nacelle Turbin Angin?", "id": "Rumah Mesin, Di Atas Menara." },
  { "en": "Apa Itu Offshore Wind Farm?", "id": "Ladang Angin, Di Lepas Pantai." },
  { "en": "Apa Itu Onshore Wind Farm?", "id": "Ladang Angin, Di Daratan." },
  { "en": "Apa Itu HVDC Monopolar Link?", "id": "Satu Konduktor, Kembali Lewat Tanah." },
  { "en": "Apa Itu HVDC Bipolar Link?", "id": "Dua Konduktor, Positif Dan Negatif." },
  { "en": "Apa Itu HVDC Back-To-Back?", "id": "Penyearah Dan Inverter, Satu Lokasi." },
  { "en": "Apa Fungsi HVDC Back-To-Back?", "id": "Menghubungkan Grid, Beda Frekuensi." },
  { "en": "Apa Itu Commutation Failure HVDC?", "id": "Gagal Pindah Arus, Pada Thyristor." },
  { "en": "Apa Itu Voltage Source Converter HVDC?", "id": "Konverter HVDC, Menggunakan IGBT." },
  { "en": "Apa Kelebihan VSC HVDC?", "id": "Kontrol Daya Aktif, Reaktif Terpisah." },
  { "en": "Apa Itu Smoothing Reactor HVDC?", "id": "Meratakan Arus DC, Mengurangi Ripple." },
  { "en": "Apa Itu DC Harmonic Filter?", "id": "Membuang Harmonisa, Di Sisi DC." },
  { "en": "Apa Itu Ground Electrode HVDC?", "id": "Elektroda Tanah, Untuk Arus Balik." },
  { "en": "Apa Itu SR Flip-Flop?", "id": "Set Reset, Flip-Flop Dasar." },
  { "en": "Apa Kondisi Terlarang SR Flip-Flop?", "id": "Input S=1, Dan R=1." },
  { "en": "Apa Itu JK Flip-Flop?", "id": "Penyempurnaan SR, Tanpa Kondisi Terlarang." },
  { "en": "Apa Fungsi Toggle JK Flip-Flop?", "id": "Output Berubah, Setiap Clock Pulse." },
  { "en": "Apa Itu D Flip-Flop?", "id": "Data Flip-Flop, Penunda Satu Bit." },
  { "en": "Apa Itu T Flip-Flop?", "id": "Toggle Flip-Flop, Pembagi Frekuensi." },
  { "en": "Apa Itu Master-Slave Flip-Flop?", "id": "Dua Flip-Flop Seri, Dikendalikan Clock." },
  { "en": "Apa Itu Edge Triggered?", "id": "Picu Pada Tepi, Naik Atau Turun." },
  { "en": "Apa Itu Level Triggered?", "id": "Picu Pada Level, Tegangan Stabil." },
  { "en": "Apa Itu Asynchronous Counter?", "id": "Output Satu, Memicu Clock Berikutnya." },
  { "en": "Apa Itu Synchronous Counter?", "id": "Semua Flip-Flop, Dipicu Clock Bersamaan." },
  { "en": "Apa Itu Shift Register?", "id": "Menyimpan Dan Menggeser, Data Bit." },
  { "en": "Apa Itu SIPO Shift Register?", "id": "Serial In, Parallel Out." },
  { "en": "Apa Itu PISO Shift Register?", "id": "Parallel In, Serial Out." },
  { "en": "Apa Itu Ring Counter?", "id": "Output Terakhir, Masuk Ke Awal." },
  { "en": "Apa Itu Johnson Counter?", "id": "Output Terakhir Invers, Masuk Awal." },
  { "en": "Apa Itu State Diagram FSM?", "id": "Grafik Alur, Perpindahan Status Logika." },
  { "en": "Apa Itu Moore Machine?", "id": "Output Hanya, Bergantung State Saat Ini." },
  { "en": "Apa Itu Mealy Machine?", "id": "Output Bergantung, State Dan Input." },
  { "en": "Apa Itu Breakdown Voltage Isolator?", "id": "Tegangan Tembus, Bahan Isolasi." },
  { "en": "Apa Itu Tracking Isolator?", "id": "Jalur Arus Bocor, Di Permukaan." },
  { "en": "Apa Itu Treeing Isolator?", "id": "Kerusakan Isolasi, Berbentuk Cabang Pohon." },
  { "en": "Apa Itu Partial Discharge?", "id": "Peluahan Listrik, Tidak Tembus Penuh." },
  { "en": "Apa Satuan Partial Discharge?", "id": "Pico Coulomb." },
  { "en": "Apa Itu Dielectric Strength?", "id": "Kuat Medan Maksimum, Tanpa Tembus." },
  { "en": "Apa Satuan Dielectric Strength?", "id": "Kilovolt, Per Milimeter." },
  { "en": "Apa Bahan Isolator Keramik?", "id": "Porcelain, Atau Kaca Tempered." },
  { "en": "Apa Bahan Isolator Polimer?", "id": "Karet Silikon, Atau EPDM." },
  { "en": "Apa Kelebihan Isolator Polimer?", "id": "Ringan, Dan Tahan Polusi." },
  { "en": "Apa Kekurangan Isolator Polimer?", "id": "Umur Pakai, Lebih Pendek." },
  { "en": "Apa Itu Minyak Trafo Naftenik?", "id": "Minyak Mineral, Titik Tuang Rendah." },
  { "en": "Apa Itu Minyak Trafo Parafinik?", "id": "Minyak Mineral, Titik Nyala Tinggi." },
  { "en": "Apa Itu Gas SF6?", "id": "Sulfur Hexafluoride, Gas Elektronegatif." },
  { "en": "Apa Kelebihan Gas SF6?", "id": "Isolasi Dan Pemadam, Busur Sangat Baik." },
  { "en": "Apa Itu Vacuum Circuit Breaker?", "id": "Pemutus Arus, Dalam Ruang Hampa." },
  { "en": "Apa Itu Contact Resistance?", "id": "Tahanan Kontak, Sambungan Listrik." },
  { "en": "Apa Alat Ukur Contact Resistance?", "id": "Micro-Ohm Meter, Atau Ducter." },
  { "en": "Apa Itu Thermography Test?", "id": "Deteksi Panas, Sambungan Longgar." },
  { "en": "Apa Itu Superkonduktor Tipe I?", "id": "Menolak Medan Magnet, Sepenuhnya." },
  { "en": "Apa Itu Superkonduktor Tipe II?", "id": "Mengizinkan Medan Magnet, Menembus Sebagian." },
  { "en": "Apa Itu Efek Meissner?", "id": "Levitasi Magnet, Di Atas Superkonduktor." },
  { "en": "Apa Suhu Kritis Tc?", "id": "Suhu Bahan, Menjadi Superkonduktor." },
  { "en": "Apa Itu Critical Current Density?", "id": "Arus Maksimum, Superkonduktor Tanpa Hambatan." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter RLC, Tanpa Penguat." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Op-Amp, Dengan Penguatan." },
  { "en": "Apa Itu Butterworth Filter?", "id": "Respon Frekuensi, Paling Datar." },
  { "en": "Apa Itu Chebyshev Filter?", "id": "Cut-Off Tajam, Ada Ripple." },
  { "en": "Apa Itu Bessel Filter?", "id": "Respon Fasa, Paling Linear." },
  { "en": "Apa Itu Elliptic Filter?", "id": "Cut-Off Paling Tajam, Ripple Keduanya." },
  { "en": "Apa Itu Notch Filter?", "id": "Membuang Satu, Frekuensi Spesifik." },
  { "en": "Apa Itu All-Pass Filter?", "id": "Mengubah Fasa, Tanpa Mengubah Amplitudo." },
  { "en": "Apa Itu Bode Plot?", "id": "Grafik Respon, Frekuensi Dan Fasa." },
  { "en": "Apa Itu Decibel Per Decade?", "id": "Kemiringan Grafik, Respon Frekuensi." },
  { "en": "Apa Itu Roll-Off Rate?", "id": "Kecepatan Penurunan, Gain Filter." },
  { "en": "Apa Itu Orde Filter?", "id": "Jumlah Komponen, Penyimpan Energi." },
  { "en": "Apa Itu Sallen-Key Topology?", "id": "Topologi Filter Aktif, Populer." },
  { "en": "Apa Itu Multiple Feedback Topology?", "id": "Topologi Filter Aktif, Stabil." },
  { "en": "Apa Rumus Tegangan Kapasitor Pengisian?", "id": "Vc = Vs(1 - e^(-t/RC))." },
  { "en": "Apa Rumus Tegangan Kapasitor Pengosongan?", "id": "Vc = Vo x e^(-t/RC)." },
  { "en": "Apa Rumus Energi Induktor?", "id": "E = 0.5 x L x I^2." },
  { "en": "Apa Rumus Frekuensi Sudut?", "id": "ω = 2 x π x f." },
  { "en": "Apa Rumus Reaktansi Total Seri?", "id": "X = XL - XC." },
  { "en": "Apa Rumus Impedansi RLC Seri?", "id": "Z = √(R^2 + X^2)." },
  { "en": "Apa Rumus Faktor Daya?", "id": "PF = R / Z." },
  { "en": "Apa Rumus Daya Semu?", "id": "S = √(P^2 + Q^2)." },
  { "en": "Apa Rumus Arus Efektif?", "id": "Irms = Ipeak / √2." },
  { "en": "Apa Rumus Tegangan Puncak?", "id": "Vp = Vrms x √2." },
  { "en": "Apa Itu Tegangan Ekstra Tinggi?", "id": "Tegangan Di Atas, 275 Kilovolt." },
  { "en": "Apa Fungsi Lightning Arrester?", "id": "Membuang Tegangan Surja, Ke Tanah." },
  { "en": "Apa Itu Counterpoise Grounding?", "id": "Kawat Tanah Horizontal, Kaki Menara." },
  { "en": "Apa Itu GIS Substation?", "id": "Gardu Induk, Isolasi Gas SF6." },
  { "en": "Apa Itu AIS Substation?", "id": "Gardu Induk, Isolasi Udara Terbuka." },
  { "en": "Apa Itu Darlington Pair?", "id": "Dua Transistor, Penguatan Arus Ganda." },
  { "en": "Apa Rumus Penguatan Darlington?", "id": "β_total = β1 x β2." },
  { "en": "Apa Itu Sziklai Pair?", "id": "Pasangan Transistor, NPN Dan PNP." },
  { "en": "Apa Itu Bootstrap Circuit?", "id": "Meningkatkan Impedansi Input, Rangkaian." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Peredam Spike Tegangan, Saat Switching." },
  { "en": "Apa Itu Multiplexer 4 Ke 1?", "id": "Empat Input Data, Satu Output." },
  { "en": "Apa Itu Decoder 3 Ke 8?", "id": "Tiga Input Biner, Delapan Output." },
  { "en": "Apa Itu Priority Encoder?", "id": "Mengkodekan Input, Prioritas Tertinggi." },
  { "en": "Apa Itu Parity Generator?", "id": "Menghasilkan Bit Paritas, Cek Error." },
  { "en": "Apa Itu Barrel Shifter?", "id": "Menggeser Data, Banyak Bit Sekaligus." },
  { "en": "Apa Itu Shaded Pole Motor?", "id": "Motor Induksi Kecil, Kutub Bayangan." },
  { "en": "Apa Arah Putar Shaded Pole?", "id": "Tetap, Tidak Bisa Dibalik." },
  { "en": "Apa Itu Capacitor Start Motor?", "id": "Kapasitor Aktif, Hanya Saat Start." },
  { "en": "Apa Itu Capacitor Run Motor?", "id": "Kapasitor Aktif, Selama Motor Jalan." },
  { "en": "Apa Itu Hall Effect Sensor?", "id": "Deteksi Medan Magnet, Jadi Tegangan." },
  { "en": "Apa Itu LVDT Sensor?", "id": "Sensor Posisi Linear, Induksi Trafo." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Regangan, Perubahan Resistansi Kawat." },
  { "en": "Apa Itu Wheatstone Bridge?", "id": "Rangkaian Ukur, Resistansi Presisi Tinggi." },
  { "en": "Apa Rumus Jembatan Seimbang?", "id": "R1 x R3 = R2 x R4." },
  { "en": "Apa Itu Swing Equation?", "id": "Persamaan Gerak, Rotor Generator Sinkron." },
  { "en": "Apa Itu Critical Clearing Time?", "id": "Waktu Maksimum, Memutus Gangguan Stabil." },
  { "en": "Apa Itu Equal Area Criterion?", "id": "Metode Grafis, Kestabilan Transien." },
  { "en": "Apa Itu Load Flow Analysis?", "id": "Analisis Aliran Daya, Steady State." },
  { "en": "Apa Itu Short Circuit Study?", "id": "Analisis Arus Gangguan, Hubung Singkat." },
  { "en": "Apa Itu Waveguide Cutoff?", "id": "Frekuensi Terendah, Yang Bisa Lewat." },
  { "en": "Apa Itu Skin Depth RF?", "id": "Kedalaman Arus, Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Characteristic Impedance?", "id": "Rasio Tegangan Arus, Gelombang Berjalan." },
  { "en": "Apa Rumus Impedansi Ruang Hampa?", "id": "Z0 = 120 x π, Ohm." },
  { "en": "Apa Itu Antenna Gain dBi?", "id": "Penguatan Relatif, Antena Isotropik." },
  { "en": "Apa Itu Curie Temperature?", "id": "Suhu Hilangnya, Sifat Kemagnetan." },
  { "en": "Apa Itu Piezoelectric Effect?", "id": "Tekanan Mekanik, Menjadi Muatan Listrik." },
  { "en": "Apa Itu Superconductor Tc?", "id": "Suhu Kritis, Hambatan Menjadi Nol." },
  { "en": "Apa Itu Ferroelectric Material?", "id": "Polarisasi Listrik, Spontan Dapat Dibalik." },
  { "en": "Apa Itu Luminous Flux?", "id": "Total Cahaya, Dipancarkan Sumber." },
  { "en": "Apa Satuan Luminous Flux?", "id": "Lumen." },
  { "en": "Apa Itu Illuminance?", "id": "Cahaya Jatuh, Pada Permukaan." },
  { "en": "Apa Satuan Illuminance?", "id": "Lux, Atau Lumen Per M^2." },
  { "en": "Apa Itu Luminous Efficacy?", "id": "Efisiensi Cahaya, Lumen Per Watt." },
  { "en": "Apa Itu PID Controller?", "id": "Proportional, Integral, Derivative Control." },
  { "en": "Apa Fungsi Bagian Integral?", "id": "Menghilangkan Error, Steady State." },
  { "en": "Apa Fungsi Bagian Derivative?", "id": "Meredam Overshoot, Dan Antisipasi Error." },
  { "en": "Apa Itu Root Locus?", "id": "Tempat Kedudukan, Akar Persamaan Karakteristik." },
  { "en": "Apa Itu Depth Of Discharge?", "id": "Persentase Kapasitas, Yang Telah Terpakai." },
  { "en": "Apa Itu State Of Charge?", "id": "Persentase Sisa, Kapasitas Baterai." },
  { "en": "Apa Itu C-Rate Charging?", "id": "Laju Arus, Relatif Kapasitas Baterai." },
  { "en": "Apa Itu Trickle Charging?", "id": "Pengisian Arus Kecil, Jaga Penuh." },
  { "en": "Apa Itu Float Voltage?", "id": "Tegangan Jaga, Baterai Lead Acid." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung, Tembaga Dari Oksidasi." },
  { "en": "Apa Itu Silkscreen PCB?", "id": "Cetakan Teks, Dan Simbol Komponen." },
  { "en": "Apa Itu Surface Mount Device?", "id": "Komponen Tempel, Tanpa Kaki Tembus." },
  { "en": "Apa Itu Through Hole Component?", "id": "Komponen Kaki, Menembus Lubang PCB." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Pemanasan Pasta Timah, Dalam Oven." },
  { "en": "Apa Rumus Energi Listrik?", "id": "W = V x I x t." },
  { "en": "Apa Rumus Daya Mekanik HP?", "id": "1 HP = 746 Watt." },
  { "en": "Apa Rumus Muatan Q?", "id": "Q = I x t." },
  { "en": "Apa Rumus Konstanta Waktu RC?", "id": "τ = R x C." },
  { "en": "Apa Rumus Konstanta Waktu RL?", "id": "τ = L / R." },
  { "en": "Apa Itu Norton Theorem?", "id": "Sumber Arus, Paralel Dengan Resistor." },
  { "en": "Apa Itu Thevenin Theorem?", "id": "Sumber Tegangan, Seri Dengan Resistor." },
  { "en": "Apa Itu Mesh Analysis?", "id": "Analisis Arus Loop, Hukum Kirchhoff." },
  { "en": "Apa Itu Nodal Analysis?", "id": "Analisis Tegangan Titik, Hukum Kirchhoff." },
  { "en": "Apa Hukum Kirchoff Arus?", "id": "Jumlah Arus Masuk, Sama Keluar." },
  { "en": "Apa Hukum Kirchoff Tegangan?", "id": "Jumlah Tegangan Loop, Adalah Nol." },
  { "en": "Apa Itu Power Factor Unity?", "id": "Arus Dan Tegangan, Satu Fasa." },
  { "en": "Apa Itu Lagging Power Factor?", "id": "Arus Tertinggal, Dari Tegangan." },
  { "en": "Apa Itu Leading Power Factor?", "id": "Arus Mendahului, Tegangan." },
  { "en": "Apa Beban Bersifat Lagging?", "id": "Induktor, Atau Motor Listrik." },
  { "en": "Apa Beban Bersifat Leading?", "id": "Kapasitor." },
  { "en": "Apa Itu Mutual Inductance?", "id": "Induksi Silang, Antar Dua Kumparan." },
  { "en": "Apa Satuan Mutual Inductance?", "id": "Henry." },
  { "en": "Apa Itu Coupling Coefficient?", "id": "Efisiensi Gandengan, Fluks Magnetik." },
  { "en": "Apa Rumus Coupling Coefficient k?", "id": "k = M / √(L1 x L2)." },
  { "en": "Apa Itu Isolation Transformer?", "id": "Rasio Lilitan, Satu Banding Satu." },
  { "en": "Apa Fungsi Trafo Isolasi?", "id": "Memisahkan Ground, Mencegah Sengatan Listrik." },
  { "en": "Apa Itu Autotransformer?", "id": "Satu Lilitan, Berfungsi Primer Sekunder." },
  { "en": "Apa Keuntungan Autotransformer?", "id": "Ukuran Kecil, Efisiensi Tinggi." },
  { "en": "Apa Bahaya Autotransformer?", "id": "Tidak Ada Isolasi, Galvanis." },
  { "en": "Apa Itu Step Voltage Regulator?", "id": "Autotransformer Dengan, Tap Changer Otomatis." },
  { "en": "Apa Itu Ferranti Effect?", "id": "Tegangan Ujung Terima, Lebih Tinggi." },
  { "en": "Apa Penyebab Ferranti Effect?", "id": "Kapasitansi Saluran, Saat Beban Ringan." },
  { "en": "Apa Solusi Ferranti Effect?", "id": "Memasang Shunt Reactor." },
  { "en": "Apa Itu Bundled Conductors?", "id": "Mengurangi Efek Corona, Transmisi Tinggi." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus Mengalir, Di Kulit Konduktor." },
  { "en": "Apa Rumus Kedalaman Kulit?", "id": "Berbanding Terbalik, Akar Frekuensi." },
  { "en": "Apa Itu Proximity Effect?", "id": "Distribusi Arus, Terganggu Konduktor Dekat." },
  { "en": "Apa Itu Transposition Tower?", "id": "Menukar Posisi Fasa, Kabel Transmisi." },
  { "en": "Apa Tujuan Transposisi?", "id": "Menyeimbangkan Impedansi, Dan Induktansi." },
  { "en": "Apa Itu Sag Pada Kabel?", "id": "Lendutan Kabel, Antara Dua Tiang." },
  { "en": "Apa Faktor Pengaruh Sag?", "id": "Berat Kabel, Suhu, Dan Jarak." },
  { "en": "Apa Itu Span Length?", "id": "Jarak Antara, Dua Tiang Listrik." },
  { "en": "Apa Itu Ground Clearance?", "id": "Jarak Terendah Kabel, Ke Tanah." },
  { "en": "Apa Itu Right Of Way?", "id": "Ruang Bebas, Jalur Transmisi Listrik." },
  { "en": "Apa Itu Jembatan Anderson?", "id": "Mengukur Induktansi, Dengan Presisi Tinggi." },
  { "en": "Apa Itu Jembatan De Sauty?", "id": "Mengukur Kapasitansi, Kapasitor Sempurna." },
  { "en": "Apa Itu Jembatan Wien?", "id": "Mengukur Frekuensi, Dan Kapasitansi." },
  { "en": "Apa Itu Jembatan Kelvin Double?", "id": "Mengukur Resistansi, Sangat Rendah." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Mengukur Resistansi, Nilai Menengah." },
  { "en": "Apa Itu Wagner Earth Device?", "id": "Menghilangkan Efek Kapasitansi, Ke Tanah." },
  { "en": "Apa Itu Q-Meter?", "id": "Alat Ukur, Faktor Kualitas Induktor." },
  { "en": "Apa Itu Megohmmeter?", "id": "Alat Ukur, Tahanan Isolasi Tinggi." },
  { "en": "Apa Itu Luxmeter?", "id": "Alat Ukur, Intensitas Penerangan Cahaya." },
  { "en": "Apa Itu Tachometer?", "id": "Alat Ukur, Kecepatan Putaran Motor." },
  { "en": "Apa Itu Steady State Stability?", "id": "Kemampuan Sistem, Kembali Stabil Perlahan." },
  { "en": "Apa Itu Transient Stability?", "id": "Kemampuan Sistem, Tahan Gangguan Besar." },
  { "en": "Apa Itu Dynamic Stability?", "id": "Kemampuan Meredam, Osilasi Kecil." },
  { "en": "Apa Itu Voltage Collapse?", "id": "Tegangan Jatuh Total, Akibat Q Kurang." },
  { "en": "Apa Itu Frequency Collapse?", "id": "Frekuensi Jatuh, Akibat P Kurang." },
  { "en": "Apa Itu Load Shedding?", "id": "Pelepasan Beban, Saat Darurat." },
  { "en": "Apa Itu Islanding Operation?", "id": "Pembangkit Beroperasi, Terpisah Dari Grid." },
  { "en": "Apa Itu Spin Reserve?", "id": "Cadangan Daya, Pembangkit Yang Berputar." },
  { "en": "Apa Itu Cold Reserve?", "id": "Cadangan Daya, Pembangkit Yang Mati." },
  { "en": "Apa Itu Economic Dispatch?", "id": "Pembagian Beban, Termurah Antar Unit." },
  { "en": "Apa Itu Universal Motor?", "id": "Motor Seri, Bisa AC Dan DC." },
  { "en": "Apa Itu Hysteresis Motor?", "id": "Motor Sinkron, Rotor Baja Keras." },
  { "en": "Apa Itu Reluctance Motor?", "id": "Motor Sinkron, Tanpa Lilitan Rotor." },
  { "en": "Apa Itu Stepper Motor Unipolar?", "id": "Stepper Dengan, Lilitan Center Tap." },
  { "en": "Apa Itu Stepper Motor Bipolar?", "id": "Stepper Dengan, Dua Lilitan Terpisah." },
  { "en": "Apa Itu Servomotor?", "id": "Motor Dengan, Umpan Balik Posisi." },
  { "en": "Apa Itu Linear Motor?", "id": "Motor Gerakan Lurus, Tanpa Roda Gigi." },
  { "en": "Apa Itu Schrage Motor?", "id": "Motor Komutator AC, Kecepatan Variabel." },
  { "en": "Apa Itu Repulsion Motor?", "id": "Motor AC, Sikat Hubung Singkat." },
  { "en": "Apa Itu TE Mode Waveguide?", "id": "Transverse Electric, Ez Sama Dengan Nol." },
  { "en": "Apa Itu TM Mode Waveguide?", "id": "Transverse Magnetic, Hz Sama Dengan Nol." },
  { "en": "Apa Itu TEM Mode?", "id": "Transverse Electromagnetic, Ez Dan Hz Nol." },
  { "en": "Apa Itu Cutoff Frequency?", "id": "Frekuensi Terendah, Yang Bisa Merambat." },
  { "en": "Apa Itu Waveguide Rectangular?", "id": "Pemandu Gelombang, Penampang Persegi Panjang." },
  { "en": "Apa Itu Waveguide Circular?", "id": "Pemandu Gelombang, Penampang Lingkaran." },
  { "en": "Apa Itu Cavity Resonator?", "id": "Kotak Logam, Resonansi Gelombang Mikro." },
  { "en": "Apa Itu Directional Coupler?", "id": "Pemisah Daya, Berdasarkan Arah Rambat." },
  { "en": "Apa Itu Magic Tee?", "id": "Hybrid Tee, Pemandu Gelombang." },
  { "en": "Apa Itu Circulator?", "id": "Mengarahkan Sinyal, Ke Port Urut." },
  { "en": "Apa Itu ALU Processor?", "id": "Unit Aritmatika, Dan Logika." },
  { "en": "Apa Itu Control Unit?", "id": "Pengendali Aliran Data, Dan Instruksi." },
  { "en": "Apa Itu Register Processor?", "id": "Memori Kecil, Kecepatan Sangat Tinggi." },
  { "en": "Apa Itu Cache Memory?", "id": "Memori Buffer, Antara CPU Dan RAM." },
  { "en": "Apa Itu Pipeline Processing?", "id": "Eksekusi Instruksi, Secara Bertahap Paralel." },
  { "en": "Apa Itu RISC Architecture?", "id": "Reduced Instruction, Set Computer." },
  { "en": "Apa Itu CISC Architecture?", "id": "Complex Instruction, Set Computer." },
  { "en": "Apa Itu Interrupt Request?", "id": "Sinyal Minta Layanan, Dari Hardware." },
  { "en": "Apa Itu DMA Controller?", "id": "Transfer Data, Tanpa Lewat CPU." },
  { "en": "Apa Itu Opcode?", "id": "Kode Operasi, Instruksi Mesin." },
  { "en": "Apa Itu Clipper Circuit?", "id": "Pemotong Puncak, Gelombang Sinyal." },
  { "en": "Apa Itu Clamper Circuit?", "id": "Penggeser Level DC, Gelombang Sinyal." },
  { "en": "Apa Itu Voltage Doubler?", "id": "Penyearah Pengganda, Tegangan Output." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan, Histeresis Ganda." },
  { "en": "Apa Itu Sample And Hold?", "id": "Mengambil Nilai, Dan Menahannya Sesaat." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Pembangkit Pulsa, Tanpa Status Stabil." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Pembangkit Pulsa, Satu Status Stabil." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Flip-Flop, Dua Status Stabil." },
  { "en": "Apa Itu Phase Locked Loop?", "id": "Sirkuit Pengunci Fasa, Dan Frekuensi." },
  { "en": "Apa Itu Voltage Controlled Oscillator?", "id": "Osilator Dikendalikan, Tegangan Input." },
  { "en": "Apa Itu Skin Effect?", "id": "Arus AC, Mengalir Di Kulit Kabel." },
  { "en": "Apa Rumus Skin Depth?", "id": "δ = √(2 / (ω x μ x σ))." },
  { "en": "Apa Itu Proximity Effect?", "id": "Arus Terganggu, Konduktor Sebelah." },
  { "en": "Apa Itu Ferranti Effect?", "id": "Tegangan Ujung, Lebih Tinggi." },
  { "en": "Apa Itu Corona Loss?", "id": "Rugi Daya, Akibat Pendaran Listrik." },
  { "en": "Apa Itu Surge Impedance Loading?", "id": "Daya Alami, Saluran Transmisi." },
  { "en": "Apa Itu Characteristic Impedance?", "id": "Impedansi Saluran, Tak Berhingga." },
  { "en": "Apa Rumus Impedansi Surja?", "id": "Zc = √(L / C)." },
  { "en": "Apa Itu Transposition?", "id": "Pertukaran Posisi Fasa, Kabel Transmisi." },
  { "en": "Apa Tujuan Transposisi?", "id": "Menyeimbangkan Impedansi, Antar Fasa." },
  { "en": "Apa Itu Luminous Flux?", "id": "Total Cahaya, Dari Sumber." },
  { "en": "Apa Itu Luminous Intensity?", "id": "Kuat Cahaya, Arah Tertentu." },
  { "en": "Apa Satuan Luminous Intensity?", "id": "Candela." },
  { "en": "Apa Itu Illuminance?", "id": "Kuat Penerangan, Pada Bidang." },
  { "en": "Apa Satuan Illuminance?", "id": "Lux." },
  { "en": "Apa Itu Luminance?", "id": "Kecerahan Permukaan, Yang Terlihat." },
  { "en": "Apa Satuan Luminance?", "id": "Candela, Per Meter Persegi." },
  { "en": "Apa Itu Efficacy Lampu?", "id": "Efisiensi Cahaya, Lumen Per Watt." },
  { "en": "Apa Itu Color Rendering Index?", "id": "Kemampuan Lampu, Menampilkan Warna Asli." },
  { "en": "Apa Itu Open Loop Control?", "id": "Sistem Kendali, Tanpa Umpan Balik." },
  { "en": "Apa Itu Closed Loop Control?", "id": "Sistem Kendali, Dengan Umpan Balik." },
  { "en": "Apa Itu Transfer Function?", "id": "Perbandingan Output, Terhadap Input." },
  { "en": "Apa Itu Pole Sistem?", "id": "Akar Penyebut, Fungsi Transfer." },
  { "en": "Apa Itu Zero Sistem?", "id": "Akar Pembilang, Fungsi Transfer." },
  { "en": "Apa Syarat Sistem Stabil?", "id": "Pole Di Sebelah Kiri, Bidang S." },
  { "en": "Apa Itu Root Locus?", "id": "Jalur Akar, Saat Gain Berubah." },
  { "en": "Apa Itu Bode Plot?", "id": "Grafik Respon Frekuensi, Logaritmik." },
  { "en": "Apa Itu Gain Margin?", "id": "Batas Penguatan, Agar Tetap Stabil." },
  { "en": "Apa Itu Phase Margin?", "id": "Batas Fasa, Agar Tetap Stabil." },
  { "en": "Apa Itu Suhu Isolasi Kelas A?", "id": "Maksimal 105, Derajat Celcius." },
  { "en": "Apa Itu Suhu Isolasi Kelas B?", "id": "Maksimal 130, Derajat Celcius." },
  { "en": "Apa Itu Suhu Isolasi Kelas F?", "id": "Maksimal 155, Derajat Celcius." },
  { "en": "Apa Itu Suhu Isolasi Kelas H?", "id": "Maksimal 180, Derajat Celcius." },
  { "en": "Apa Itu Motor NEMA Design B?", "id": "Torsi Normal, Arus Start Normal." },
  { "en": "Apa Itu Motor NEMA Design D?", "id": "Torsi Start Tinggi, Slip Tinggi." },
  { "en": "Apa Itu Enclosure Motor TEFC?", "id": "Tertutup Total, Berpendingin Kipas Luar." },
  { "en": "Apa Itu Enclosure Motor ODP?", "id": "Terbuka, Tahan Tetesan Air." },
  { "en": "Apa Kode ANSI Device 50N?", "id": "Relay Arus Lebih, Tanah Instan." },
  { "en": "Apa Kode ANSI Device 51N?", "id": "Relay Arus Lebih, Tanah Waktu." },
  { "en": "Apa Kode ANSI Device 67?", "id": "Relay Arus Lebih, Berarah." },
  { "en": "Apa Kode ANSI Device 87T?", "id": "Proteksi Diferensial, Untuk Transformator." },
  { "en": "Apa Kode ANSI Device 87B?", "id": "Proteksi Diferensial, Untuk Busbar." },
  { "en": "Apa Itu Baterai Lithium Titanate (LTO)?", "id": "Umur Panjang, Densitas Energi Rendah." },
  { "en": "Apa Arti C-Rate 1C Baterai?", "id": "Arus Cas, Sama Dengan Kapasitas." },
  { "en": "Apa Arti C-Rate 2C Baterai?", "id": "Arus Cas, Dua Kali Kapasitas." },
  { "en": "Apa Durasi Voltage Sag?", "id": "Setengah Siklus, Hingga Satu Menit." },
  { "en": "Apa Durasi Voltage Swell?", "id": "Setengah Siklus, Hingga Satu Menit." },
  { "en": "Apa Itu Momentary Interruption?", "id": "Padam Singkat, Kurang Dari Semenit." },
  { "en": "Apa Itu Sustained Interruption?", "id": "Padam Lama, Lebih Dari Semenit." },
  { "en": "Apa Itu Rated Insulation Level?", "id": "Ketahanan Dielektrik, Terhadap Tegangan Lebih." },
  { "en": "Apa Itu TRV Circuit Breaker?", "id": "Tegangan Muncul, Setelah Pemutusan Arus." },
  { "en": "Apa Fungsi Resistor Pull-Up I2C?", "id": "Menarik Tegangan Bus, Ke VCC." },
  { "en": "Apa Itu Komunikasi SPI Master-Slave?", "id": "Satu Pengendali, Banyak Perangkat Pengikut." },
  { "en": "Apa Itu UART Parity Error?", "id": "Jumlah Bit Data, Tidak Sesuai." },
  { "en": "Apa Itu Driven Element Yagi?", "id": "Elemen Antena, Yang Terhubung Kabel." },
  { "en": "Apa Itu Reflector Element Yagi?", "id": "Elemen Pemantul, Di Belakang Driven." },
  { "en": "Apa Itu Director Element Yagi?", "id": "Elemen Pengarah, Di Depan Driven." },
  { "en": "Apa Kelebihan Antena Parabola?", "id": "Penguatan Tinggi, Arah Sangat Fokus." },
  { "en": "Apa Itu Feed Horn Antenna?", "id": "Sumber Sinyal, Pada Titik Fokus." },
  { "en": "Apa Rumus Gain Op-Amp Inverting?", "id": "Av = -Rf / Rin." },
  { "en": "Apa Rumus Gain Op-Amp Non-Inverting?", "id": "Av = 1 + (Rf / Rin)." },
  { "en": "Apa Rumus Output Integrator Op-Amp?", "id": "Vout = -1/RC x ∫Vin dt." },
  { "en": "Apa Rumus Output Differentiator Op-Amp?", "id": "Vout = -RC x (dVin / dt)." },
  { "en": "Apa Rumus Output Summing Amplifier?", "id": "Vout = -(V1 + V2 + V3)." },
  { "en": "Apa Rumus Daya Aktif AC?", "id": "P = V x I x Cos φ." },
  { "en": "Apa Rumus Daya Reaktif AC?", "id": "Q = V x I x Sin φ." },
  { "en": "Apa Rumus Daya Semu AC?", "id": "S = V x I." },
  { "en": "Apa Rumus Frekuensi Resonansi LC?", "id": "f0 = 1 / (2π√LC)." },
  { "en": "Apa Rumus Bandwidth RLC?", "id": "BW = f0 / Q." },
  { "en": "Apa Rumus Quality Factor Seri?", "id": "Q = (1/R) x √(L/C)." },
  { "en": "Apa Rumus Quality Factor Paralel?", "id": "Q = R x √(C/L)." },
  { "en": "Apa Rumus Skin Depth?", "id": "δ = √(2ρ / ωμ)." },
  { "en": "Apa Rumus Pembagi Tegangan?", "id": "Vout = Vin x R2 / (R1+R2)." },
  { "en": "Apa Rumus Pembagi Arus?", "id": "I1 = Itotal x R2 / (R1+R2)." },
  { "en": "Apa Syarat Jembatan Wheatstone Seimbang?", "id": "R1/R2 = R3/R4." },
  { "en": "Apa Rumus Transformasi Star Delta?", "id": "Ra = (R1R2 + R2R3 + R3R1) / R1." },
  { "en": "Apa Rumus Transformasi Delta Star?", "id": "R1 = (Rb x Rc) / (Ra+Rb+Rc)." },
  { "en": "Apa Syarat Transfer Daya DC?", "id": "R Beban, Sama Dengan R Thevenin." },
  { "en": "Apa Syarat Transfer Daya AC?", "id": "Z Beban, Konjugat Z Sumber." },
  { "en": "Apa Rumus Tegangan RMS Sinus?", "id": "Vrms = Vpeak / √2." },
  { "en": "Apa Rumus Tegangan Rata-Rata Sinus?", "id": "Vavg = 2 x Vpeak / π." },
  { "en": "Apa Itu Deret Fourier?", "id": "Sinyal Periodik, Jumlah Gelombang Sinus." },
  { "en": "Apa Transformasi Laplace Fungsi Step?", "id": "1 / s." },
  { "en": "Apa Transformasi Laplace Fungsi Ramp?", "id": "1 / s^2." },
  { "en": "Apa Transformasi Laplace Fungsi Impuls?", "id": "1." },
  { "en": "Apa Simbol Unit Delay Z-Transform?", "id": "z^-1." },
  { "en": "Apa Bunyi Teorema Sampling Nyquist?", "id": "fs >= 2 x fmax." },
  { "en": "Apa Itu Efek Aliasing?", "id": "Frekuensi Tinggi, Terbaca Sebagai Rendah." },
  { "en": "Apa Itu Level Kuantisasi ADC?", "id": "Jumlah Nilai Diskrit, Pada ADC." },
  { "en": "Apa Rumus Resolusi Tegangan ADC?", "id": "Vref / (2^n - 1)." },
  { "en": "Apa Itu Rangkaian R-2R Ladder?", "id": "Resistor Tangga, Konversi Digital Analog." },
  { "en": "Apa Kelebihan Flash ADC?", "id": "Konversi Sangat Cepat, Komparator Paralel." },
  { "en": "Apa Itu SAR ADC?", "id": "Successive Approximation, Register ADC." },
  { "en": "Apa Itu Sigma-Delta ADC?", "id": "Oversampling, Noise Shaping Resolusi Tinggi." },
  { "en": "Apa Itu Dual Slope ADC?", "id": "Integrasi Sinyal, Presisi Tapi Lambat." },
  { "en": "Apa Kepanjangan CMRR?", "id": "Common Mode, Rejection Ratio." },
  { "en": "Apa Kepanjangan PSRR?", "id": "Power Supply, Rejection Ratio." },
  { "en": "Apa Itu Slew Rate Op-Amp?", "id": "Kecepatan Maksimum, Perubahan Tegangan Output." },
  { "en": "Apa Itu Input Bias Current?", "id": "Arus Basis, Transistor Input Op-Amp." },
  { "en": "Apa Itu Gain Bandwidth Product?", "id": "Perkalian Gain, Dan Bandwidth Konstan." },
  { "en": "Apa Itu Virtual Ground Op-Amp?", "id": "Titik Tegangan Nol, Semu Op-Amp." },
  { "en": "Apa Itu Histeresis Schmitt Trigger?", "id": "Perbedaan Tegangan, Threshold Atas Bawah." },
  { "en": "Apa Output Timer 555 Astable?", "id": "Gelombang Kotak, Terus Menerus." },
  { "en": "Apa Output Timer 555 Monostable?", "id": "Satu Pulsa Output, Saat Dipicu." },
  { "en": "Apa Output Timer 555 Bistable?", "id": "Flip-Flop, Dua Keadaan Stabil." },
  { "en": "Apa Sifat Osilator Kristal?", "id": "Sangat Stabil, Terhadap Suhu Waktu." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Umpan Balik, Pembagi Tegangan Kapasitif." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Umpan Balik, Pembagi Tegangan Induktif." },
  { "en": "Apa Itu Phase Shift Oscillator?", "id": "Geseran Fasa RC, 180 Derajat." },
  { "en": "Apa Itu Wien Bridge Oscillator?", "id": "Osilator Sinus, Frekuensi Audio Rendah." },
  { "en": "Apa Itu Voltage Controlled Oscillator?", "id": "Frekuensi Berubah, Sesuai Tegangan Input." },
  { "en": "Apa Itu Lock Range PLL?", "id": "Rentang Frekuensi, PLL Tetap Terkunci." },
  { "en": "Apa Itu Capture Range PLL?", "id": "Rentang Frekuensi, PLL Bisa Mengunci." },
  { "en": "Apa Itu Indeks Modulasi FM?", "id": "Rasio Deviasi Frekuensi, Dan Modulasi." },
  { "en": "Apa Itu Indeks Modulasi AM?", "id": "Rasio Amplitudo Sinyal, Dan Carrier." },
  { "en": "Apa Itu Pulse Width Modulation?", "id": "Mengubah Lebar Pulsa, Sinyal Tetap." },
  { "en": "Apa Itu Pulse Code Modulation?", "id": "Representasi Digital, Sinyal Analog Sampling." },
  { "en": "Apa Itu Delta Modulation?", "id": "Mengirim Selisih, Nilai Sinyal Berurutan." },
  { "en": "Apa Itu Time Division Multiplexing?", "id": "Pembagian Waktu, Untuk Banyak Kanal." },
  { "en": "Apa Itu Frequency Division Multiplexing?", "id": "Pembagian Frekuensi, Untuk Banyak Kanal." },
  { "en": "Apa Itu Code Division Multiple Access?", "id": "Kode Unik, Untuk Setiap Pengguna." },
  { "en": "Apa Pita Frekuensi GSM?", "id": "900 MHz, Dan 1800 MHz." },
  { "en": "Apa Pita Frekuensi LTE?", "id": "Beragam, Termasuk 1800 dan 2300 MHz." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memori Program, Dan Data Terpisah." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Memori Program, Dan Data Menyatu." },
  { "en": "Apa Itu Instruksi Pipelining?", "id": "Eksekusi Instruksi, Secara Bertahap Paralel." },
  { "en": "Apa Itu Branch Prediction?", "id": "Menebak Alur Program, Sebelum Dieksekusi." },
  { "en": "Apa Itu Cache Hit Ratio?", "id": "Persentase Data, Ditemukan Di Cache." },
  { "en": "Apa Itu Direct Memory Access?", "id": "Transfer Data Memori, Tanpa Lewat CPU." },
  { "en": "Apa Itu Interrupt Vector Table?", "id": "Daftar Alamat, Layanan Interupsi Program." },
  { "en": "Apa Itu Stack Pointer?", "id": "Register Penunjuk, Alamat Tumpukan Memori." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Reset Sistem, Jika Program Macet." },
  { "en": "Apa Itu Brown Out Reset?", "id": "Reset Otomatis, Saat Tegangan Turun." },
  { "en": "Apa Itu Marx Generator?", "id": "Pembangkit Tegangan, Impuls Petir Bertingkat." },
  { "en": "Apa Itu Cockcroft-Walton Generator?", "id": "Pembangkit Tegangan, Tinggi DC Bertingkat." },
  { "en": "Apa Itu Van De Graaff Generator?", "id": "Pembangkit Statis, Menggunakan Sabuk Berjalan." },
  { "en": "Apa Itu Tesla Coil?", "id": "Trafo Resonansi, Tegangan Tinggi Frekuensi." },
  { "en": "Apa Itu Lichtenberg Figure?", "id": "Pola Cabang, Peluahan Listrik Permukaan." },
  { "en": "Apa Itu Corona Ring?", "id": "Cincin Perata, Distribusi Medan Listrik." },
  { "en": "Apa Itu Grading Capacitor?", "id": "Kapasitor Pembagi, Tegangan Pada Breaker." },
  { "en": "Apa Itu Bushing Trafo?", "id": "Isolator Tembus, Terminal Tegangan Tinggi." },
  { "en": "Apa Itu Faraday Cage?", "id": "Sangkar Logam, Pelindung Medan Listrik." },
  { "en": "Apa Itu Numerical Aperture Fiber?", "id": "Kemampuan Serat, Menerima Cahaya Masuk." },
  { "en": "Apa Rumus Numerical Aperture?", "id": "NA = √(n1^2 - n2^2)." },
  { "en": "Apa Itu Mode Field Diameter?", "id": "Diameter Efektif, Distribusi Cahaya Fiber." },
  { "en": "Apa Itu Cutoff Wavelength Fiber?", "id": "Panjang Gelombang, Batas Single Mode." },
  { "en": "Apa Itu Chromatic Dispersion?", "id": "Penyebaran Pulsa, Akibat Panjang Gelombang." },
  { "en": "Apa Itu Polarization Mode Dispersion?", "id": "Penyebaran Pulsa, Akibat Polarisasi Cahaya." },
  { "en": "Apa Itu Attenuation Coefficient?", "id": "Rugi Daya, Per Kilometer Kabel." },
  { "en": "Apa Itu Splice Loss?", "id": "Rugi Daya, Pada Sambungan Fiber." },
  { "en": "Apa Itu Macrobending Loss?", "id": "Rugi Daya, Akibat Tekukan Kabel." },
  { "en": "Apa Itu OTDR Dead Zone?", "id": "Jarak Buta, Awal Pengukuran Fiber." },
  { "en": "Apa Itu PID Tuning?", "id": "Pengaturan Parameter, Proporsional Integral Derivatif." },
  { "en": "Apa Metode Ziegler-Nichols?", "id": "Metode Tuning PID, Secara Empiris." },
  { "en": "Apa Metode Cohen-Coon?", "id": "Metode Tuning PID, Berbasis Model Proses." },
  { "en": "Apa Itu Setpoint Tracking?", "id": "Respon Output, Mengikuti Nilai Target." },
  { "en": "Apa Itu Disturbance Rejection?", "id": "Kemampuan Menolak, Gangguan Luar Sistem." },
  { "en": "Apa Itu Cascade Control?", "id": "Dua Loop Kendali, Tersusun Seri." },
  { "en": "Apa Itu Feedforward Control?", "id": "Koreksi Gangguan, Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Ratio Control?", "id": "Menjaga Perbandingan, Dua Variabel Proses." },
  { "en": "Apa Itu Split Range Control?", "id": "Satu Output Kendali, Dua Aktuator." },
  { "en": "Apa Itu Override Control?", "id": "Pengambilalihan Kendali, Saat Kondisi Kritis." },
  { "en": "Apa Rumus Daya Aktif 3 Fasa?", "id": "P = √3 x V x I x Cosφ." },
  { "en": "Apa Rumus Daya Semu 3 Fasa?", "id": "S = √3 x V x I." },
  { "en": "Apa Rumus Daya Reaktif 3 Fasa?", "id": "Q = √3 x V x I x Sinφ." },
  { "en": "Apa Rumus Arus Line Star?", "id": "IL = IPh." },
  { "en": "Apa Rumus Tegangan Line Star?", "id": "VL = √3 x VPh." },
  { "en": "Apa Rumus Arus Line Delta?", "id": "IL = √3 x IPh." },
  { "en": "Apa Rumus Tegangan Line Delta?", "id": "VL = VPh." },
  { "en": "Apa Rumus Faktor Daya?", "id": "PF = P / S." },
  { "en": "Apa Rumus Perbaikan Faktor Daya?", "id": "Qc = P x (Tanφ1 - Tanφ2)." },
  { "en": "Apa Rumus Rugi Daya Kabel?", "id": "Ploss = 3 x I^2 x R." },
  { "en": "Apa Itu Grounding Rod?", "id": "Batang Tembaga, Ditanam Ke Tanah." },
  { "en": "Apa Itu Grounding Mesh?", "id": "Jaring Kawat, Ditanam Horizontal." },
  { "en": "Apa Itu Step Voltage?", "id": "Beda Potensial, Antara Dua Kaki." },
  { "en": "Apa Itu Touch Voltage?", "id": "Beda Potensial, Tangan Dan Kaki." },
  { "en": "Apa Itu Earth Tester?", "id": "Alat Ukur, Tahanan Pentanahan." },
  { "en": "Apa Prinsip Earth Tester?", "id": "Metode Jatuh Tegangan, Tiga Titik." },
  { "en": "Apa Itu Soil Resistivity?", "id": "Tahanan Jenis Tanah, Ohm Meter." },
  { "en": "Apa Itu Neutral Grounding Resistor?", "id": "Resistor Pembatas, Arus Gangguan Tanah." },
  { "en": "Apa Itu Solid Grounding?", "id": "Netral Terhubung Langsung, Ke Tanah." },
  { "en": "Apa Itu Floating Neutral?", "id": "Titik Netral, Tidak Ditanahkan." },
  { "en": "Apa Itu Lithium Iron Phosphate?", "id": "Baterai LiFePO4, Aman Dan Awet." },
  { "en": "Apa Itu Lithium Titanate Oxide?", "id": "Baterai LTO, Pengisian Sangat Cepat." },
  { "en": "Apa Itu Nickel Manganese Cobalt?", "id": "Baterai NMC, Densitas Energi Tinggi." },
  { "en": "Apa Itu Battery Management System?", "id": "Sistem Pengaman, Dan Penyeimbang Sel." },
  { "en": "Apa Itu State Of Health?", "id": "Kondisi Kesehatan, Baterai Dibanding Baru." },
  { "en": "Apa Itu Depth Of Discharge?", "id": "Persentase Kapasitas, Yang Digunakan." },
  { "en": "Apa Itu Cycle Life?", "id": "Jumlah Siklus, Hingga Kapasitas Turun." },
  { "en": "Apa Itu Peukert's Law?", "id": "Kapasitas Berkurang, Jika Arus Besar." },
  { "en": "Apa Itu Self Discharge Rate?", "id": "Laju Kehilangan Muatan, Saat Disimpan." },
  { "en": "Apa Itu Thermal Runaway?", "id": "Pemanasan Baterai, Tidak Terkendali." },
  { "en": "Apa Itu Rectifier Diode?", "id": "Dioda Penyearah, Arus Besar." },
  { "en": "Apa Itu Zener Diode?", "id": "Dioda Regulator, Tegangan Mundur." },
  { "en": "Apa Itu Schottky Diode?", "id": "Dioda Switching Cepat, Drop Rendah." },
  { "en": "Apa Itu Varactor Diode?", "id": "Dioda Kapasitor, Variabel Tegangan." },
  { "en": "Apa Itu Tunnel Diode?", "id": "Dioda Resistansi Negatif, Osilator." },
  { "en": "Apa Itu PIN Diode?", "id": "Dioda Sakelar, Frekuensi Radio." },
  { "en": "Apa Itu Photodiode?", "id": "Sensor Cahaya, Arus Mundur." },
  { "en": "Apa Itu Light Emitting Diode?", "id": "Dioda Pemancar Cahaya, Saat Maju." },
  { "en": "Apa Itu Laser Diode?", "id": "Dioda Pemancar, Cahaya Koheren." },
  { "en": "Apa Itu Gunn Diode?", "id": "Dioda Osilator, Gelombang Mikro." },
  { "en": "Apa Itu Bandwidth Oscilloscope?", "id": "Frekuensi Maksimum, Sinyal Terukur Akurat." },
  { "en": "Apa Itu Sampling Rate?", "id": "Jumlah Sampel, Per Detik." },
  { "en": "Apa Itu Rise Time?", "id": "Waktu Sinyal Naik, 10% Ke 90%." },
  { "en": "Apa Itu Trigger Level?", "id": "Tegangan Batas, Memulai Tampilan Gelombang." },
  { "en": "Apa Itu Coupling AC?", "id": "Memblokir DC, Menampilkan Sinyal AC." },
  { "en": "Apa Itu Coupling DC?", "id": "Menampilkan Sinyal, AC Dan DC." },
  { "en": "Apa Itu Probe Attenuation?", "id": "Faktor Pelemahan, Sinyal Masukan Probe." },
  { "en": "Apa Itu FFT Function?", "id": "Analisis Spektrum, Frekuensi Sinyal." },
  { "en": "Apa Itu Lissajous Pattern?", "id": "Grafik X-Y, Beda Fasa Frekuensi." },
  { "en": "Apa Itu Persistence Display?", "id": "Menampilkan Jejak Sinyal, Sebelumnya." },
  { "en": "Apa Rumus Energi Kinetik?", "id": "Ek = 0.5 x m x v^2." },
  { "en": "Apa Rumus Energi Potensial Listrik?", "id": "Ep = q x V." },
  { "en": "Apa Rumus Gaya Lorentz Kawat?", "id": "F = B x I x L x Sinθ." },
  { "en": "Apa Rumus Gaya Lorentz Muatan?", "id": "F = q x v x B x Sinθ." },
  { "en": "Apa Rumus Fluks Magnetik?", "id": "Φ = B x A x Cosθ." },
  { "en": "Apa Rumus GGL Induksi?", "id": "ε = -N x (dΦ / dt)." },
  { "en": "Apa Rumus Impedansi Total?", "id": "Z = √(R^2 + X^2)." },
  { "en": "Apa Rumus Frekuensi Resonansi?", "id": "f = 1 / (2 x π x √LC)." }



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
