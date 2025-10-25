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


  { "en": "Apa Itu Panjang Gelombang (Wavelength) (λ)?", "id": "Jarak Spasial Satu Siklus Gelombang." },
  { "en": "Apa Hubungan Kecepatan (v), Frekuensi (f), Panjang Gelombang (λ)?", "id": "v = f * λ." },
  { "en": "Apa Itu Superposisi (Superposition) Gelombang?", "id": "Penjumlahan Amplitudo Gelombang Bertemu." },
  { "en": "Apa Itu Interferensi Konstruktif (Constructive Interference)?", "id": "Gelombang Sefasa Saling Menguatkan." },
  { "en": "Apa Itu Interferensi Destruktif (Destructive Interference)?", "id": "Gelombang Beda Fasa Saling Melemahkan." },
  { "en": "Apa Itu Gelombang Berdiri (Standing Wave)?", "id": "Hasil Interferensi Gelombang Pantul." },
  { "en": "Apa Itu Simpul (Node) Gelombang Berdiri?", "id": "Titik Amplitudo Nol Getaran." },
  { "en": "Apa Itu Perut (Antinode) Gelombang Berdiri?", "id": "Titik Amplitudo Maksimum Getaran." },
  { "en": "Apa Itu Efek Doppler (Doppler Effect)?", "id": "Perubahan Frekuensi Akibat Gerak Relatif." },
  { "en": "Frekuensi Bertambah Atau Berkurang Jika Sumber Mendekat?", "id": "Frekuensi Terdengar Bertambah Tinggi." },
  { "en": "Apa Itu Gelombang Kejut (Shock Wave)?", "id": "Gelombang Tekanan Akibat Kecepatan Supersonik." },
  { "en": "Apa Itu Bilangan Mach (Mach Number)?", "id": "Rasio Kecepatan Objek, Kecepatan Suara." },
  { "en": "Apa Itu Akustika (Acoustics)?", "id": "Studi Tentang Suara." },
  { "en": "Apa Itu Intensitas (Intensity) Suara?", "id": "Daya Suara Per Satuan Luas." },
  { "en": "Apa Satuan Intensitas (Intensity) Suara?", "id": "Watt Per Meter Persegi (W/m²)." },
  { "en": "Apa Itu Tingkat Intensitas (Intensity Level) Suara?", "id": "Ukuran Logaritmik Intensitas (Desibel)." },
  { "en": "Apa Satuan Tingkat Intensitas (Intensity Level) Suara?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Frekuensi (Frequency) Nada Suara?", "id": "Menentukan Tinggi Rendahnya Nada (Pitch)." },
  { "en": "Apa Itu Amplitudo (Amplitude) Suara?", "id": "Menentukan Keras Lemahnya Suara (Loudness)." },
  { "en": "Apa Itu Timbre (Warna Suara)?", "id": "Kualitas Pembeda Suara (Harmonisa)." },
  { "en": "Apa Itu Resonansi (Resonance) Akustik?", "id": "Penguatan Amplitudo Suara Frekuensi Alami." },
  { "en": "Apa Itu Gema (Echo)?", "id": "Pantulan Suara Terdengar Jelas." },
  { "en": "Apa Itu Gaung (Reverberation)?", "id": "Pantulan Suara Bertahan Ruangan Tertutup." },
  { "en": "Apa Itu Penyerapan (Absorption) Suara?", "id": "Pengurangan Energi Suara Oleh Bahan." },
  { "en": "Apa Itu Insulasi (Insulation) Suara?", "id": "Pencegahan Transmisi Suara Antar Ruang." },
  { "en": "Apa Itu Kimia (Chemistry)?", "id": "Studi Materi Dan Perubahannya." },
  { "en": "Apa Itu Atom (Atom)?", "id": "Unit Dasar Materi Kimia." },
  { "en": "Apa Tiga Partikel Subatomik Utama?", "id": "Proton, Neutron, Elektron." },
  { "en": "Muatan Apa Yang Dimiliki Proton?", "id": "Muatan Positif (+1)." },
  { "en": "Muatan Apa Yang Dimiliki Elektron?", "id": "Muatan Negatif (-1)." },
  { "en": "Muatan Apa Yang Dimiliki Neutron?", "id": "Tidak Bermuatan (Netral)." },
  { "en": "Dimana Proton Dan Neutron Berada?", "id": "Di Dalam Inti Atom (Nukleus)." },
  { "en": "Dimana Elektron Berada?", "id": "Mengorbit Inti Atom (Kulit Elektron)." },
  { "en": "Apa Itu Nomor Atom (Atomic Number) (Z)?", "id": "Jumlah Proton Dalam Inti Atom." },
  { "en": "Apa Itu Nomor Massa (Mass Number) (A)?", "id": "Jumlah Proton Ditambah Neutron." },
  { "en": "Apa Itu Isotop (Isotope)?", "id": "Atom Unsur Sama, Nomor Massa Beda." },
  { "en": "Apa Yang Berbeda Pada Isotop (Isotope)?", "id": "Jumlah Neutron Dalam Inti." },
  { "en": "Apa Itu Ion (Ion)?", "id": "Atom Atau Molekul Bermuatan Listrik." },
  { "en": "Apa Itu Kation (Cation)?", "id": "Ion Bermuatan Positif (Kehilangan Elektron)." },
  { "en": "Apa Itu Anion (Anion)?", "id": "Ion Bermuatan Negatif (Menerima Elektron)." },
  { "en": "Apa Itu Molekul (Molecule)?", "id": "Gabungan Dua Atom Atau Lebih." },
  { "en": "Apa Itu Ikatan Kimia (Chemical Bond)?", "id": "Gaya Tarik Antar Atom Molekul." },
  { "en": "Apa Itu Ikatan Ionik (Ionic Bond)?", "id": "Ikatan Akibat Transfer Elektron (Logam-Nonlogam)." },
  { "en": "Apa Itu Ikatan Kovalen (Covalent Bond)?", "id": "Ikatan Akibat Berbagi Elektron (Nonlogam-Nonlogam)." },
  { "en": "Apa Itu Ikatan Kovalen (Covalent Bond) Polar?", "id": "Berbagi Elektron Tidak Sama Rata." },
  { "en": "Apa Itu Ikatan Kovalen (Covalent Bond) Nonpolar?", "id": "Berbagi Elektron Sama Rata." },
  { "en": "Apa Itu Elektronegativitas (Electronegativity)?", "id": "Kemampuan Atom Menarik Elektron Ikatan." },
  { "en": "Apa Itu Ikatan Logam (Metallic Bond)?", "id": "Ikatan Antar Atom Logam (Lautan Elektron)." },
  { "en": "Apa Itu Ikatan Hidrogen (Hydrogen Bond)?", "id": "Gaya Antarmolekul Libatkan Hidrogen." },
  { "en": "Apa Itu Tabel Periodik (Periodic Table)?", "id": "Susunan Unsur Kimia Berdasarkan Sifat." },
  { "en": "Siapa Pengembang Tabel Periodik (Periodic Table) Modern?", "id": "Dmitri Mendeleev." },
  { "en": "Apa Itu Golongan (Group) Tabel Periodik?", "id": "Kolom Vertikal (Sifat Kimia Mirip)." },
  { "en": "Apa Itu Periode (Period) Tabel Periodik?", "id": "Baris Horizontal (Jumlah Kulit Elektron)." },
  { "en": "Apa Itu Logam Alkali (Alkali Metal)?", "id": "Golongan 1 (Sangat Reaktif)." },
  { "en": "Apa Itu Logam Alkali Tanah (Alkaline Earth Metal)?", "id": "Golongan 2." },
  { "en": "Apa Itu Halogen (Halogen)?", "id": "Golongan 17 (Sangat Reaktif Nonlogam)." },
  { "en": "Apa Itu Gas Mulia (Noble Gas)?", "id": "Golongan 18 (Sangat Stabil, Tidak Reaktif)." },
  { "en": "Apa Itu Mol (Mole)?", "id": "Satuan Jumlah Zat Kimia." },
  { "en": "Berapa Bilangan Avogadro (Avogadro's Number)?", "id": "Sekitar 6.022 x 10²³ Partikel/Mol." },
  { "en": "Apa Itu Massa Molar (Molar Mass)?", "id": "Massa Satu Mol Zat (Gram/Mol)." },
  { "en": "Apa Itu Reaksi Kimia (Chemical Reaction)?", "id": "Proses Perubahan Zat Kimia." },
  { "en": "Apa Itu Reaktan (Reactant)?", "id": "Zat Awal Sebelum Reaksi." },
  { "en": "Apa Itu Produk (Product)?", "id": "Zat Hasil Setelah Reaksi." },
  { "en": "Apa Itu Persamaan Kimia (Chemical Equation)?", "id": "Representasi Simbolis Reaksi Kimia." },
  { "en": "Apa Itu Stoikiometri (Stoichiometry)?", "id": "Perhitungan Kuantitatif Reaktan, Produk." },
  { "en": "Apa Itu Hukum Kekekalan Massa (Conservation of Mass)?", "id": "Massa Total Sebelum, Sesudah Reaksi Sama." },
  { "en": "Apa Itu Reaksi Redoks (Redox Reaction)?", "id": "Reaksi Melibatkan Transfer Elektron." },
  { "en": "Apa Itu Oksidasi (Oxidation)?", "id": "Kehilangan Elektron (Bilangan Oksidasi Naik)." },
  { "en": "Apa Itu Reduksi (Reduction)?", "id": "Penerimaan Elektron (Bilangan Oksidasi Turun)." },
  { "en": "Apa Itu Oksidator (Oxidizing Agent)?", "id": "Zat Penyebab Oksidasi (Mengalami Reduksi)." },
  { "en": "Apa Itu Reduktor (Reducing Agent)?", "id": "Zat Penyebab Reduksi (Mengalami Oksidasi)." },
  { "en": "Apa Itu Bilangan Oksidasi (Oxidation Number)?", "id": "Muatan Hipotetis Atom Senyawa." },
  { "en": "Apa Itu Sel Elektrokimia (Electrochemical Cell)?", "id": "Perangkat Konversi Energi Kimia, Listrik." },
  { "en": "Apa Itu Sel Volta (Voltaic Cell) (Galvanik)?", "id": "Sel Hasilkan Listrik Reaksi Spontan." },
  { "en": "Apa Itu Sel Elektrolisis (Electrolysis Cell)?", "id": "Sel Gunakan Listrik Reaksi Non-Spontan." },
  { "en": "Apa Itu Anoda (Anode) Sel Volta?", "id": "Elektroda Negatif (Tempat Oksidasi)." },
  { "en": "Apa Itu Katoda (Cathode) Sel Volta?", "id": "Elektroda Positif (Tempat Reduksi)." },
  { "en": "Apa Itu Jembatan Garam (Salt Bridge)?", "id": "Menjaga Kenetralan Muatan Sel Volta." },
  { "en": "Apa Itu Potensial Sel (Cell Potential) (Esel)?", "id": "Beda Potensial Antara Dua Elektroda." },
  { "en": "Apa Itu Potensial Reduksi Standar (E°)?", "id": "Potensial Reduksi Setengah Reaksi Standar." },
  { "en": "Apa Itu Persamaan Nernst (Nernst Equation)?", "id": "Hubungan Potensial Sel, Konsentrasi." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Proses Redoks Merusak Logam." },
  { "en": "Bagaimana Pencegahan Korosi (Corrosion)?", "id": "Pengecatan, Pelapisan, Proteksi Katodik." },
  { "en": "Apa Itu Asam (Acid)?", "id": "Zat Hasilkan Ion H+ Larutan Air." },
  { "en": "Apa Itu Basa (Base)?", "id": "Zat Hasilkan Ion OH- Larutan Air." },
  { "en": "Apa Itu Skala pH (pH Scale)?", "id": "Ukuran Keasaman Atau Kebasaan Larutan." },
  { "en": "Berapa Rentang Skala pH (pH Scale)?", "id": "0 Hingga 14." },
  { "en": "pH Berapa Untuk Larutan Asam?", "id": "pH Kurang Dari 7." },
  { "en": "pH Berapa Untuk Larutan Basa?", "id": "pH Lebih Dari 7." },
  { "en": "pH Berapa Untuk Larutan Netral?", "id": "pH Sama Dengan 7." },
  { "en": "Apa Rumus pH (pH Formula)?", "id": "pH = -log[H+]." },
  { "en": "Apa Itu Reaksi Netralisasi (Neutralization Reaction)?", "id": "Reaksi Asam Dan Basa Hasilkan Garam, Air." },
  { "en": "Apa Itu Indikator Asam-Basa (Acid-Base Indicator)?", "id": "Zat Berubah Warna Berdasarkan pH." },
  { "en": "Contoh Indikator Asam-Basa (Acid-Base Indicator)?", "id": "Lakmus, Fenolftalein." },
  { "en": "Apa Itu Titrasi (Titration)?", "id": "Metode Penentuan Konsentrasi Larutan." },
  { "en": "Apa Itu Larutan Buffer (Buffer Solution)?", "id": "Larutan Menahan Perubahan pH." },
  { "en": "Apa Itu Kimia Organik (Organic Chemistry)?", "id": "Studi Senyawa Karbon." },
  { "en": "Apa Itu Hidrokarbon (Hydrocarbon)?", "id": "Senyawa Hanya Mengandung Karbon, Hidrogen." },
  { "en": "Apa Itu Alkana (Alkane)?", "id": "Hidrokarbon Jenuh (Ikatan Tunggal)." },
  { "en": "Apa Itu Alkena (Alkene)?", "id": "Hidrokarbon Ikatan Rangkap Dua." },
  { "en": "Apa Itu Alkuna (Alkyne)?", "id": "Hidrokarbon Ikatan Rangkap Tiga." },
  { "en": "Apa Itu Senyawa Aromatik (Aromatic Compound)?", "id": "Senyawa Cincin Benzena." },
  { "en": "Apa Itu Gugus Fungsi (Functional Group)?", "id": "Atom/Gugus Penentu Sifat Kimia." },
  { "en": "Contoh Gugus Fungsi (Functional Group)?", "id": "Alkohol (-OH), Karboksilat (-COOH)." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Raksasa (Rantai Monomer Berulang)." },
  { "en": "Apa Itu Monomer (Monomer)?", "id": "Unit Berulang Penyusun Polimer." },
  { "en": "Apa Itu Polimerisasi (Polymerization)?", "id": "Proses Pembentukan Polimer Dari Monomer." },
  { "en": "Contoh Polimer (Polymer) Sintetis?", "id": "Plastik (Polietilena), Nilon." },
  { "en": "Contoh Polimer (Polymer) Alami?", "id": "Protein, DNA (Deoxyribonucleic Acid), Selulosa." },
  { "en": "Apa Itu Biokimia (Biochemistry)?", "id": "Studi Kimia Proses Kehidupan." },
  { "en": "Apa Empat Makromolekul Utama Kehidupan?", "id": "Karbohidrat, Lipid, Protein, Asam Nukleat." },
  { "en": "Apa Fungsi Karbohidrat (Carbohydrate)?", "id": "Sumber Energi Utama." },
  { "en": "Apa Fungsi Lipid (Lipid) (Lemak)?", "id": "Penyimpanan Energi, Membran Sel." },
  { "en": "Apa Fungsi Protein (Protein)?", "id": "Struktur Sel, Enzim, Antibodi." },
  { "en": "Apa Monomer (Monomer) Protein?", "id": "Asam Amino (Amino Acid)." },
  { "en": "Apa Fungsi Asam Nukleat (Nucleic Acid)?", "id": "Menyimpan, Mentransmisikan Informasi Genetik." },
  { "en": "Dua Jenis Asam Nukleat (Nucleic Acid)?", "id": "DNA (Deoxyribonucleic Acid), RNA (Ribonucleic Acid)." },
  { "en": "Apa Monomer (Monomer) Asam Nukleat?", "id": "Nukleotida (Nucleotide)." },
  { "en": "Apa Itu Enzim (Enzyme)?", "id": "Katalis Biologis (Mempercepat Reaksi)." },
  { "en": "Apa Itu DNA (Deoxyribonucleic Acid)?", "id": "Molekul Pembawa Materi Genetik." },
  { "en": "Apa Struktur DNA (Deoxyribonucleic Acid)?", "id": "Heliks Ganda (Double Helix)." },
  { "en": "Apa Empat Basa Nitrogen DNA?", "id": "Adenin (A), Guanin (G), Sitosin (C), Timin (T)." },
  { "en": "Pasangan Basa (Base Pairing) DNA?", "id": "A Dengan T, G Dengan C." },
  { "en": "Apa Itu RNA (Ribonucleic Acid)?", "id": "Asam Nukleat Peran Sintesis Protein." },
  { "en": "Apa Perbedaan Utama DNA, RNA?", "id": "Gula (Deoksiribosa Vs Ribosa), Basa (T Vs U)." },
  { "en": "Apa Basa Urasil (Uracil) (U)?", "id": "Basa Nitrogen Menggantikan Timin Di RNA." },
  { "en": "Apa Itu Gen (Gene)?", "id": "Segmen DNA (Deoxyribonucleic Acid) Pengkode Protein." },
  { "en": "Apa Itu Kromosom (Chromosome)?", "id": "Struktur DNA (Deoxyribonucleic Acid) Terkondensasi." },
  { "en": "Apa Itu Genom (Genome)?", "id": "Kumpulan Lengkap Materi Genetik Organisme." },
  { "en": "Apa Itu Ekspresi Gen (Gene Expression)?", "id": "Proses Informasi Gen Menjadi Produk." },
  { "en": "Apa Itu Transkripsi (Transcription)?", "id": "Sintesis RNA (Ribonucleic Acid) Dari DNA." },
  { "en": "Apa Itu Translasi (Translation)?", "id": "Sintesis Protein Dari mRNA (Messenger RNA)." },
  { "en": "Apa Itu mRNA (Messenger RNA)?", "id": "RNA Pembawa Kode Genetik Protein." },
  { "en": "Apa Itu tRNA (Transfer RNA)?", "id": "RNA Pembawa Asam Amino Ribosom." },
  { "en": "Apa Itu rRNA (Ribosomal RNA)?", "id": "RNA Komponen Utama Ribosom." },
  { "en": "Apa Itu Ribosom (Ribosome)?", "id": "Mesin Seluler Sintesis Protein." },
  { "en": "Apa Itu Kodon (Codon)?", "id": "Tiga Basa Nukleotida Kode Asam Amino." },
  { "en": "Apa Itu Kode Genetik (Genetic Code)?", "id": "Aturan Penerjemahan Kodon Asam Amino." },
  { "en": "Apa Itu Mutasi (Mutation)?", "id": "Perubahan Permanen Urutan DNA." },
  { "en": "Apa Itu Metabolisme (Metabolism)?", "id": "Reaksi Kimia Penunjang Kehidupan Sel." },
  { "en": "Apa Itu Katabolisme (Catabolism)?", "id": "Pemecahan Molekul Kompleks Hasilkan Energi." },
  { "en": "Apa Itu Anabolisme (Anabolism)?", "id": "Sintesis Molekul Kompleks Butuh Energi." },
  { "en": "Apa Itu ATP (Adenosine Triphosphate)?", "id": "Molekul Utama Pembawa Energi Sel." },
  { "en": "Apa Itu Respirasi Seluler (Cellular Respiration)?", "id": "Proses Hasilkan ATP Oksidasi Glukosa." },
  { "en": "Apa Itu Glikolisis (Glycolysis)?", "id": "Tahap Awal Pemecahan Glukosa." },
  { "en": "Apa Itu Siklus Krebs (Krebs Cycle)?", "id": "Siklus Reaksi Hasilkan Energi (Mitokondria)." },
  { "en": "Apa Itu Fosforilasi Oksidatif (Oxidative Phosphorylation)?", "id": "Tahap Utama Produksi ATP Oksigen." },
  { "en": "Apa Itu Fotosintesis (Photosynthesis)?", "id": "Proses Ubah Cahaya Jadi Energi Kimia." },
  { "en": "Organisme Apa Melakukan Fotosintesis (Photosynthesis)?", "id": "Tumbuhan, Alga, Beberapa Bakteri." },
  { "en": "Apa Pigmen Utama Fotosintesis (Photosynthesis)?", "id": "Klorofil (Chlorophyll)." },
  { "en": "Dimana Fotosintesis (Photosynthesis) Terjadi Pada Tumbuhan?", "id": "Di Dalam Kloroplas (Chloroplast)." },
  { "en": "Apa Dua Tahap Utama Fotosintesis (Photosynthesis)?", "id": "Reaksi Terang Dan Siklus Calvin." },
  { "en": "Apa Produk Reaksi Terang (Light Reaction)?", "id": "ATP, NADPH (Nicotinamide Adenine Dinucleotide Phosphate), Oksigen." },
  { "en": "Apa Produk Siklus Calvin (Calvin Cycle)?", "id": "Glukosa (Gula)." },
  { "en": "Apa Itu Sel (Cell)?", "id": "Unit Dasar Struktural, Fungsional Kehidupan." },
  { "en": "Apa Itu Sel Prokariotik (Prokaryotic Cell)?", "id": "Sel Tanpa Inti Sejati (Bakteri)." },
  { "en": "Apa Itu Sel Eukariotik (Eukaryotic Cell)?", "id": "Sel Dengan Inti Sejati (Hewan, Tumbuhan)." },
  { "en": "Apa Itu Membran Sel (Cell Membrane)?", "id": "Lapisan Luar Pengatur Lalu Lintas Sel." },
  { "en": "Apa Itu Sitoplasma (Cytoplasm)?", "id": "Isi Sel Di Luar Inti." },
  { "en": "Apa Itu Inti Sel (Nucleus)?", "id": "Organel Pusat Kontrol Sel Eukariotik." },
  { "en": "Apa Itu Mitokondria (Mitochondria)?", "id": "Organel Pembangkit Energi Sel (ATP)." },
  { "en": "Apa Itu Kloroplas (Chloroplast)?", "id": "Organel Tempat Fotosintesis Tumbuhan." },
  { "en": "Apa Itu Ribosom (Ribosome)?", "id": "Tempat Sintesis Protein." },
  { "en": "Apa Itu Retikulum Endoplasma (Endoplasmic Reticulum)?", "id": "Jaringan Membran Sintesis, Transportasi Molekul." },
  { "en": "Apa Itu Aparatus Golgi (Golgi Apparatus)?", "id": "Organel Modifikasi, Pengemasan Protein, Lipid." },
  { "en": "Apa Itu Lisosom (Lysosome)?", "id": "Organel Pencernaan Seluler." },
  { "en": "Apa Itu Vakuola (Vacuole)?", "id": "Kantung Penyimpanan Sel (Besar Tumbuhan)." },
  { "en": "Apa Itu Dinding Sel (Cell Wall)?", "id": "Lapisan Kaku Luar Sel Tumbuhan." },
  { "en": "Apa Fungsi Dinding Sel (Cell Wall)?", "id": "Memberi Bentuk, Perlindungan Sel Tumbuhan." },
  { "en": "Apa Itu Pembelahan Sel (Cell Division)?", "id": "Proses Sel Membelah Diri." },
  { "en": "Apa Itu Mitosis (Mitosis)?", "id": "Pembelahan Sel Hasilkan Sel Identik." },
  { "en": "Apa Itu Meiosis (Meiosis)?", "id": "Pembelahan Sel Hasilkan Sel Kelamin." },
  { "en": "Apa Itu Materi (Matter)?", "id": "Segala Sesuatu Memiliki Massa, Volume." },
  { "en": "Apa Tiga Wujud Materi Umum?", "id": "Padat, Cair, Gas." },
  { "en": "Apa Wujud Materi Keempat?", "id": "Plasma (Gas Terionisasi)." },
  { "en": "Apa Itu Perubahan Fasa (Phase Change)?", "id": "Transisi Antara Wujud Materi." },
  { "en": "Apa Itu Mencair (Melting)?", "id": "Perubahan Padat Menjadi Cair." },
  { "en": "Apa Itu Membeku (Freezing)?", "id": "Perubahan Cair Menjadi Padat." },
  { "en": "Apa Itu Menguap (Evaporation/Boiling)?", "id": "Perubahan Cair Menjadi Gas." },
  { "en": "Apa Itu Mengembun (Condensation)?", "id": "Perubahan Gas Menjadi Cair." },
  { "en": "Apa Itu Menyublim (Sublimation)?", "id": "Perubahan Padat Langsung Gas." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Perubahan Gas Langsung Padat." },
  { "en": "Apa Itu Titik Leleh (Melting Point)?", "id": "Suhu Zat Padat Mencair." },
  { "en": "Apa Itu Titik Beku (Freezing Point)?", "id": "Suhu Zat Cair Membeku." },
  { "en": "Apa Itu Titik Didih (Boiling Point)?", "id": "Suhu Zat Cair Mendidih Tekanan Tertentu." },
  { "en": "Apa Itu Titik Tripel (Triple Point)?", "id": "Suhu, Tekanan Tiga Fasa Koeksis." },
  { "en": "Apa Itu Titik Kritis (Critical Point)?", "id": "Suhu, Tekanan Batas Fasa Cair, Gas." },
  { "en": "Apa Itu Fluida Superkritis (Supercritical Fluid)?", "id": "Zat Di Atas Titik Kritis." },
  { "en": "Apa Itu Unsur (Element)?", "id": "Zat Murni Tidak Bisa Diurai Lagi." },
  { "en": "Apa Itu Senyawa (Compound)?", "id": "Zat Terbentuk Ikatan Kimia Unsur." },
  { "en": "Apa Itu Campuran (Mixture)?", "id": "Gabungan Zat Tanpa Ikatan Kimia." },
  { "en": "Apa Itu Campuran Homogen (Homogeneous Mixture)?", "id": "Campuran Komposisi Seragam (Larutan)." },
  { "en": "Apa Itu Campuran Heterogen (Heterogeneous Mixture)?", "id": "Campuran Komposisi Tidak Seragam." },
  { "en": "Apa Itu Larutan (Solution)?", "id": "Campuran Homogen Zat Terlarut, Pelarut." },
  { "en": "Apa Itu Zat Terlarut (Solute)?", "id": "Zat Terlarut Dalam Pelarut." },
  { "en": "Apa Itu Pelarut (Solvent)?", "id": "Zat Melarutkan Zat Terlarut." },
  { "en": "Apa Pelarut Universal (Universal Solvent)?", "id": "Air (H₂O)." },
  { "en": "Apa Itu Konsentrasi (Concentration) Larutan?", "id": "Jumlah Zat Terlarut Per Volume/Massa." },
  { "en": "Apa Satuan Molaritas (Molarity) (M)?", "id": "Mol Zat Terlarut Per Liter Larutan." },
  { "en": "Apa Satuan Molalitas (Molality) (m)?", "id": "Mol Zat Terlarut Per Kg Pelarut." },
  { "en": "Apa Itu Kelarutan (Solubility)?", "id": "Jumlah Maksimal Zat Terlarut Larutan." },
  { "en": "Apa Itu Larutan Jenuh (Saturated Solution)?", "id": "Larutan Mengandung Zat Terlarut Maksimal." },
  { "en": "Apa Itu Suspensi (Suspension)?", "id": "Campuran Heterogen Partikel Padat Cairan." },
  { "en": "Apa Itu Koloid (Colloid)?", "id": "Campuran Antara Larutan, Suspensi." },
  { "en": "Apa Itu Efek Tyndall (Tyndall Effect)?", "id": "Penghamburan Cahaya Oleh Partikel Koloid." },
  { "en": "Apa Itu Gerak Brown (Brownian Motion)?", "id": "Gerak Acak Partikel Koloid." },
  { "en": "Apa Itu Kinetika Kimia (Chemical Kinetics)?", "id": "Studi Laju Reaksi Kimia." },
  { "en": "Apa Itu Laju Reaksi (Reaction Rate)?", "id": "Perubahan Konsentrasi Reaktan/Produk Waktu." },
  { "en": "Faktor Apa Mempengaruhi Laju Reaksi?", "id": "Konsentrasi, Suhu, Luas Permukaan, Katalis." },
  { "en": "Apa Itu Energi Aktivasi (Activation Energy)?", "id": "Energi Minimum Reaksi Terjadi." },
  { "en": "Apa Itu Katalis (Catalyst)?", "id": "Zat Mempercepat Reaksi Tanpa Habis." },
  { "en": "Bagaimana Katalis (Catalyst) Bekerja?", "id": "Menurunkan Energi Aktivasi Reaksi." },
  { "en": "Apa Itu Enzim (Enzyme)?", "id": "Katalis Biologis (Protein)." },
  { "en": "Apa Itu Kesetimbangan Kimia (Chemical Equilibrium)?", "id": "Laju Reaksi Maju Sama Balik." },
  { "en": "Apa Itu Prinsip Le Chatelier (Le Chatelier's Principle)?", "id": "Sistem Setimbang Merespon Perubahan Kondisi." },
  { "en": "Apa Itu Konstanta Kesetimbangan (Equilibrium Constant) (K)?", "id": "Rasio Konsentrasi Produk, Reaktan Setimbang." },
  { "en": "Apa Itu Termokimia (Thermochemistry)?", "id": "Studi Panas Terlibat Reaksi Kimia." },
  { "en": "Apa Itu Reaksi Eksotermik (Exothermic Reaction)?", "id": "Reaksi Melepaskan Panas Ke Lingkungan." },
  { "en": "Apa Itu Reaksi Endotermik (Endothermic Reaction)?", "id": "Reaksi Menyerap Panas Dari Lingkungan." },
  { "en": "Apa Itu Entalpi (Enthalpy) (H)?", "id": "Ukuran Kandungan Panas Sistem." },
  { "en": "Apa Itu Perubahan Entalpi (ΔH)?", "id": "Panas Diserap/Dilepas Reaksi Tekanan Tetap." },
  { "en": "Tanda ΔH (Perubahan Entalpi) Reaksi Eksotermik?", "id": "Negatif (ΔH < 0)." },
  { "en": "Tanda ΔH (Perubahan Entalpi) Reaksi Endotermik?", "id": "Positif (ΔH > 0)." },
  { "en": "Apa Itu Hukum Hess (Hess's Law)?", "id": "Perubahan Entalpi Total Tidak Bergantung Jalur." },
  { "en": "Apa Itu Kalorimetri (Calorimetry)?", "id": "Pengukuran Panas Reaksi Kimia." },
  { "en": "Apa Itu Kalorimeter (Calorimeter)?", "id": "Alat Pengukur Perubahan Panas." },
  { "en": "Apa Itu Entropi (Entropy) (S)?", "id": "Ukuran Ketidakteraturan Sistem." },
  { "en": "Apa Itu Energi Bebas Gibbs (Gibbs Free Energy) (G)?", "id": "Menentukan Kespontanan Reaksi." },
  { "en": "Apa Kriteria Reaksi Spontan Gibbs?", "id": "Perubahan Energi Bebas Negatif (ΔG < 0)." },
  { "en": "Apa Nama Lain Untuk Hukum Arus Kirchhoff?", "id": "Hukum Titik Cabang (Junction Rule)." },
  { "en": "Apa Nama Lain Untuk Hukum Tegangan Kirchhoff?", "id": "Hukum Loop (Loop Rule)." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Ideal?", "id": "Tegangan Konstan Berapapun Arusnya." },
  { "en": "Apa Itu Sumber Arus (Current Source) Ideal?", "id": "Arus Konstan Berapapun Tegangannya." },
  { "en": "Apa Resistansi Internal Sumber Tegangan Ideal?", "id": "Nol Ohm." },
  { "en": "Apa Resistansi Internal Sumber Arus Ideal?", "id": "Tak Terhingga Ohm." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Praktis?", "id": "Sumber Ideal Seri Resistor Internal." },
  { "en": "Apa Itu Sumber Arus (Current Source) Praktis?", "id": "Sumber Ideal Paralel Resistor Internal." },
  { "en": "Bagaimana Konversi Sumber Tegangan Ke Arus?", "id": "Is = Vs / Rs, Rp = Rs." },
  { "en": "Bagaimana Konversi Sumber Arus Ke Tegangan?", "id": "Vs = Is * Rp, Rs = Rp." },
  { "en": "Apa Syarat Berlaku Teorema Superposisi?", "id": "Rangkaian Harus Bersifat Linear." },
  { "en": "Apa Yang Dimatikan Saat Analisis Superposisi?", "id": "Sumber Independen (Satu Per Satu Aktif)." },
  { "en": "Apa Itu Rangkaian Ekuivalen Thevenin (Thevenin Equivalent)?", "id": "Sumber Tegangan Seri Resistor." },
  { "en": "Apa Itu Rangkaian Ekuivalen Norton (Norton Equivalent)?", "id": "Sumber Arus Paralel Resistor." },
  { "en": "Apa Hubungan Resistansi Thevenin, Norton?", "id": "Rth Sama Dengan Rn." },
  { "en": "Apa Itu Resistansi Beban (Load Resistance)?", "id": "Resistansi Komponen Penerima Daya." },
  { "en": "Kapan Terjadi Transfer Daya Maksimum DC?", "id": "Saat RL Sama Dengan Rth." },
  { "en": "Apa Efisiensi Saat Transfer Daya Maksimum?", "id": "Lima Puluh Persen (50%)." },
  { "en": "Apa Itu Kapasitansi (Capacitance)?", "id": "Kemampuan Menyimpan Muatan Listrik." },
  { "en": "Apa Satuan Kapasitansi (Capacitance)?", "id": "Farad (F)." },
  { "en": "Apa Itu Induktansi (Inductance)?", "id": "Kemampuan Hasilkan GGL Induksi." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Bagaimana Perilaku Kapasitor (Capacitor) Saat Awal DC?", "id": "Seperti Hubungan Singkat (Short Circuit)." },
  { "en": "Bagaimana Perilaku Kapasitor (Capacitor) Setelah Lama DC?", "id": "Seperti Rangkaian Terbuka (Open Circuit)." },
  { "en": "Bagaimana Perilaku Induktor (Inductor) Saat Awal DC?", "id": "Seperti Rangkaian Terbuka (Open Circuit)." },
  { "en": "Bagaimana Perilaku Induktor (Inductor) Setelah Lama DC?", "id": "Seperti Hubungan Singkat (Short Circuit)." },
  { "en": "Apa Rumus Reaktansi Kapasitif (Capacitive Reactance) (Xc)?", "id": "Xc = 1 / (2πfC)." },
  { "en": "Apa Rumus Reaktansi Induktif (Inductive Reactance) (Xl)?", "id": "Xl = 2πfL." },
  { "en": "Bagaimana Reaktansi Kapasitif (Xc) Berubah Frekuensi Naik?", "id": "Xc Akan Menurun." },
  { "en": "Bagaimana Reaktansi Induktif (Xl) Berubah Frekuensi Naik?", "id": "Xl Akan Meningkat." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Satuan Impedansi (Impedance)?", "id": "Ohm (Ω)." },
  { "en": "Bagaimana Impedansi (Impedance) RLC Seri?", "id": "Z = √(R² + (Xl - Xc)²)." },
  { "en": "Apa Itu Sudut Fasa (Phase Angle) (θ)?", "id": "Perbedaan Fasa Tegangan, Arus AC." },
  { "en": "Bagaimana Sudut Fasa (Phase Angle) Rangkaian Resistif?", "id": "Nol Derajat (Sefasa)." },
  { "en": "Bagaimana Sudut Fasa (Phase Angle) Rangkaian Induktif Murni?", "id": "Positif Sembilan Puluh Derajat (+90°)." },
  { "en": "Bagaimana Sudut Fasa (Phase Angle) Rangkaian Kapasitif Murni?", "id": "Negatif Sembilan Puluh Derajat (-90°)." },
  { "en": "Apa Itu Daya Nyata (Real Power) (P)?", "id": "Daya Sebenarnya Digunakan Beban." },
  { "en": "Apa Satuan Daya Nyata (Real Power)?", "id": "Watt (W)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Diperlukan Medan Magnet/Listrik." },
  { "en": "Apa Satuan Daya Reaktif (Reactive Power)?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Apa Itu Daya Semu (Apparent Power) (S)?", "id": "Total Daya Dikirim Sumber." },
  { "en": "Apa Satuan Daya Semu (Apparent Power)?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Hubungan P (Daya Nyata), Q (Daya Reaktif), S (Daya Semu)?", "id": "S² = P² + Q²." },
  { "en": "Apa Itu Faktor Daya (Power Factor) (PF)?", "id": "Cos(θ) = P / S." },
  { "en": "Nilai Faktor Daya (Power Factor) Antara Berapa?", "id": "Antara Nol Hingga Satu." },
  { "en": "Apa Arti Faktor Daya (Power Factor) Satu?", "id": "Beban Murni Resistif, Efisien." },
  { "en": "Apa Arti Faktor Daya (Power Factor) Kurang Satu?", "id": "Ada Komponen Reaktif (Induktif/Kapasitif)." },
  { "en": "Apa Kerugian Faktor Daya (Power Factor) Rendah?", "id": "Arus Lebih Tinggi, Rugi Saluran." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave Rectifier)?", "id": "Menggunakan Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave Rectifier)?", "id": "Menggunakan Dua Atau Empat Dioda." },
  { "en": "Apa Keuntungan Penyearah Gelombang Penuh (Full-Wave Rectifier)?", "id": "Riak (Ripple) Lebih Kecil, Efisien." },
  { "en": "Apa Fungsi Kapasitor Filter Penyearah?", "id": "Menghaluskan Riak (Ripple) Tegangan DC." },
  { "en": "Apa Itu Regulator Zener (Zener Regulator)?", "id": "Regulator Tegangan Sederhana Dioda Zener." },
  { "en": "Apa Fungsi Dioda Zener (Zener Diode)?", "id": "Menjaga Tegangan Konstan Daerah Breakdown." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Osilasi (Oscillation)?", "id": "Umpan Balik Positif, Kriteria Barkhausen." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Menaikkan Amplitudo Sinyal." },
  { "en": "Apa Itu Penguatan (Gain) Penguat?", "id": "Rasio Sinyal Output Terhadap Input." },
  { "en": "Apa Satuan Penguatan (Gain) Tegangan?", "id": "Tanpa Satuan Atau Desibel (dB)." },
  { "en": "Apa Rumus Penguatan (Gain) Dalam Desibel (dB)?", "id": "Gain(dB) = 20 log(Vout/Vin)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Penguat?", "id": "Rentang Frekuensi Penguatan Efektif." },
  { "en": "Apa Itu Titik -3dB (Titik Setengah Daya)?", "id": "Frekuensi Saat Daya Turun Setengah." },
  { "en": "Apa Itu Distorsi (Distortion) Penguat?", "id": "Perubahan Bentuk Sinyal Output." },
  { "en": "Apa Itu Distorsi Harmonik (Harmonic Distortion)?", "id": "Munculnya Frekuensi Harmonisa Output." },
  { "en": "Apa Itu Distorsi Intermodulasi (Intermodulation Distortion)?", "id": "Munculnya Frekuensi Baru Akibat Pencampuran." },
  { "en": "Apa Itu Distorsi Crossover (Crossover Distortion)?", "id": "Distorsi Penguat Kelas B Dekat Nol." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Penguat?", "id": "Hambatan Dilihat Sumber Sinyal." },
  { "en": "Apa Impedansi Input (Input Impedance) Ideal Penguat Tegangan?", "id": "Tak Terhingga." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Penguat?", "id": "Hambatan Internal Dilihat Beban." },
  { "en": "Apa Impedansi Output (Output Impedance) Ideal Penguat Tegangan?", "id": "Nol." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Level Tegangan Logika TTL (Transistor-Transistor Logic)?", "id": "Sekitar 0V (Low), 5V (High)." },
  { "en": "Apa Level Tegangan Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Tergantung Tegangan Catu Daya (Vdd)." },
  { "en": "Apa Itu Sistem Bilangan Biner (Binary System)?", "id": "Basis Dua (Digit 0 Dan 1)." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Satuan Dasar Informasi Digital." },
  { "en": "Apa Itu Byte (Byte)?", "id": "Delapan Bit." },
  { "en": "Apa Itu Konversi Biner Ke Desimal?", "id": "Jumlahkan Pangkat Dua Posisi Bit." },
  { "en": "Apa Itu Konversi Desimal Ke Biner?", "id": "Bagi Berulang Dengan Dua (Sisa)." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal (Hexadecimal System)?", "id": "Basis Enam Belas (0-9, A-F)." },
  { "en": "Mengapa Heksadesimal (Hexadecimal) Digunakan Komputer?", "id": "Representasi Singkat Bilangan Biner." },
  { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter Teks." },
  { "en": "Apa Itu Transformasi Bintang-Delta (Star-Delta Transform)?", "id": "Konversi Antara Konfigurasi Y Dan Δ." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Daerah Sekitar Magnet Atau Arus Listrik." },
  { "en": "Apa Satuan Kuat Medan Magnet (H)?", "id": "Ampere Per Meter (A/m)." },
  { "en": "Apa Satuan Kerapatan Fluks Magnet (B)?", "id": "Tesla (T)." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik (μ)?", "id": "Kemampuan Bahan Mendukung Medan Magnet." },
  { "en": "Apa Itu Bahan Feromagnetik (Ferromagnetic Material)?", "id": "Permeabilitas Sangat Tinggi (Besi)." },
  { "en": "Apa Itu Elektromagnet (Electromagnet)?", "id": "Magnet Dibuat Aliran Arus Listrik." },
  { "en": "Apa Itu Hukum Ampere (Ampere's Law)?", "id": "Hubungan Medan Magnet Sekitar Arus." },
  { "en": "Apa Itu Hukum Induksi Faraday (Faraday's Law)?", "id": "Perubahan Fluks Magnet Induksi GGL." },
  { "en": "Apa Itu Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Induksi Diri?", "id": "GGL Akibat Perubahan Arus Sendiri." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Induksi Mutual?", "id": "GGL Akibat Perubahan Arus Kumparan Lain." },
  { "en": "Apa Itu Energi (Energy) Medan Magnet Induktor?", "id": "Energi Tersimpan U = ½LI²." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Bumi Untuk Keamanan." },
  { "en": "Apa Fungsi Utama Grounding (Pembumian)?", "id": "Melindungi Dari Sengatan Listrik." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Putus Saat Berlebih)." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Miniatur (Bisa Reset)." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pelindung Kebocoran Arus Ke Tanah." },
  { "en": "Apa Nama Lain ELCB (Earth Leakage Circuit Breaker)?", "id": "RCCB (Residual Current Circuit Breaker)." },
  { "en": "Apa Itu Isolator (Isolator) Listrik?", "id": "Bahan Penghambat Aliran Arus Listrik." },
  { "en": "Apa Itu Konduktor (Conductor) Listrik?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Contoh Konduktor (Conductor) Listrik Yang Baik?", "id": "Perak, Tembaga, Emas, Aluminium." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor, Isolator." },
  { "en": "Contoh Semikonduktor (Semiconductor) Murni?", "id": "Silikon (Si), Germanium (Ge)." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Atom Asing (Impuritas)." },
  { "en": "Apa Tujuan Doping (Doping) Semikonduktor?", "id": "Mengubah Sifat Konduktivitas Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor) Tipe-N?", "id": "Kelebihan Elektron (Donor)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor) Tipe-P?", "id": "Kelebihan Lubang (Akseptor)." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Pertemuan Bahan Tipe-P, Tipe-N." },
  { "en": "Komponen Apa Berbasis Sambungan P-N?", "id": "Dioda Dan Transistor." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Daerah Kosong Muatan Sekitar Sambungan." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Tegangan Positif Ke P, Negatif Ke N." },
  { "en": "Apa Efek Bias Maju (Forward Bias)?", "id": "Arus Mengalir Mudah Lewat Dioda." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Tegangan Negatif Ke P, Positif Ke N." },
  { "en": "Apa Efek Bias Mundur (Reverse Bias)?", "id": "Arus Sangat Kecil (Arus Bocor)." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Penyearah Arus Listrik." },
  { "en": "Apa Fungsi Utama Dioda (Diode)?", "id": "Mengalirkan Arus Satu Arah Saja." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya Saat Aktif." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Stabilisator Tegangan Mundur." },
  { "en": "Apa Itu Dioda Schottky (Schottky Diode)?", "id": "Dioda Cepat, Tegangan Maju Rendah." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Komponen Penguat Arus Terkontrol Arus." },
  { "en": "Apa Tiga Terminal BJT (Bipolar Junction Transistor)?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Fungsi BJT (Bipolar Junction Transistor) Sebagai Penguat?", "id": "Arus Basis Kecil Kontrol Kolektor Besar." },
  { "en": "Apa Fungsi BJT (Bipolar Junction Transistor) Sebagai Saklar?", "id": "Mode Cut-Off (Off), Saturasi (On)." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Komponen Penguat Terkontrol Tegangan." },
  { "en": "Apa Tiga Terminal FET (Field Effect Transistor)?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Apa Keunggulan FET (Field Effect Transistor)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET (Field Effect Transistor) Gerbang Terisolasi." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Diferensial Penguatan Sangat Tinggi." },
  { "en": "Apa Dua Input Op-Amp (Operational Amplifier)?", "id": "Input Inverting (-), Non-Inverting (+)." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open-Loop Gain)?", "id": "Penguatan Op-Amp Tanpa Umpan Balik." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengontrol Penguatan, Meningkatkan Stabilitas." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Op-Amp Penguatan Satu (Buffer)." },
  { "en": "Apa Itu Resistor (Resistor)?", "id": "Komponen Menghambat Aliran Arus." },
  { "en": "Satuan Apa Untuk Resistor (Resistor)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Kode Warna Resistor (Resistor)?", "id": "Gelang Warna Menentukan Nilai Resistansi." },
  { "en": "Apa Itu Toleransi (Tolerance) Resistor?", "id": "Persentase Penyimpangan Nilai Resistansi." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Menyimpan Energi Medan Listrik." },
  { "en": "Satuan Apa Untuk Kapasitor (Capacitor)?", "id": "Farad (F)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Polar?", "id": "Memiliki Polaritas Positif, Negatif (Elco)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Non-Polar?", "id": "Tidak Memiliki Polaritas (Keramik, Film)." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Komponen Menyimpan Energi Medan Magnet." },
  { "en": "Satuan Apa Untuk Induktor (Inductor)?", "id": "Henry (H)." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Besaran Apa Diukur Multimeter (Multimeter)?", "id": "Tegangan (Volt), Arus (Ampere), Hambatan (Ohm)." },
  { "en": "Bagaimana Pasang Voltmeter (Voltmeter)?", "id": "Paralel Dengan Komponen Diukur." },
  { "en": "Bagaimana Pasang Amperemeter (Ammeter)?", "id": "Seri Dengan Rangkaian Diukur." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Menampilkan Bentuk Gelombang Sinyal." },
  { "en": "Apa Sumbu Vertikal (Vertical Axis) Osiloskop?", "id": "Menunjukkan Amplitudo Tegangan." },
  { "en": "Apa Sumbu Horizontal (Horizontal Axis) Osiloskop?", "id": "Menunjukkan Waktu Atau Frekuensi." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Alat Menghasilkan Sinyal Uji." },
  { "en": "Bentuk Sinyal Apa Dihasilkan Generator?", "id": "Sinus, Kotak, Segitiga, Dll." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Sumber Energi Listrik Untuk Rangkaian." },
  { "en": "Apa Itu Arus Searah (Direct Current) (DC)?", "id": "Arus Mengalir Satu Arah Konstan." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current) (AC)?", "id": "Arus Berubah Arah Periodik." },
  { "en": "Apa Itu Frekuensi (Frequency)?", "id": "Jumlah Siklus Per Detik (Hz)." },
  { "en": "Apa Itu Periode (Period)?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Apa Itu Amplitudo (Amplitude)?", "id": "Nilai Puncak Maksimum Gelombang." },
  { "en": "Apa Itu Fasa (Phase)?", "id": "Posisi Relatif Gelombang Dalam Siklus." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis Dikontrol Listrik." },
  { "en": "Apa Bagian Utama Relay (Relay)?", "id": "Koil (Coil) Dan Kontak (Contact)." },
  { "en": "Apa Itu Kontak NO (Normally Open) Relay?", "id": "Kontak Terbuka Saat Koil Mati." },
  { "en": "Apa Itu Kontak NC (Normally Closed) Relay?", "id": "Kontak Tertutup Saat Koil Mati." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Komponen Mengubah Tegangan AC." },
  { "en": "Apa Prinsip Kerja Transformator (Transformer)?", "id": "Induksi Elektromagnetik Mutual." },
  { "en": "Apa Itu Belitan Primer (Primary Winding)?", "id": "Belitan Input Transformator." },
  { "en": "Apa Itu Belitan Sekunder (Secondary Winding)?", "id": "Belitan Output Transformator." },
  { "en": "Apa Itu Rasio Belitan (Turns Ratio)?", "id": "Perbandingan Jumlah Lilitan Primer, Sekunder." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Stator (Stator) Motor/Generator?", "id": "Bagian Yang Diam." },
  { "en": "Apa Itu Rotor (Rotor) Motor/Generator?", "id": "Bagian Yang Berputar." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Sistem Pembangkitan Hingga Konsumen." },
  { "en": "Apa Komponen Utama Sistem Tenaga?", "id": "Pembangkit, Transmisi, Distribusi." },
  { "en": "Apa Fungsi Pembangkit (Generation)?", "id": "Menghasilkan Energi Listrik." },
  { "en": "Apa Fungsi Transmisi (Transmission)?", "id": "Menyalurkan Listrik Jarak Jauh (Tegangan Tinggi)." },
  { "en": "Apa Fungsi Distribusi (Distribution)?", "id": "Menyalurkan Listrik Ke Konsumen (Tegangan Rendah)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Tempat Transformasi Tegangan, Switching." },
  { "en": "Mengapa Transmisi (Transmission) Tegangan Tinggi?", "id": "Mengurangi Rugi-Rugi Daya Saluran." },
  { "en": "Apa Itu Beban (Load) Listrik?", "id": "Peralatan Mengkonsumsi Energi Listrik." },
  { "en": "Apa Itu Resistansi (Resistance) Kabel?", "id": "Hambatan Listrik Kawat Konduktor." },
  { "en": "Apa Itu Kode IP (Ingress Protection)?", "id": "Peringkat Proteksi Terhadap Debu, Air." },
  { "en": "Angka Pertama Kode IP (Ingress Protection) Menunjukkan Apa?", "id": "Proteksi Terhadap Benda Padat (Debu)." },
  { "en": "Angka Kedua Kode IP (Ingress Protection) Menunjukkan Apa?", "id": "Proteksi Terhadap Cairan (Air)." },
  { "en": "Apa Arti IP65 (Ingress Protection 65)?", "id": "Tahan Debu Total, Semprotan Air." },
  { "en": "Apa Itu Kelas Isolasi (Insulation Class)?", "id": "Batas Suhu Operasi Aman Material Isolasi." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Banyak Komponen Dalam Satu Chip." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "Unit Pemroses Pusat (CPU) Dalam IC." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil (CPU, Memori, I/O) Dalam IC." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Tempat Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis Volatil." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca Non-Volatil." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Pendeteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Penggerak Mekanis (Hasil Kontrol)." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Tempat Komponen Terpasang." },
  { "en": "Apa Itu Solder (Soldering)?", "id": "Proses Penyambungan Komponen Ke PCB." },
  { "en": "Apa Itu Fluks (Flux) Solder?", "id": "Membantu Proses Penyolderan (Bersihkan Oksida)." },
  { "en": "Apa Itu Komponen SMD (Surface Mount Device)?", "id": "Komponen Dipasang Permukaan PCB." },
  { "en": "Apa Itu Komponen Through-Hole (Tembus Lubang)?", "id": "Komponen Kaki Menembus Lubang PCB." },
  { "en": "Apa Itu Safety Factor (Faktor Keamanan)?", "id": "Margin Keamanan Desain Teknik." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Ketidakseimbangan Muatan Listrik Permukaan." },
  { "en": "Bagaimana Listrik Statis (Static Electricity) Dihasilkan?", "id": "Gesekan Antara Dua Material Berbeda." },
  { "en": "Apa Itu Medan Listrik (Electric Field)?", "id": "Daerah Sekitar Muatan Gaya Listrik." },
  { "en": "Apa Arah Medan Listrik (Electric Field)?", "id": "Dari Muatan Positif Ke Negatif." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential)?", "id": "Energi Potensial Per Satuan Muatan." },
  { "en": "Apa Satuan Potensial Listrik (Electric Potential)?", "id": "Volt (V)." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik Dua Titik." },
  { "en": "Apa Satuan Tegangan (Voltage)?", "id": "Volt (V)." },
  { "en": "Apa Itu Arus (Current) Listrik?", "id": "Aliran Muatan Listrik (Elektron)." },
  { "en": "Apa Satuan Arus (Current)?", "id": "Ampere (A)." },
  { "en": "Apa Arah Arus Konvensional (Conventional Current)?", "id": "Aliran Muatan Positif (Teoretis)." },
  { "en": "Apa Arah Aliran Elektron (Electron Flow)?", "id": "Berlawanan Arus Konvensional (Nyata)." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Aliran Arus Listrik." },
  { "en": "Apa Satuan Resistansi (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Bunyi Hukum Ohm (Ohm's Law)?", "id": "V = I * R." },
  { "en": "Apa Faktor Mempengaruhi Resistansi (Resistance) Kawat?", "id": "Panjang, Luas, Jenis Bahan, Suhu." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Jenis Bahan Konduktor." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemampuan Bahan Hantarkan Listrik." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik Dihasilkan/Digunakan." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Rumus Daya (Power) (P)?", "id": "P = V * I." },
  { "en": "Apa Itu Energi (Energy) Listrik?", "id": "Daya Kali Waktu (E = P*t)." },
  { "en": "Apa Satuan Energi (Energy) Listrik Umum?", "id": "Kilowatt-Hour (kWh)." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Komponen Terhubung Ujung Ke Ujung." },
  { "en": "Bagaimana Arus (Current) Rangkaian Seri?", "id": "Arus Sama Di Semua Komponen." },
  { "en": "Bagaimana Tegangan (Voltage) Rangkaian Seri?", "id": "Tegangan Terbagi Antar Komponen." },
  { "en": "Bagaimana Resistansi Total (Total Resistance) Seri?", "id": "Jumlah Semua Resistansi (R1+R2)." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Komponen Terhubung Di Titik Sama." },
  { "en": "Bagaimana Tegangan (Voltage) Rangkaian Paralel?", "id": "Tegangan Sama Di Semua Komponen." },
  { "en": "Bagaimana Arus (Current) Rangkaian Paralel?", "id": "Arus Terbagi Antar Cabang." },
  { "en": "Bagaimana Resistansi Total (Total Resistance) Paralel?", "id": "Kebalikan Jumlah Kebalikan Resistansi." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source)?", "id": "Menyediakan Beda Potensial Konstan." },
  { "en": "Apa Itu Sumber Arus (Current Source)?", "id": "Menyediakan Arus Konstan." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Menyimpan Energi Medan Listrik." },
  { "en": "Satuan Apa Kapasitor (Capacitor)?", "id": "Farad (F)." },
  { "en": "Apa Itu Dielektrik (Dielectric) Kapasitor?", "id": "Bahan Isolator Antar Pelat." },
  { "en": "Bagaimana Kapasitansi Total (Total Capacitance) Seri?", "id": "Kebalikan Jumlah Kebalikan Kapasitansi." },
  { "en": "Bagaimana Kapasitansi Total (Total Capacitance) Paralel?", "id": "Jumlah Semua Kapasitansi (C1+C2)." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Menyimpan Energi Medan Magnet." },
  { "en": "Satuan Apa Induktor (Inductor)?", "id": "Henry (H)." },
  { "en": "Apa Sifat Induktor (Inductor) Terhadap Perubahan Arus?", "id": "Menentang Perubahan Arus Tiba-Tiba." },
  { "en": "Bagaimana Induktansi Total (Total Inductance) Seri?", "id": "Jumlah Semua Induktansi (L1+L2)." },
  { "en": "Bagaimana Induktansi Total (Total Inductance) Paralel?", "id": "Kebalikan Jumlah Kebalikan Induktansi." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Komponen AC (Kapasitor/Induktor)." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian AC (R+X)." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Alirkan Arus Satu Arah." },
  { "en": "Apa Itu Penyearahan (Rectification)?", "id": "Proses Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Penguat Atau Saklar Elektronik." },
  { "en": "Apa Dua Jenis Transistor Utama?", "id": "BJT (Bipolar Junction Transistor) Dan FET (Field Effect Transistor)." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Banyak Komponen Dalam Satu Chip." },
  { "en": "Apa Itu Sinyal Analog (Analog Signal)?", "id": "Sinyal Nilainya Kontinu Berubah." },
  { "en": "Apa Itu Sinyal Digital (Digital Signal)?", "id": "Sinyal Nilainya Diskrit (0 Atau 1)." },
  { "en": "Apa Itu Konversi Analog Ke Digital (ADC)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Konversi Digital Ke Analog (DAC)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Operasi Sirkuit Digital." },
  { "en": "Sebutkan Gerbang Logika (Logic Gate) Dasar?", "id": "AND, OR, NOT." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Frekuensi Standar (Standard Frequency) Listrik Indonesia?", "id": "Lima Puluh Hertz (50 Hz)." },
  { "en": "Apa Itu Tegangan Standar (Standard Voltage) Rumah Indonesia?", "id": "Dua Ratus Dua Puluh Volt (220 V)." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Kabel (Cable) Listrik?", "id": "Konduktor Berisolasi Hantarkan Listrik." },
  { "en": "Apa Fungsi Isolator (Insulator) Kabel?", "id": "Mencegah Kontak Listrik Berbahaya." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Putus Saat Arus Berlebih." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Pelindung Putus Otomatis (Bisa Reset)." },
  { "en": "Apa Itu Listrik Tiga Fasa (Three-Phase)?", "id": "Sistem Listrik Industri Daya Besar." },
  { "en": "Apa Beda Fasa (Phase Difference) Antar Fasa?", "id": "Seratus Dua Puluh Derajat (120°)." },
  { "en": "Apa Itu Pengukuran (Measurement) Listrik?", "id": "Menentukan Nilai Besaran Listrik." },
  { "en": "Alat Apa Ukur Tegangan (Voltage)?", "id": "Voltmeter (Voltmeter)." },
  { "en": "Alat Apa Ukur Arus (Current)?", "id": "Amperemeter (Ammeter)." },
  { "en": "Alat Apa Ukur Resistansi (Resistance)?", "id": "Ohmmeter (Ohmmeter)." },
  { "en": "Alat Apa Ukur Daya (Power)?", "id": "Wattmeter (Wattmeter)." },
  { "en": "Alat Apa Ukur Energi (Energy)?", "id": "kWh Meter (kWh Meter)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Sistem Listrik?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Rugi-Rugi (Losses) Daya?", "id": "Energi Listrik Hilang (Panas)." },
  { "en": "Apa Itu Keselamatan (Safety) Listrik?", "id": "Tindakan Pencegahan Bahaya Listrik." },
  { "en": "Mengapa Air Berbahaya Dekat Listrik?", "id": "Air Dapat Menghantarkan Listrik." },
  { "en": "Apa Itu Sengatan (Shock) Listrik?", "id": "Aliran Arus Lewat Tubuh Manusia." },
  { "en": "Apa Itu Hubungan Singkat (Short Circuit)?", "id": "Kontak Langsung Hambatan Rendah." },
  { "en": "Apa Itu Beban Berlebih (Overload)?", "id": "Arus Melebihi Kapasitas Rangkaian." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah DC Menjadi AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Kombinasi Medan Listrik, Magnet." },
  { "en": "Apa Itu Gelombang Radio (Radio Wave)?", "id": "Gelombang Elektromagnetik Komunikasi Nirkabel." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Pemancar Atau Penerima Gelombang Radio." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Sensor (Sensor) Listrik?", "id": "Mendeteksi Besaran Listrik (Arus, Tegangan)." },
  { "en": "Apa Itu Aktuator (Actuator) Listrik?", "id": "Penggerak Dikontrol Sinyal Listrik." },
  { "en": "Contoh Aktuator (Actuator) Listrik?", "id": "Motor, Solenoida, Relay." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Sistem Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Khusus Otomatisasi Industri." },
  { "en": "Bahasa Pemrograman PLC (Programmable Logic Controller) Umum?", "id": "Ladder Logic (Diagram Tangga)." },
  { "en": "Apa Itu Bus Komunikasi (Communication Bus)?", "id": "Jalur Berbagi Data Antar Perangkat." },
  { "en": "Contoh Bus Komunikasi (Communication Bus) Industri?", "id": "Modbus, Profibus, CAN Bus." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Skala Besar." },
  { "en": "Apa Itu Sinyal Analog 4-20mA?", "id": "Standar Sinyal Arus Industri." },
  { "en": "Apa Itu Bahan Magnetik (Magnetic Material)?", "id": "Bahan Berinteraksi Medan Magnet." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Magnetisasi Riwayat Medan." },
  { "en": "Apa Itu Efek Hall (Hall Effect)?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect)?", "id": "Sensor Deteksi Medan Magnet." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu (Efek Seebeck)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu (Resistansi Logam)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Peka Suhu." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Peka Cahaya." },
  { "en": "Apa Itu Fototransistor (Phototransistor)?", "id": "Transistor Peka Cahaya." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Peka Cahaya." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Terang?", "id": "Resistansi Menjadi Rendah." },
  { "en": "Bagaimana Resistansi LDR (Light Dependent Resistor) Saat Gelap?", "id": "Resistansi Menjadi Tinggi." },
  { "en": "Apa Itu Optocoupler (Optocoupler)?", "id": "Isolator Sinyal Berbasis Cahaya." },
  { "en": "Apa Fungsi Utama Optocoupler (Optocoupler)?", "id": "Mengisolasi Dua Rangkaian Listrik." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relay Elektronik Tanpa Kontak Mekanis." },
  { "en": "Apa Keunggulan SSR (Solid State Relay)?", "id": "Lebih Cepat, Awet, Hening." },
  { "en": "Apa Itu Saklar (Switch) SPST?", "id": "Single Pole Single Throw." },
  { "en": "Apa Fungsi Saklar SPST (Single Pole Single Throw)?", "id": "Menyambung Atau Memutus Satu Sirkuit." },
  { "en": "Apa Itu Saklar (Switch) SPDT?", "id": "Single Pole Double Throw." },
  { "en": "Apa Fungsi Saklar SPDT (Single Pole Double Throw)?", "id": "Memilih Antara Dua Sirkuit." },
  { "en": "Apa Itu Tombol Tekan (Push Button)?", "id": "Saklar Sementara (Momentary)." },
  { "en": "Apa Itu Normally Open (NO) Push Button?", "id": "Terbuka Normal, Tertutup Saat Ditekan." },
  { "en": "Apa Itu Normally Closed (NC) Push Button?", "id": "Tertutup Normal, Terbuka Saat Ditekan." },
  { "en": "Apa Itu Saklar DIP (Dual In-line Package)?", "id": "Saklar Kecil Pengaturan Konfigurasi." },
  { "en": "Apa Itu Saklar Rotary (Rotary Switch)?", "id": "Saklar Pemilih Posisi Putar." },
  { "en": "Apa Itu Potensiometer (Potentiometer)?", "id": "Resistor Variabel Tiga Terminal." },
  { "en": "Apa Fungsi Potensiometer (Potentiometer)?", "id": "Sebagai Pembagi Tegangan Variabel." },
  { "en": "Apa Itu Rheostat (Rheostat)?", "id": "Resistor Variabel Dua Terminal." },
  { "en": "Apa Fungsi Rheostat (Rheostat)?", "id": "Sebagai Pengatur Arus Variabel." },
  { "en": "Apa Itu Trimmer Potentiometer (Trimpot)?", "id": "Potensiometer Kecil Penyetelan Presisi." },
  { "en": "Apa Itu Gelombang Sinus (Sine Wave)?", "id": "Bentuk Gelombang Dasar Arus AC." },
  { "en": "Apa Itu Gelombang Kotak (Square Wave)?", "id": "Sinyal Digital (Dua Level)." },
  { "en": "Apa Itu Gelombang Segitiga (Triangle Wave)?", "id": "Gelombang Naik Turun Linear." },
  { "en": "Apa Itu Gelombang Gigi Gergaji (Sawtooth Wave)?", "id": "Gelombang Naik/Turun Linear Cepat." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle)?", "id": "Persentase Waktu Sinyal Aktif (On)." },
  { "en": "Bagaimana Hitung Siklus Kerja (Duty Cycle)?", "id": "(Waktu On / Periode Total) x 100%." },
  { "en": "Apa Itu Osilasi (Oscillation)?", "id": "Getaran Atau Perubahan Periodik." },
  { "en": "Apa Itu Redaman (Damping)?", "id": "Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Frekuensi Alami (Natural Frequency)?", "id": "Frekuensi Osilasi Sistem Tanpa Redaman." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Amplitudo Maksimal Saat Frekuensi Sama Alami." },
  { "en": "Apa Itu Rangkaian Resonansi (Resonant Circuit)?", "id": "Rangkaian RLC (Resistor Induktor Kapasitor) Pada Frekuensi Resonansi." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Kualitas Rangkaian Resonansi." },
  { "en": "Q (Faktor Kualitas) Tinggi Berarti Apa?", "id": "Resonansi Tajam, Bandwidth Sempit." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi Efektif." },
  { "en": "Apa Itu Titik Setengah Daya (-3dB)?", "id": "Frekuensi Daya Turun Setengah." },
  { "en": "Apa Hubungan Q, Resonansi, Bandwidth?", "id": "Q = Frekuensi Resonansi / Bandwidth." },
  { "en": "Apa Itu Filter (Filter) Elektronik?", "id": "Rangkaian Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Passband (Pita Lolos) Filter?", "id": "Frekuensi Yang Dilewatkan Filter." },
  { "en": "Apa Itu Stopband (Pita Henti) Filter?", "id": "Frekuensi Yang Diredam Filter." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Transisi Passband, Stopband." },
  { "en": "Apa Itu Roll-Off (Roll-Off) Filter?", "id": "Kecuraman Atenuasi Di Stopband." },
  { "en": "Apa Satuan Roll-Off (Roll-Off)?", "id": "Desibel Per Oktaf (dB/octave)." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Komponen Pasif (R, L, C)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Komponen Aktif (Op-Amp)." },
  { "en": "Apa Keuntungan Filter Aktif (Active Filter)?", "id": "Bisa Penguatan, Tanpa Induktor." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Roll-Off Filter." },
  { "en": "Filter Orde Pertama (First Order) Roll-Off?", "id": "-20 dB Per Dekade." },
  { "en": "Filter Orde Kedua (Second Order) Roll-Off?", "id": "-40 dB Per Dekade." },
  { "en": "Apa Itu Tipe Respons Filter?", "id": "Butterworth, Chebyshev, Bessel." },
  { "en": "Respons Filter Butterworth (Butterworth Filter)?", "id": "Passband Datar Maksimal (Maximally Flat)." },
  { "en": "Respons Filter Chebyshev (Chebyshev Filter)?", "id": "Roll-Off Curam, Riak Passband." },
  { "en": "Respons Filter Bessel (Bessel Filter)?", "id": "Respon Fasa Linear Terbaik." },
  { "en": "Apa Itu Fasa (Phase) Sinyal?", "id": "Posisi Relatif Gelombang Dalam Siklus." },
  { "en": "Apa Itu Pergeseran Fasa (Phase Shift)?", "id": "Perbedaan Fasa Antara Input, Output." },
  { "en": "Komponen Apa Sebabkan Pergeseran Fasa (Phase Shift)?", "id": "Kapasitor Dan Induktor." },
  { "en": "Bagaimana Fasa Kapasitor (Capacitor)?", "id": "Arus Mendahului Tegangan 90 Derajat." },
  { "en": "Bagaimana Fasa Induktor (Inductor)?", "id": "Tegangan Mendahului Arus 90 Derajat." },
  { "en": "Apa Itu Medan Listrik Statis (Electrostatic Field)?", "id": "Medan Dihasilkan Muatan Diam." },
  { "en": "Apa Itu Medan Magnet Statis (Magnetostatic Field)?", "id": "Medan Dihasilkan Arus Konstan." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Medan Dihasilkan Muatan Bergerak." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan E, B Merambat." },
  { "en": "Apa Kecepatan Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Kecepatan Cahaya (c)." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Radiasi/Terima Gelombang Elektromagnetik." },
  { "en": "Apa Itu Radiasi (Radiation) Elektromagnetik?", "id": "Pancaran Energi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang?", "id": "Orientasi Getaran Medan Listrik." },
  { "en": "Apa Itu Panjang Gelombang (Wavelength) (Lambda)?", "id": "Jarak Satu Siklus Gelombang." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang Radio?", "id": "Perambatan Gelombang Radio Di Medium." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Struktur Pemandu Gelombang Elektromagnetik." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Apa Itu Refleksi (Reflection) Sinyal?", "id": "Sinyal Memantul Akibat Mismatch Impedansi." },
  { "en": "Apa Itu Gelombang Berdiri (Standing Wave)?", "id": "Interferensi Gelombang Datang, Pantul." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Rasio Gelombang Berdiri." },
  { "en": "Nilai VSWR (Voltage Standing Wave Ratio) Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Transfer Daya Optimal." },
  { "en": "Alat Apa Untuk Impedance Matching (Pencocokan Impedansi)?", "id": "Transformator, Stub, Jaringan LC." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Grafik Bantu Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Penguat (Amplifier) RF?", "id": "Penguat Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Osilator (Oscillator) RF?", "id": "Penghasil Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Mixer (Mixer) RF?", "id": "Pencampur Frekuensi Sinyal RF." },
  { "en": "Apa Itu Filter (Filter) RF?", "id": "Filter Rangkaian Frekuensi Radio." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Carrier RF." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Ekstraksi Informasi Dari Sinyal RF." },
  { "en": "Apa Itu Modulasi AM (Amplitude Modulation)?", "id": "Informasi Mengubah Amplitudo Carrier." },
  { "en": "Apa Itu Modulasi FM (Frequency Modulation)?", "id": "Informasi Mengubah Frekuensi Carrier." },
  { "en": "Apa Itu Modulasi PM (Phase Modulation)?", "id": "Informasi Mengubah Fasa Carrier." },
  { "en": "Apa Itu Superposisi (Superposition)?", "id": "Efek Total Jumlah Efek Individual." },
  { "en": "Apa Itu Linearitas (Linearity)?", "id": "Sistem Output Proporsional Input." },
  { "en": "Apa Itu Non-Linearitas (Non-Linearity)?", "id": "Sistem Output Tidak Proporsional Input." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Dari Fundamental." },
  { "en": "Apa Itu Intermodulasi (Intermodulation)?", "id": "Produk Frekuensi Akibat Non-Linearitas." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Sinyal Acak Tak Diinginkan." },
  { "en": "Apa Itu Rasio Sinyal-Derau (SNR)?", "id": "Ukuran Kekuatan Sinyal Relatif Derau." },
  { "en": "Apa Itu Angka Derau (Noise Figure)?", "id": "Ukuran Degradasi SNR Oleh Perangkat." },
  { "en": "Apa Itu Penguat Common Source (Common Source) MOSFET?", "id": "Konfigurasi Penguat MOSFET (Metal-Oxide-Semiconductor FET) Umum." },
  { "en": "Apa Karakteristik Penguat Common Source (Common Source)?", "id": "Penguatan Tegangan Tinggi, Fasa Terbalik." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain) MOSFET?", "id": "Konfigurasi Pengikut Source (Source Follower)." },
  { "en": "Apa Nama Lain Penguat Common Drain (Common Drain)?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Karakteristik Penguat Common Drain (Common Drain)?", "id": "Penguatan Tegangan Hampir Satu (Buffer)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate) MOSFET?", "id": "Konfigurasi Impedansi Input Rendah." },
  { "en": "Apa Itu Beban Aktif (Active Load) Penguat?", "id": "Menggunakan Transistor Sebagai Beban." },
  { "en": "Apa Keuntungan Beban Aktif (Active Load)?", "id": "Penguatan Tegangan Tinggi, Hemat Ruang IC." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Menyalin Arus Referensi Ke Output." },
  { "en": "Apa Fungsi Cermin Arus (Current Mirror)?", "id": "Sebagai Beban Aktif Atau Bias Arus." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Sinyal Mode Bersama (Common Mode Signal)?", "id": "Komponen Sinyal Sama Kedua Input." },
  { "en": "Apa Itu Sinyal Mode Diferensial (Differential Mode Signal)?", "id": "Komponen Sinyal Berbeda Kedua Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Bersama." },
  { "en": "CMRR (Common Mode Rejection Ratio) Tinggi Menandakan Apa?", "id": "Penolakan Noise Mode Bersama Baik." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Output Ke Input." },
  { "en": "Apa Efek Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Gain, Mengurangi Distorsi." },
  { "en": "Apa Itu Penguatan Loop Tertutup (Closed Loop Gain)?", "id": "Penguatan Dengan Umpan Balik Negatif." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Non-Inverting Gain Satu." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Buffer (Penyangga) Impedansi." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Tinggi?", "id": "Tidak Banyak Menarik Arus Sumber." },
  { "en": "Apa Itu Impedansi Output (Output Impedance) Rendah?", "id": "Mampu Memberi Arus Ke Beban." },
  { "en": "Apa Itu Penjumlah Inverting (Inverting Summing Amplifier)?", "id": "Menjumlahkan Tegangan Input (Output Terbalik)." },
  { "en": "Apa Itu Penguat Diferensi (Difference Amplifier)?", "id": "Menguatkan Selisih Dua Tegangan Input." },
  { "en": "Apa Itu Integrator (Integrator) Op-Amp?", "id": "Output Adalah Integral Input." },
  { "en": "Apa Itu Diferensiator (Differentiator) Op-Amp?", "id": "Output Adalah Turunan Input." },
  { "en": "Apa Masalah Rangkaian Diferensiator (Differentiator) Sederhana?", "id": "Tidak Stabil, Rentan Noise Tinggi." },
  { "en": "Apa Itu Komparator (Comparator) Op-Amp?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Output Komparator (Comparator)?", "id": "Tegangan Saturasi Positif Atau Negatif." },
  { "en": "Apa Itu Schmitt Trigger (Schmitt Trigger)?", "id": "Komparator (Comparator) Dengan Histeresis." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Perbedaan Level Threshold Naik Turun." },
  { "en": "Apa Fungsi Histeresis (Hysteresis) Schmitt Trigger?", "id": "Mencegah Osilasi Akibat Noise." },
  { "en": "Apa Itu Osilator Jembatan Wien (Wien Bridge Oscillator)?", "id": "Penghasil Gelombang Sinus Relatif Murni." },
  { "en": "Apa Itu Osilator Geser Fasa (Phase Shift Oscillator)?", "id": "Osilator Menggunakan Jaringan RC Penggeser Fasa." },
  { "en": "Apa Itu Osilator Colpitts (Colpitts Oscillator)?", "id": "Osilator LC (Induktor Kapasitor) Umpan Balik Kapasitif." },
  { "en": "Apa Itu Osilator Hartley (Hartley Oscillator)?", "id": "Osilator LC (Induktor Kapasitor) Umpan Balik Induktif." },
  { "en": "Apa Itu Osilator Kristal (Crystal Oscillator)?", "id": "Osilator Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Multivibrator Astabil (Astable Multivibrator)?", "id": "Osilator Gelombang Kotak (Tanpa Input)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable Multivibrator)?", "id": "Penghasil Satu Pulsa (One-Shot)." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable Multivibrator)?", "id": "Memiliki Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC (Integrated Circuit) Serbaguna Timer, Osilator." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Kalang Terkunci Fasa." },
  { "en": "Apa Fungsi PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi FM, Pemulihan Clock." },
  { "en": "Apa Itu VCO (Voltage Controlled Oscillator)?", "id": "Osilator Frekuensi Diatur Tegangan." },
  { "en": "Apa Itu Detektor Fasa (Phase Detector)?", "id": "Membandingkan Fasa Dua Sinyal Input." },
  { "en": "Apa Itu Filter Loop (Loop Filter) PLL?", "id": "Filter Low-Pass Mengontrol VCO." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Tipe Filter Berdasarkan Respons?", "id": "Low-Pass, High-Pass, Band-Pass, Band-Stop." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Filter?", "id": "Frekuensi Batas Operasi Filter (-3dB)." },
  { "en": "Apa Itu Roll-Off (Roll-Off) Filter?", "id": "Tingkat Atenuasi Di Stopband." },
  { "en": "Apa Itu Respons Butterworth (Butterworth Response)?", "id": "Passband Paling Datar (Maximally Flat)." },
  { "en": "Apa Itu Respons Chebyshev (Chebyshev Response)?", "id": "Roll-Off Curam Dengan Riak Passband." },
  { "en": "Apa Itu Respons Bessel (Bessel Response)?", "id": "Respon Fasa Linear Terbaik (Delay Rata)." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Terkendali Tegangan?", "id": "VCVS (Voltage Controlled Voltage Source)." },
  { "en": "Apa Itu Sumber Arus (Current Source) Terkendali Tegangan?", "id": "VCCS (Voltage Controlled Current Source)." },
  { "en": "Apa Itu Sumber Tegangan (Voltage Source) Terkendali Arus?", "id": "CCVS (Current Controlled Voltage Source)." },
  { "en": "Apa Itu Sumber Arus (Current Source) Terkendali Arus?", "id": "CCCS (Current Controlled Current Source)." },
  { "en": "Apa Itu Transkonduktansi (Transconductance) (gm)?", "id": "Rasio Perubahan Arus Output, Tegangan Input." },
  { "en": "Apa Itu Transresistansi (Transresistance) (Rm)?", "id": "Rasio Perubahan Tegangan Output, Arus Input." },
  { "en": "Apa Itu Penguatan Arus (Current Gain) (Ai)?", "id": "Rasio Perubahan Arus Output, Arus Input." },
  { "en": "Apa Itu Penguatan Tegangan (Voltage Gain) (Av)?", "id": "Rasio Perubahan Tegangan Output, Tegangan Input." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Komponen Elektronik Terintegrasi Satu Chip." },
  { "en": "Apa Bahan Dasar IC (Integrated Circuit)?", "id": "Silikon (Silicon) Murni." },
  { "en": "Apa Itu Wafer (Wafer) Silikon?", "id": "Lembaran Tipis Silikon Bahan IC." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan IC Di Wafer." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Teknik Pencetakan Pola Sirkuit." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Proses Penghilangan Material Selektif." },
  { "en": "Apa Itu Doping (Doping)?", "id": "Penambahan Impuritas Ubah Sifat Semikonduktor." },
  { "en": "Apa Itu Deposisi (Deposition)?", "id": "Proses Pelapisan Material Tipis." },
  { "en": "Apa Itu Kemasan (Packaging) IC?", "id": "Wadah Pelindung IC Dengan Kaki (Pin)." },
  { "en": "Apa Itu Pin (Pin) IC?", "id": "Kaki Logam Koneksi Eksternal IC." },
  { "en": "Apa Itu Datasheet (Lembar Data) IC?", "id": "Dokumentasi Spesifikasi Teknis IC." },
  { "en": "Apa Itu Nilai Maksimum Absolut (Absolute Maximum Ratings)?", "id": "Batas Kondisi Operasi Tak Boleh Dilewati." },
  { "en": "Apa Itu Karakteristik Listrik (Electrical Characteristics)?", "id": "Parameter Kinerja IC Kondisi Tertentu." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Merusak IC." },
  { "en": "Bagaimana Penanganan IC (Integrated Circuit) Yang Benar?", "id": "Gunakan Tindakan Pencegahan ESD." },
  { "en": "Apa Itu Gelang Anti-Statis (Anti-Static Wrist Strap)?", "id": "Alat Mencegah Penumpukan Muatan Statis." },
  { "en": "Apa Itu Solder (Soldering)?", "id": "Proses Penyambungan Logam Timah Cair." },
  { "en": "Apa Itu Desoldering (Desoldering)?", "id": "Proses Pelepasan Sambungan Solder." },
  { "en": "Apa Itu Solder Besi (Soldering Iron)?", "id": "Alat Pemanas Untuk Menyolder." },
  { "en": "Apa Itu Timah Solder (Solder Wire)?", "id": "Logam Paduan Pengisi Sambungan." },
  { "en": "Apa Itu Fluks (Flux) Solder?", "id": "Bahan Kimia Pembersih Permukaan Logam." },
  { "en": "Mengapa Perlu Fluks (Flux) Saat Menyolder?", "id": "Menghilangkan Oksida, Membasahi Sambungan." },
  { "en": "Apa Itu Solder Wick (Sumbu Solder)?", "id": "Pita Tembaga Penyerap Sisa Timah." },
  { "en": "Apa Itu Solder Sucker (Penyedot Timah)?", "id": "Alat Vakum Penyedot Timah Lebihan." },
  { "en": "Apa Itu Komponen SMD (Surface Mount Device)?", "id": "Komponen Tanpa Kaki Tembus PCB." },
  { "en": "Apa Keuntungan Komponen SMD (Surface Mount Device)?", "id": "Ukuran Kecil, Perakitan Otomatis." },
  { "en": "Apa Itu Solder Reflow (Reflow Soldering)?", "id": "Metode Solder SMD (Surface Mount Device) Oven." },
  { "en": "Apa Itu Pasta Solder (Solder Paste)?", "id": "Campuran Bola Timah, Fluks (Reflow)." },
  { "en": "Apa Itu Stensil (Stencil) SMT?", "id": "Cetakan Aplikasi Pasta Solder SMD." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Tercetak Koneksi Komponen." },
  { "en": "Apa Bahan Dasar PCB (Printed Circuit Board)?", "id": "FR-4 (Fiberglass Epoxy)." },
  { "en": "Apa Itu Jalur (Trace) PCB?", "id": "Garis Tembaga Konduktif Di PCB." },
  { "en": "Apa Itu Pad (Pad) PCB?", "id": "Area Tembaga Tempat Solder Komponen." },
  { "en": "Apa Itu Via (Via) PCB?", "id": "Lubang Konduktif Penghubung Lapisan PCB." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung Jalur PCB (Hijau)." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Label Teks, Simbol Di Atas PCB." },
  { "en": "Apa Itu Ground Plane (Bidang Tanah) PCB?", "id": "Area Tembaga Luas Terhubung Ground." },
  { "en": "Apa Fungsi Ground Plane (Bidang Tanah)?", "id": "Return Path Arus, Shielding EMI." },
  { "en": "Apa Itu Power Plane (Bidang Daya) PCB?", "id": "Area Tembaga Luas Sumber Daya (VCC)." },
  { "en": "Apa Itu Multilayer PCB (PCB Multi Lapis)?", "id": "PCB Lebih Dari Dua Lapisan Tembaga." },
  { "en": "Apa Keuntungan Multilayer PCB (PCB Multi Lapis)?", "id": "Kepadatan Komponen Tinggi, Routing Mudah." },
  { "en": "Apa Itu Skematik (Schematic) Elektronik?", "id": "Diagram Logis Rangkaian (Simbol)." },
  { "en": "Apa Itu Layout (Layout) PCB?", "id": "Desain Fisik Penempatan Komponen, Jalur." },
  { "en": "Perangkat Lunak Apa Untuk Desain PCB?", "id": "Eagle, KiCad, Altium, OrCAD." },
  { "en": "Apa Itu File Gerber (Gerber File)?", "id": "Format Standar Manufaktur PCB." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar Komponen Perakitan PCB." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy) Pengukuran?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision) Pengukuran?", "id": "Kedekatan Hasil Ukur Berulang." },
  { "en": "Apa Itu Resolusi (Resolution) Alat Ukur?", "id": "Perubahan Terkecil Dapat Dideteksi." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Pengukuran?", "id": "Rentang Keraguan Nilai Hasil Ukur." },
  { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Digital Multimeter (Alat Ukur Digital)." },
  { "en": "Apa Kepanjangan DMM (Digital Multimeter)?", "id": "Multimeter Digital." },
  { "en": "Besaran Apa Diukur DMM (Digital Multimeter)?", "id": "Tegangan, Arus, Resistansi (Utama)." },
  { "en": "Apa Itu Mode Kontinuitas (Continuity Test) DMM?", "id": "Memeriksa Sambungan Sirkuit (Buzzer)." },
  { "en": "Apa Itu Mode Tes Dioda (Diode Test) DMM?", "id": "Mengukur Tegangan Maju Dioda." },
  { "en": "Apa Itu True RMS (True RMS) Multimeter?", "id": "Mengukur Nilai RMS Akurat Gelombang Non-Sinus." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Menampilkan Bentuk Gelombang Sinyal." },
  { "en": "Apa Sumbu Vertikal (Vertical Axis) Osiloskop?", "id": "Tegangan (Volt Per Divisi)." },
  { "en": "Apa Sumbu Horizontal (Horizontal Axis) Osiloskop?", "id": "Waktu (Detik Per Divisi)." },
  { "en": "Apa Itu Pemicuan (Triggering) Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Berulang." },
  { "en": "Apa Itu Probe (Probe) Osiloskop?", "id": "Kabel Koneksi Osiloskop Ke Rangkaian." },
  { "en": "Apa Itu Atenuasi (Attenuation) Probe 10X?", "id": "Mengurangi Sinyal 10 Kali (Impedansi Tinggi)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Osiloskop?", "id": "Frekuensi Maksimum Dapat Diukur Akurat." },
  { "en": "Apa Itu Sample Rate (Laju Cuplik) Osiloskop Digital?", "id": "Seberapa Cepat Sinyal Dicuplik ADC." },
  { "en": "Apa Itu Generator Fungsi (Function Generator)?", "id": "Penghasil Sinyal Uji (Sinus, Kotak, dll.)." },
  { "en": "Apa Itu Catu Daya (Power Supply) Laboratorium?", "id": "Sumber Tegangan/Arus DC Variabel." },
  { "en": "Apa Fitur Catu Daya (Power Supply) Laboratorium?", "id": "Regulasi Tegangan (CV), Arus (CC)." },
  { "en": "Apa Itu Mode CV (Constant Voltage)?", "id": "Menjaga Tegangan Output Konstan." },
  { "en": "Apa Itu Mode CC (Constant Current)?", "id": "Menjaga Arus Output Konstan." },
  { "en": "Apa Itu Pengukur LCR (LCR Meter)?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Menganalisis Sinyal Digital Multi Kanal." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Menganalisis Sinyal Domain Frekuensi." },
  { "en": "Apa Itu Jaringan Vektor Analyzer (VNA)?", "id": "Mengukur Parameter S Jaringan RF." },
  { "en": "Apa Itu Pengujian Komponen (Component Testing)?", "id": "Memverifikasi Karakteristik Komponen." },
  { "en": "Bagaimana Menguji Resistor (Resistor)?", "id": "Ukur Resistansi Dengan Ohmmeter." },
  { "en": "Bagaimana Menguji Kapasitor (Capacitor)?", "id": "Ukur Kapasitansi (LCR), Tes Bocor." },
  { "en": "Bagaimana Menguji Induktor (Inductor)?", "id": "Ukur Induktansi (LCR), Tes Kontinuitas." },
  { "en": "Bagaimana Menguji Dioda (Diode)?", "id": "Gunakan Mode Tes Dioda Multimeter." },
  { "en": "Bagaimana Menguji Transistor BJT (Bipolar Junction Transistor)?", "id": "Tes Sambungan Dioda BE, BC, Ukur Hfe." },
  { "en": "Bagaimana Menguji Transistor MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Tes Gerbang (Gate Charging), Drain-Source." },
  { "en": "Apa Itu Pemecahan Masalah (Troubleshooting)?", "id": "Proses Identifikasi, Perbaikan Kesalahan." },
  { "en": "Langkah Awal Pemecahan Masalah (Troubleshooting)?", "id": "Verifikasi Masalah, Periksa Sumber Daya." },
  { "en": "Metode Pemecahan Masalah (Troubleshooting) Umum?", "id": "Setengah Pemisahan (Half-Splitting), Penggantian." },
  { "en": "Apa Itu Setengah Pemisahan (Half-Splitting)?", "id": "Membagi Rangkaian Cari Area Bermasalah." },
  { "en": "Alat Bantu Pemecahan Masalah (Troubleshooting)?", "id": "Multimeter, Osiloskop, Skematik." },
  { "en": "Apa Pentingnya Skematik (Schematic)?", "id": "Peta Rangkaian Untuk Analisis." },
  { "en": "Kesalahan Umum Rangkaian Elektronik?", "id": "Solder Buruk, Komponen Rusak, Kesalahan Desain." },
  { "en": "Apa Itu Sambungan Dingin (Cold Solder Joint)?", "id": "Solderan Tidak Menyatu Sempurna." },
  { "en": "Apa Itu Jembatan Solder (Solder Bridge)?", "id": "Solder Hubung Singkat Jalur Berdekatan." },
  { "en": "Bagaimana Identifikasi Komponen Terbakar?", "id": "Perubahan Warna, Bau Gosong, Fisik Rusak." },
  { "en": "Apa Itu Keselamatan Bengkel (Workshop Safety)?", "id": "Praktik Aman Bekerja Elektronika." },
  { "en": "Mengapa Gunakan Kacamata Pengaman (Safety Glasses)?", "id": "Melindungi Mata Dari Serpihan Solder." },
  { "en": "Kapan Gunakan Gelang Anti-Statis (Anti-Static Wrist Strap)?", "id": "Saat Menangani Komponen Sensitif ESD." },
  { "en": "Bahaya Apa Dari Solder Besi Panas?", "id": "Luka Bakar Kulit." },
  { "en": "Mengapa Ventilasi Penting Saat Menyolder?", "id": "Menghindari Menghirup Asap Timah/Fluks." },
  { "en": "Bagaimana Menyimpan Bahan Kimia Elektronik?", "id": "Tempat Sejuk, Kering, Berlabel Jelas." },
  { "en": "Apa Yang Dilakukan Jika Terkena Sengatan Listrik?", "id": "Putuskan Sumber, Cari Bantuan Medis." },
  { "en": "Apa Itu Proyek Elektronika (Electronics Project)?", "id": "Membangun Rangkaian Elektronik Fungsional." },
  { "en": "Tahapan Proyek Elektronika (Electronics Project)?", "id": "Ide, Desain, Prototipe, Pengujian, Finalisasi." },
  { "en": "Apa Itu Prototipe (Prototype)?", "id": "Versi Awal Rangkaian Untuk Pengujian." },
  { "en": "Dimana Membuat Prototipe (Prototype)?", "id": "Breadboard Atau Protoboard." },
  { "en": "Mengapa Pengujian (Testing) Penting?", "id": "Memastikan Rangkaian Bekerja Sesuai Desain." },
  { "en": "Apa Itu Debugging (Debugging) Proyek?", "id": "Mencari Dan Memperbaiki Kesalahan Proyek." },
  { "en": "Apa Itu Dokumentasi (Documentation) Proyek?", "id": "Mencatat Desain, Proses, Hasil Proyek." },
  { "en": "Apa Itu Arduino (Arduino)?", "id": "Platform Mikrokontroler Open Source Populer." },
  { "en": "Bahasa Pemrograman Apa Digunakan Arduino?", "id": "Bahasa C++ (Dengan Library Arduino)." },
  { "en": "Apa Itu Raspberry Pi (Raspberry Pi)?", "id": "Komputer Papan Tunggal (Single-Board Computer)." },
  { "en": "Apa Beda Arduino (Arduino), Raspberry Pi (Raspberry Pi)?", "id": "Arduino (Mikrokontroler), Pi (Komputer Kecil)." },
  { "en": "Apa Itu Sensor Shield (Sensor Shield) Arduino?", "id": "Papan Tambahan Memudahkan Koneksi Sensor." },
  { "en": "Apa Itu Komunitas Open Source Hardware?", "id": "Berbagi Desain Perangkat Keras Terbuka." },
  { "en": "Apa Itu Etika Rekayasa (Engineering Ethics)?", "id": "Prinsip Moral Profesi Insinyur." },
  { "en": "Tanggung Jawab Utama Insinyur (Engineer)?", "id": "Keselamatan, Kesehatan, Kesejahteraan Publik." },
  { "en": "Apa Itu Kekayaan Intelektual (Intellectual Property)?", "id": "Hak Cipta, Paten, Merek Dagang." },
  { "en": "Apa Itu Paten (Patent)?", "id": "Hak Eksklusif Atas Penemuan." },
  { "en": "Apa Itu Hak Cipta (Copyright)?", "id": "Hak Eksklusif Atas Karya Kreatif." },
  { "en": "Apa Itu Merek Dagang (Trademark)?", "id": "Tanda Pembeda Produk/Jasa." },
  { "en": "Apa Itu Standar Teknik (Engineering Standard)?", "id": "Spesifikasi, Pedoman Teknis Disepakati." },
  { "en": "Organisasi Standar Teknik Internasional?", "id": "ISO (International Organization for Standardization), IEC (International Electrotechnical Commission)." },
  { "en": "Organisasi Standar Teknik Elektro Populer?", "id": "IEEE (Institute of Electrical and Electronics Engineers), IEC (International Electrotechnical Commission)." },
  { "en": "Apa Kepanjangan IEEE (Institute of Electrical and Electronics Engineers)?", "id": "Institut Insinyur Listrik Dan Elektronika." },
  { "en": "Apa Kepanjangan IEC (International Electrotechnical Commission)?", "id": "Komisi Elektroteknik Internasional." },
  { "en": "Apa Itu Konversi Energi (Energy Conversion)?", "id": "Mengubah Energi Satu Bentuk Ke Lain." },
  { "en": "Contoh Konversi Energi (Energy Conversion) Elektro?", "id": "Motor (Listrik->Mekanik), Generator (Mekanik->Listrik)." },
  { "en": "Apa Hukum Kekekalan Energi (Conservation of Energy)?", "id": "Energi Tidak Dapat Diciptakan/Dimusnahkan." },
  { "en": "Apa Itu Efisiensi (Efficiency) Konversi Energi?", "id": "Rasio Energi Output Berguna, Input Total." },
  { "en": "Mengapa Efisiensi (Efficiency) Tidak 100 Persen?", "id": "Selalu Ada Rugi-Rugi (Panas)." },
  { "en": "Apa Itu Sumber Energi Terbarukan (Renewable Energy)?", "id": "Sumber Energi Dapat Diperbarui Alami." },
  { "en": "Contoh Sumber Energi Terbarukan (Renewable Energy)?", "id": "Surya, Angin, Air, Panas Bumi." },
  { "en": "Apa Itu Sel Surya (Solar Cell) (Fotovoltaik)?", "id": "Mengubah Energi Cahaya Matahari Ke Listrik." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Angin Ke Listrik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (Hydro Power)?", "id": "Mengubah Energi Potensial Air Ke Listrik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi (Geothermal)?", "id": "Mengubah Panas Bumi Ke Listrik." },
  { "en": "Apa Itu Biomassa (Biomass)?", "id": "Sumber Energi Bahan Organik." },
  { "en": "Apa Tantangan Energi Terbarukan (Renewable Energy)?", "id": "Intermitensi (Tidak Kontinu), Penyimpanan." },
  { "en": "Apa Itu Sistem Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Untuk Nanti (Baterai)." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Modern Integrasi Terbarukan." },
  { "en": "Apa Itu Kabel Serat Optik (Fiber Optic)?", "id": "Media Transmisi Data Menggunakan Cahaya." },
  { "en": "Apa Keunggulan Serat Optik (Fiber Optic)?", "id": "Kapasitas Tinggi, Kebal Interferensi EM." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total Cahaya." },
  { "en": "Apa Itu Mode Tunggal (Single Mode) Serat Optik?", "id": "Inti Kecil, Jarak Jauh, Bandwidth Tinggi." },
  { "en": "Apa Itu Mode Jamak (Multi Mode) Serat Optik?", "id": "Inti Besar, Jarak Pendek, Lebih Murah." },
  { "en": "Apa Itu Atenuasi (Attenuation) Serat Optik?", "id": "Pelemahan Sinyal Cahaya Sepanjang Serat." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya (Batasi Bandwidth)." },
  { "en": "Apa Sumber Cahaya (Light Source) Serat Optik?", "id": "LED (Light Emitting Diode) Atau Dioda Laser." },
  { "en": "Apa Detektor (Detector) Serat Optik?", "id": "Fotodioda (Photodiode)." },
  { "en": "Apa Itu Splicing (Splicing) Serat Optik?", "id": "Penyambungan Dua Ujung Serat Optik." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Alat Sambungan Serat Optik (SC, LC)." },
  { "en": "Apa Itu Pengukuran OTDR (Optical Time Domain Reflectometer)?", "id": "Mendeteksi Kerusakan, Jarak Serat Optik." },
  { "en": "Apa Itu Elektromagnetisme Komputasi (CEM)?", "id": "Simulasi Numerik Fenomena Elektromagnetik." },
  { "en": "Metode Apa Digunakan CEM (Computational Electromagnetics)?", "id": "FEM (Finite Element Method), FDTD (Finite Difference Time Domain), MoM (Method of Moments)." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric Material)?", "id": "Isolator Dapat Terpolarisasi Medan Listrik." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant) (Permitivitas Relatif)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Rugi Dielektrik (Dielectric Loss)?", "id": "Energi Hilang Bahan Dielektrik Medan AC." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Breakdown." },
  { "en": "Apa Itu Bahan Piezoelektrik (Piezoelectric Material)?", "id": "Hasilkan Listrik Saat Ditekan Mekanis." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect) Terbalik?", "id": "Listrik Hasilkan Deformasi Mekanis." },
  { "en": "Apa Itu Sensor Ultrasonik (Ultrasonic Sensor)?", "id": "Gunakan Gelombang Suara Ultrasonik Ukur Jarak." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Mengubah Energi Satu Bentuk Ke Lain." },
  { "en": "Contoh Transduser (Transducer)?", "id": "Mikrofon (Suara->Listrik), Speaker (Listrik->Suara)." },
  { "en": "Apa Itu Pengkondisian Sinyal (Signal Conditioning)?", "id": "Memproses Sinyal Sensor Agar Sesuai." },
  { "en": "Apa Saja Proses Pengkondisian Sinyal?", "id": "Penguatan, Penyaringan, Linierisasi, Isolasi." },
  { "en": "Apa Itu Penguatan (Amplification)?", "id": "Meningkatkan Amplitudo Sinyal Lemah." },
  { "en": "Apa Itu Penyaringan (Filtering)?", "id": "Menghilangkan Komponen Frekuensi Tak Diinginkan." },
  { "en": "Apa Itu Linierisasi (Linearization)?", "id": "Membuat Respon Sensor Menjadi Linear." },
  { "en": "Apa Itu Isolasi (Isolation) Sinyal?", "id": "Memisahkan Ground Sensor Dan Akuisisi." },
  { "en": "Mengapa Perlu Isolasi (Isolation) Sinyal?", "id": "Keamanan, Menghilangkan Ground Loop." },
  { "en": "Apa Itu Akuisisi Data (Data Acquisition) (DAQ)?", "id": "Proses Pengumpulan Data Analog Ke Digital." },
  { "en": "Apa Komponen Utama Sistem DAQ (Data Acquisition System)?", "id": "Sensor, Pengkondisi, ADC, Komputer." },
  { "en": "Apa Itu Resolusi (Resolution) ADC (Analog-to-Digital Converter)?", "id": "Jumlah Bit Output Digital." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) ADC?", "id": "Seberapa Cepat Sinyal Analog Dicuplik." },
  { "en": "Apa Itu Teorema Nyquist (Nyquist Theorem)?", "id": "Laju Sampel Minimal Dua Kali Frekuensi Maks." },
  { "en": "Apa Itu Aliasing (Aliasing) ADC?", "id": "Distorsi Akibat Laju Sampel Terlalu Rendah." },
  { "en": "Bagaimana Mencegah Aliasing (Aliasing)?", "id": "Gunakan Filter Anti-Aliasing (Low-Pass)." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Analog Ke Level Digital." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Selisih Nilai Analog Asli, Hasil Kuantisasi." },
  { "en": "Resolusi Lebih Tinggi Berarti Kesalahan Kuantisasi Bagaimana?", "id": "Kesalahan Kuantisasi Lebih Kecil." },
  { "en": "Apa Itu Antarmuka Instrumen (Instrument Interface)?", "id": "Cara Menghubungkan Alat Ukur Ke Komputer." },
  { "en": "Contoh Antarmuka Instrumen (Instrument Interface)?", "id": "GPIB (General Purpose Interface Bus), USB (Universal Serial Bus), Ethernet." },
  { "en": "Apa Kepanjangan GPIB (General Purpose Interface Bus)?", "id": "Bus Antarmuka Tujuan Umum." },
  { "en": "Apa Itu LabVIEW (LabVIEW)?", "id": "Perangkat Lunak Pemrograman Grafis Akuisisi Data." },
  { "en": "Apa Itu Pemrosesan Sinyal Digital (DSP)?", "id": "Manipulasi Sinyal Menggunakan Teknik Digital." },
  { "en": "Operasi Dasar DSP (Digital Signal Processing)?", "id": "Filtering, Transformasi Fourier, Konvolusi." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma DSP Ubah Spektrum Sinyal." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Filter Digital Respon Impuls Terbatas." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Filter Digital Respon Impuls Tak Terbatas." },
  { "en": "Mana Lebih Stabil, FIR (Finite Impulse Response) Atau IIR (Infinite Impulse Response)?", "id": "FIR (Finite Impulse Response) Selalu Stabil." },
  { "en": "Mana Lebih Efisien Komputasi, FIR Atau IIR?", "id": "IIR (Infinite Impulse Response) Umumnya Lebih Efisien." },
  { "en": "Apa Itu Transformasi Fourier Cepat (FFT)?", "id": "Algoritma Efisien Analisis Spektrum Frekuensi." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis (Respon Sistem LTI)." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Tidak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Bagaimana Output Sistem LTI Dihitung?", "id": "Konvolusi Input Dengan Respon Impuls." },
  { "en": "Apa Itu Transformasi Z (Z-Transform)?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Bidang-Z (Z-Plane)?", "id": "Bidang Kompleks Analisis Transformasi Z." },
  { "en": "Apa Hubungan Kestabilan Sistem Diskrit, Pole?", "id": "Stabil Jika Semua Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Pengontrol PID (Proportional Integral Derivative)?", "id": "Kontroler Umpan Balik Industri Umum." },
  { "en": "Apa Fungsi Aksi Proporsional (P)?", "id": "Respon Proporsional Terhadap Error." },
  { "en": "Apa Fungsi Aksi Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Apa Fungsi Aksi Derivatif (D)?", "id": "Memberi Respon Prediktif (Antisipasi)." },
  { "en": "Apa Itu Penalaan (Tuning) PID?", "id": "Menentukan Parameter Kp, Ki, Kd Optimal." },
  { "en": "Metode Penalaan (Tuning) PID Umum?", "id": "Metode Ziegler-Nichols." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Respon Transien (Transient Response) Sistem Kontrol?", "id": "Perilaku Sistem Menuju Keadaan Tunak." },
  { "en": "Apa Itu Error Keadaan Tunak (Steady-State Error)?", "id": "Selisih Setpoint, Output Saat Stabil." },
  { "en": "Apa Itu Overshoot (Overshoot)?", "id": "Output Melebihi Setpoint Sebelum Stabil." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Output Naik Dari 10% Ke 90%." },
  { "en": "Apa Itu Waktu Turun (Settling Time)?", "id": "Waktu Output Masuk, Tetap Dalam Batas Error." },
  { "en": "Apa Itu Root Locus (Tempat Kedudukan Akar)?", "id": "Metode Grafis Analisis Kestabilan, Respon." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Respon Frekuensi (Magnitudo, Fasa)." },
  { "en": "Apa Itu Gain Margin (GM) (Batas Penguatan)?", "id": "Ukuran Kestabilan Relatif (Penguatan)." },
  { "en": "Apa Itu Phase Margin (PM) (Batas Fasa)?", "id": "Ukuran Kestabilan Relatif (Fasa)." },
  { "en": "Apa Itu Diagram Nyquist (Nyquist Plot)?", "id": "Plot Respon Frekuensi Domain Polar." },
  { "en": "Apa Itu Kriteria Kestabilan Nyquist (Nyquist Criterion)?", "id": "Menentukan Kestabilan Loop Tertutup." },
  { "en": "Apa Itu Kontroler Lead (Lead Compensator)?", "id": "Memperbaiki Respon Transien (Kecepatan)." },
  { "en": "Apa Itu Kontroler Lag (Lag Compensator)?", "id": "Memperbaiki Error Keadaan Tunak (Akurasi)." },
  { "en": "Apa Itu Kontroler Lead-Lag (Lead-Lag Compensator)?", "id": "Gabungan Manfaat Lead Dan Lag." },
  { "en": "Apa Itu Representasi Ruang Keadaan (State Space)?", "id": "Model Matematis Sistem (Matriks)." },
  { "en": "Apa Itu Vektor Keadaan (State Vector)?", "id": "Kumpulan Variabel Keadaan Internal." },
  { "en": "Apa Itu Keterkendalian (Controllability)?", "id": "Kemampuan Input Mengubah Keadaan." },
  { "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Output Tentukan Keadaan." },
  { "en": "Apa Itu Kontrol Digital (Digital Control)?", "id": "Implementasi Kontroler Komputer Digital." },
  { "en": "Apa Keuntungan Kontrol Digital (Digital Control)?", "id": "Fleksibel, Presisi, Mudah Implementasi Kompleks." },
  { "en": "Apa Itu Discretization (Diskritisasi)?", "id": "Mengubah Model Kontinu Ke Diskrit." },
  { "en": "Apa Itu Zero-Order Hold (ZOH)?", "id": "Metode Rekonstruksi Sinyal DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Kepanjangan ZOH (Zero-Order Hold)?", "id": "Penahan Orde Nol." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Logika Terprogram Industri." },
  { "en": "Bahasa Pemrograman PLC (Programmable Logic Controller) Utama?", "id": "Ladder Logic (Diagram Tangga)." },
  { "en": "Apa Itu Input/Output (I/O) PLC?", "id": "Terminal Koneksi Sensor, Aktuator." },
  { "en": "Apa Itu Siklus Scan (Scan Cycle) PLC?", "id": "Baca Input -> Eksekusi Program -> Tulis Output." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Operator Mesin." },
  { "en": "Apa Fungsi HMI (Human Machine Interface)?", "id": "Visualisasi, Kontrol Proses Industri." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Terpusat Skala Luas." },
  { "en": "Apa Itu RTU (Remote Terminal Unit) SCADA?", "id": "Unit Lapangan Akuisisi Data, Kontrol." },
  { "en": "Apa Itu DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi Proses Industri." },
  { "en": "Apa Beda SCADA (Supervisory Control and Data Acquisition), DCS (Distributed Control System)?", "id": "SCADA (Lebih Pengawasan), DCS (Lebih Kontrol Proses)." },
  { "en": "Apa Itu Fieldbus (Bus Lapangan)?", "id": "Jaringan Komunikasi Digital Industri." },
  { "en": "Contoh Protokol Fieldbus (Bus Lapangan)?", "id": "Profibus, Modbus, Foundation Fieldbus, CANopen." },
  { "en": "Apa Keuntungan Fieldbus (Bus Lapangan)?", "id": "Mengurangi Pengkabelan, Diagnostik Lebih Baik." },
  { "en": "Apa Itu Sistem Otomatisasi (Automation System)?", "id": "Sistem Mengurangi Intervensi Manusia." },
  { "en": "Apa Itu Sensor Analog (Analog Sensor)?", "id": "Output Sinyal Kontinu Proporsional." },
  { "en": "Apa Itu Sensor Digital (Digital Sensor)?", "id": "Output Sinyal Diskrit (On/Off)." },
  { "en": "Apa Itu Sinyal Analog 0-10V?", "id": "Standar Sinyal Tegangan Kontrol Industri." },
  { "en": "Apa Itu Sinyal Analog 4-20mA?", "id": "Standar Sinyal Arus Kontrol Industri." },
  { "en": "Keuntungan Sinyal Arus 4-20mA?", "id": "Kebal Noise, Deteksi Putus Kabel." },
  { "en": "Mengapa Mulai Dari 4mA Bukan 0mA?", "id": "Membedakan Sinyal Nol, Kabel Putus." },
  { "en": "Apa Itu Kalibrasi (Calibration) Sensor?", "id": "Menyesuaikan Output Sensor Sesuai Standar." },
  { "en": "Apa Itu Linearitas (Linearity) Sensor?", "id": "Seberapa Linear Output Terhadap Input." },
  { "en": "Apa Itu Histeresis (Hysteresis) Sensor?", "id": "Perbedaan Output Arah Naik Turun." },
  { "en": "Apa Itu Repeatability (Repeatability) Sensor?", "id": "Kemampuan Sensor Beri Hasil Sama." },
  { "en": "Apa Itu Waktu Respon (Response Time) Sensor?", "id": "Waktu Sensor Merespon Perubahan Input." },
  { "en": "Apa Itu Drift (Drift) Sensor?", "id": "Perubahan Output Sensor Seiring Waktu." },
  { "en": "Apa Itu Resolusi (Resolution) Sensor?", "id": "Perubahan Input Terkecil Terdeteksi." },
  { "en": "Apa Itu Rentang (Range) Sensor?", "id": "Batas Minimum, Maksimum Pengukuran." },
  { "en": "Apa Itu Span (Span) Sensor?", "id": "Selisih Batas Maksimum, Minimum Rentang." },
  { "en": "Apa Itu Transduser (Transducer) Tekanan?", "id": "Mengubah Tekanan Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser (Transducer) Suhu?", "id": "Mengubah Suhu Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser (Transducer) Level?", "id": "Mengubah Ketinggian Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser (Transducer) Aliran?", "id": "Mengubah Laju Aliran Sinyal Listrik." },
  { "en": "Apa Itu Load Cell (Sel Beban)?", "id": "Transduser Mengubah Gaya/Berat Sinyal Listrik." },
  { "en": "Prinsip Apa Digunakan Load Cell (Sel Beban)?", "id": "Strain Gauge (Pengukur Regangan)." },
  { "en": "Apa Itu Penguat Sinyal (Signal Amplifier)?", "id": "Meningkatkan Kekuatan Sinyal Sensor." },
  { "en": "Apa Itu Filter Sinyal (Signal Filter)?", "id": "Menghilangkan Noise Dari Sinyal Sensor." },
  { "en": "Apa Itu Isolator Sinyal (Signal Isolator)?", "id": "Memisahkan Elektrikal Sinyal Input, Output." },
  { "en": "Apa Fungsi Isolator Sinyal (Signal Isolator)?", "id": "Proteksi, Menghilangkan Ground Loop." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX) Analog?", "id": "Memilih Satu Sinyal Analog Input." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX) Analog?", "id": "Mengarahkan Sinyal Analog Ke Output." },
  { "en": "Apa Itu Sistem Kontrol Loop Terbuka?", "id": "Kontrol Tanpa Mengukur Hasil Output." },
  { "en": "Apa Itu Sistem Kontrol Loop Tertutup?", "id": "Kontrol Menggunakan Umpan Balik Output." },
  { "en": "Apa Itu Variabel Proses (Process Variable) (PV)?", "id": "Besaran Fisik Yang Dikontrol." },
  { "en": "Apa Itu Setpoint (Setpoint) (SP)?", "id": "Nilai Target Diinginkan Variabel Proses." },
  { "en": "Apa Itu Kesalahan (Error) (e)?", "id": "Selisih Antara Setpoint, Variabel Proses." },
  { "en": "Apa Itu Manipulated Variable (MV)?", "id": "Output Kontroler Mengatur Elemen Final." },
  { "en": "Apa Itu Elemen Kontrol Final (Final Control Element)?", "id": "Perangkat Mengubah Proses (Katup, Pemanas)." },
  { "en": "Apa Itu Gangguan (Disturbance)?", "id": "Faktor Eksternal Mengganggu Proses." },
  { "en": "Apa Itu Aksi Kontrol Proporsional (P)?", "id": "Output Kontrol Proporsional Terhadap Error." },
  { "en": "Apa Itu Gain (Gain) Proporsional (Kp)?", "id": "Penguatan Aksi Kontrol Proporsional." },
  { "en": "Apa Itu Offset (Offset) Kontrol Proporsional?", "id": "Error Keadaan Tunak Sisa." },
  { "en": "Apa Itu Aksi Kontrol Integral (I)?", "id": "Output Kontrol Proporsional Integral Error." },
  { "en": "Apa Fungsi Aksi Integral (I)?", "id": "Menghilangkan Offset (Error Keadaan Tunak)." },
  { "en": "Apa Itu Waktu Integral (Integral Time) (Ti)?", "id": "Parameter Kecepatan Aksi Integral." },
  { "en": "Apa Itu Aksi Kontrol Derivatif (D)?", "id": "Output Kontrol Proporsional Laju Error." },
  { "en": "Apa Fungsi Aksi Derivatif (D)?", "id": "Memberi Respon Cepat (Antisipasi)." },
  { "en": "Apa Itu Waktu Derivatif (Derivative Time) (Td)?", "id": "Parameter Kekuatan Aksi Derivatif." },
  { "en": "Apa Masalah Aksi Derivatif (D)?", "id": "Sensitif Terhadap Noise Proses." },
  { "en": "Apa Itu Kontroler PID (Proportional Integral Derivative)?", "id": "Menggabungkan Aksi Proporsional, Integral, Derivatif." },
  { "en": "Apa Itu Penalaan (Tuning) PID?", "id": "Menentukan Nilai Kp, Ti, Td Optimal." },
  { "en": "Apa Itu Kestabilan (Stability) Loop Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Osilasi (Oscillation) Loop Kontrol?", "id": "Output Berayun Sekitar Setpoint." },
  { "en": "Apa Itu Kontrol Umpan Maju (Feedforward Control)?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Output." },
  { "en": "Apa Itu Kontrol Cascade (Cascade Control)?", "id": "Dua Loop Kontrol (Master, Slave)." },
  { "en": "Apa Itu Kontrol Rasio (Ratio Control)?", "id": "Menjaga Rasio Tertentu Dua Variabel." },
  { "en": "Apa Itu Kontrol Split Range (Split Range Control)?", "id": "Satu Output Kontroler Atur Dua Elemen." },
  { "en": "Apa Itu Dead Time (Waktu Mati) Proses?", "id": "Waktu Tunda Awal Respon Proses." },
  { "en": "Apa Itu Kapasitas (Capacitance) Proses?", "id": "Kemampuan Proses Menyimpan Energi/Material." },
  { "en": "Apa Itu Resistansi (Resistance) Proses?", "id": "Hambatan Terhadap Aliran Energi/Material." },
  { "en": "Apa Itu Proses Orde Pertama (First Order Process)?", "id": "Proses Dengan Satu Konstanta Waktu." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) Proses?", "id": "Ukuran Kelambatan Respon Proses." },
  { "en": "Apa Itu Proses Self-Regulating (Self-Regulating Process)?", "id": "Proses Mencapai Keseimbangan Baru Sendiri." },
  { "en": "Apa Itu Proses Integrating (Integrating Process)?", "id": "Output Terus Berubah Jika Input Ada." },
  { "en": "Contoh Proses Integrating (Integrating Process)?", "id": "Level Tangki Dengan Pompa Keluar." },
  { "en": "Apa Itu Katup Kontrol (Control Valve)?", "id": "Elemen Final Pengatur Aliran Fluida." },
  { "en": "Apa Itu Aktuator (Actuator) Katup?", "id": "Penggerak Katup (Pneumatik, Elektrik)." },
  { "en": "Apa Itu Positioner (Positioner) Katup?", "id": "Memastikan Posisi Katup Sesuai Sinyal." },
  { "en": "Apa Karakteristik Aliran (Flow Characteristic) Katup?", "id": "Hubungan Posisi Katup, Laju Aliran." },
  { "en": "Sebutkan Karakteristik Aliran (Flow Characteristic) Umum?", "id": "Linear, Equal Percentage, Quick Opening." },
  { "en": "Apa Itu Fail-Open (Fail-Open) Katup?", "id": "Katup Terbuka Jika Sinyal Hilang." },
  { "en": "Apa Itu Fail-Close (Fail-Close) Katup?", "id": "Katup Tertutup Jika Sinyal Hilang." },
  { "en": "Apa Itu Sinyal Pneumatik (Pneumatic Signal) Standar?", "id": "Tekanan Udara 3-15 Psi." },
  { "en": "Apa Itu Konverter I/P (I/P Converter)?", "id": "Mengubah Sinyal Arus (I) Ke Pneumatik (P)." },
  { "en": "Apa Itu Konverter P/I (P/I Converter)?", "id": "Mengubah Sinyal Pneumatik (P) Ke Arus (I)." },
  { "en": "Apa Itu Diagram P&ID (Piping and Instrumentation Diagram)?", "id": "Diagram Teknik Proses Industri." },
  { "en": "Apa Kepanjangan P&ID (Piping and Instrumentation Diagram)?", "id": "Diagram Perpipaan Dan Instrumentasi." },
  { "en": "Apa Informasi Dalam P&ID (Piping and Instrumentation Diagram)?", "id": "Peralatan Proses, Perpipaan, Instrumentasi, Kontrol." },
  { "en": "Apa Itu Loop Diagram (Diagram Loop)?", "id": "Diagram Detail Koneksi Loop Kontrol." },
  { "en": "Apa Itu Tag Number (Nomor Tag) Instrumen?", "id": "Identifikasi Unik Setiap Instrumen." },
  { "en": "Apa Itu Standar ISA (International Society of Automation)?", "id": "Standar Instrumentasi, Otomatisasi." },
  { "en": "Apa Itu Programmable Logic Controller (PLC)?", "id": "Kontroler Logika Terprogram Industri." },
  { "en": "Apa Bahasa Pemrograman Utama PLC?", "id": "Ladder Diagram (Diagram Tangga)." },
  { "en": "Apa Itu Input/Output (I/O) PLC?", "id": "Modul Koneksi Sensor, Aktuator." },
  { "en": "Apa Itu Input Diskrit (Discrete Input) PLC?", "id": "Input Sinyal On/Off (Saklar)." },
  { "en": "Apa Itu Output Diskrit (Discrete Output) PLC?", "id": "Output Sinyal On/Off (Relay, Lampu)." },
  { "en": "Apa Itu Input Analog (Analog Input) PLC?", "id": "Input Sinyal Kontinu (4-20mA, 0-10V)." },
  { "en": "Apa Itu Output Analog (Analog Output) PLC?", "id": "Output Sinyal Kontinu (Kontrol Katup)." },
  { "en": "Apa Itu Siklus Scan (Scan Cycle) PLC?", "id": "Baca Input -> Eksekusi Logika -> Tulis Output." },
  { "en": "Apa Itu Waktu Scan (Scan Time) PLC?", "id": "Durasi Satu Siklus Scan Lengkap." },
  { "en": "Apa Itu Watchdog Timer (WDT) PLC?", "id": "Timer Pemantau Eksekusi Program Normal." },
  { "en": "Apa Fungsi Watchdog Timer (WDT)?", "id": "Reset PLC Jika Program Macet." },
  { "en": "Apa Itu Modul Komunikasi (Communication Module) PLC?", "id": "Modul Koneksi Jaringan (Ethernet, Modbus)." },
  { "en": "Apa Itu Distributed Control System (DCS)?", "id": "Sistem Kontrol Terdistribusi Skala Besar." },
  { "en": "Apa Beda Utama PLC, DCS?", "id": "PLC (Logika Cepat), DCS (Proses Kontinu)." },
  { "en": "Apa Itu Supervisory Control and Data Acquisition (SCADA)?", "id": "Sistem Pengawasan, Akuisisi Data Terpusat." },
  { "en": "Apa Fungsi Utama SCADA (Supervisory Control and Data Acquisition)?", "id": "Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Itu Human Machine Interface (HMI)?", "id": "Antarmuka Grafis Operator Sistem Otomatisasi." },
  { "en": "Apa Fungsi HMI (Human Machine Interface)?", "id": "Visualisasi Data, Input Perintah Operator." },
  { "en": "Apa Itu Jaringan Industri (Industrial Network)?", "id": "Jaringan Komunikasi Khusus Lingkungan Industri." },
  { "en": "Contoh Jaringan Industri (Industrial Network)?", "id": "Ethernet/IP, Profinet, Modbus TCP." },
  { "en": "Apa Itu Protokol Modbus (Modbus Protocol)?", "id": "Protokol Komunikasi Serial Industri Populer." },
  { "en": "Apa Itu Modbus RTU (Remote Terminal Unit)?", "id": "Varian Modbus Serial (RS-485/RS-232)." },
  { "en": "Apa Itu Modbus TCP/IP (Transmission Control Protocol/Internet Protocol)?", "id": "Varian Modbus Lewat Jaringan Ethernet." },
  { "en": "Apa Itu Master (Master) Modbus?", "id": "Perangkat Memulai Komunikasi (Minta Data)." },
  { "en": "Apa Itu Slave (Slave) Modbus?", "id": "Perangkat Merespon Permintaan Master." },
  { "en": "Apa Itu Profibus (Process Field Bus)?", "id": "Standar Fieldbus Populer Eropa." },
  { "en": "Apa Itu Profinet (Process Field Network)?", "id": "Standar Ethernet Industri (Pengembangan Profibus)." },
  { "en": "Apa Itu Ethernet/IP (Ethernet Industrial Protocol)?", "id": "Standar Ethernet Industri (Rockwell Automation)." },
  { "en": "Apa Itu Safety PLC (Safety Programmable Logic Controller)?", "id": "PLC Khusus Aplikasi Keselamatan (SIS)." },
  { "en": "Apa Itu Sistem Instrumen Keselamatan (SIS)?", "id": "Safety Instrumented System (Pengaman Proses)." },
  { "en": "Apa Fungsi Utama SIS (Safety Instrumented System)?", "id": "Membawa Proses Ke Keadaan Aman." },
  { "en": "Apa Itu Tingkat Integritas Keselamatan (SIL)?", "id": "Ukuran Keandalan Fungsi Keselamatan." },
  { "en": "SIL (Safety Integrity Level) Berapa Yang Paling Andal?", "id": "SIL 4 (Sangat Andal)." },
  { "en": "Apa Itu Analisis Bahaya (Hazard Analysis)?", "id": "Identifikasi Potensi Bahaya Proses." },
  { "en": "Apa Itu Manajemen Risiko (Risk Management)?", "id": "Mengelola Risiko Ke Tingkat Diterima." },
  { "en": "Apa Itu Diagram Blok Fungsional (Functional Block Diagram)?", "id": "Representasi Grafis Fungsi Sistem Kontrol." },
  { "en": "Apa Itu Pengujian FAT (Factory Acceptance Test)?", "id": "Pengujian Sistem Di Pabrik Pembuat." },
  { "en": "Apa Kepanjangan FAT (Factory Acceptance Test)?", "id": "Tes Penerimaan Pabrik." },
  { "en": "Apa Itu Pengujian SAT (Site Acceptance Test)?", "id": "Pengujian Sistem Di Lokasi Pemasangan." },
  { "en": "Apa Kepanjangan SAT (Site Acceptance Test)?", "id": "Tes Penerimaan Lokasi." },
  { "en": "Apa Itu Komisioning (Commissioning)?", "id": "Proses Menghidupkan, Menguji Sistem Baru." },
  { "en": "Apa Itu Pemeliharaan Preventif (Preventive Maintenance)?", "id": "Pemeliharaan Terjadwal Cegah Kerusakan." },
  { "en": "Apa Itu Pemeliharaan Prediktif (Predictive Maintenance)?", "id": "Pemeliharaan Berdasarkan Prediksi Kondisi." },
  { "en": "Apa Itu Pemeliharaan Korektif (Corrective Maintenance)?", "id": "Pemeliharaan Setelah Terjadi Kerusakan." },
  { "en": "Apa Itu Analisis Akar Masalah (RCA)?", "id": "Root Cause Analysis (Cari Penyebab Utama)." },
  { "en": "Apa Kepanjangan RCA (Root Cause Analysis)?", "id": "Analisis Akar Penyebab." },
  { "en": "Apa Itu Mean Time To Repair (MTTR)?", "id": "Waktu Rata-Rata Untuk Perbaikan." },
  { "en": "Apa Kepanjangan MTTR (Mean Time To Repair)?", "id": "Waktu Rata-Rata Untuk Perbaikan." },
  { "en": "Apa Itu Ketersediaan (Availability) Sistem?", "id": "Persentase Waktu Sistem Beroperasi." },
  { "en": "Apa Itu Redundansi (Redundancy)?", "id": "Duplikasi Komponen Tingkatkan Keandalan." },
  { "en": "Apa Itu Redundansi Panas (Hot Standby)?", "id": "Cadangan Siap Ambil Alih Langsung." },
  { "en": "Apa Itu Redundansi Dingin (Cold Standby)?", "id": "Cadangan Perlu Dihidupkan Manual/Otomatis." },
  { "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?", "id": "Kemampuan Sistem Tetap Operasi Saat Gagal." },
  { "en": "Apa Itu Mode Kegagalan Aman (Fail-Safe)?", "id": "Sistem Gagal Ke Kondisi Aman." },
  { "en": "Apa Itu Mode Kegagalan Operasi (Fail-Operational)?", "id": "Sistem Tetap Operasi Walau Gagal Sebagian." },
  { "en": "Apa Itu Teori Rangkaian (Circuit Theory)?", "id": "Analisis Perilaku Rangkaian Listrik Ideal." },
  { "en": "Apa Elemen Dasar Rangkaian Listrik?", "id": "Sumber, Resistor, Kapasitor, Induktor." },
  { "en": "Apa Itu Rangkaian Linear (Linear Circuit)?", "id": "Rangkaian Berlaku Prinsip Superposisi." },
  { "en": "Apa Itu Rangkaian Non-Linear (Non-Linear Circuit)?", "id": "Rangkaian Mengandung Komponen Non-Linear." },
  { "en": "Contoh Komponen Non-Linear?", "id": "Dioda, Transistor." },
  { "en": "Apa Itu Analisis Domain Waktu (Time Domain)?", "id": "Analisis Perilaku Sinyal Terhadap Waktu." },
  { "en": "Apa Itu Analisis Domain Frekuensi (Frequency Domain)?", "id": "Analisis Perilaku Sinyal Terhadap Frekuensi." },
  { "en": "Transformasi Apa Ubah Domain Waktu Ke Frekuensi?", "id": "Transformasi Fourier Atau Laplace." },
  { "en": "Apa Itu Kondisi Awal (Initial Condition)?", "id": "Keadaan Rangkaian (Tegangan/Arus) Awal." },
  { "en": "Mengapa Kondisi Awal (Initial Condition) Penting?", "id": "Menentukan Respon Transien Rangkaian." },
  { "en": "Apa Itu Sumber Independen (Independent Source)?", "id": "Sumber Nilainya Tidak Tergantung Rangkaian." },
  { "en": "Apa Itu Sumber Dependen (Dependent Source)?", "id": "Sumber Nilainya Tergantung Variabel Lain." },
  { "en": "Apa Itu Dualitas (Duality) Rangkaian?", "id": "Hubungan Timbal Balik Antar Konsep Rangkaian." },
  { "en": "Apa Dual (Dual) Dari Tegangan (Voltage)?", "id": "Arus (Current)." },
  { "en": "Apa Dual (Dual) Dari Resistansi (Resistance)?", "id": "Konduktansi (Conductance)." },
  { "en": "Apa Dual (Dual) Dari Induktansi (Inductance)?", "id": "Kapasitansi (Capacitance)." },
  { "en": "Apa Dual (Dual) Dari Rangkaian Seri?", "id": "Rangkaian Paralel." },
  { "en": "Apa Dual (Dual) Dari Hukum KVL (Kirchhoff's Voltage Law)?", "id": "Hukum KCL (Kirchhoff's Current Law)." },
  { "en": "Apa Itu Teorema Millman (Millman's Theorem)?", "id": "Menyederhanakan Rangkaian Sumber Paralel." },
  { "en": "Apa Itu Teorema Substitusi (Substitution Theorem)?", "id": "Mengganti Cabang Dengan Sumber Ekuivalen." },
  { "en": "Apa Itu Teorema Resiprositas (Reciprocity Theorem)?", "id": "Hubungan Timbal Balik Sumber, Respon Linear." },
  { "en": "Apa Itu Analisis Sensitivitas (Sensitivity Analysis)?", "id": "Studi Pengaruh Perubahan Komponen." },
  { "en": "Apa Itu Toleransi (Tolerance) Komponen?", "id": "Variasi Nilai Komponen Dari Nominal." },
  { "en": "Apa Itu Analisis Kasus Terburuk (Worst-Case Analysis)?", "id": "Analisis Performa Batas Toleransi Ekstrem." },
  { "en": "Apa Itu Analisis Monte Carlo (Monte Carlo Analysis)?", "id": "Analisis Statistik Variasi Komponen." },
  { "en": "Apa Itu SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Perangkat Lunak Simulasi Rangkaian Elektronik." },
  { "en": "Apa Kepanjangan SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program Simulasi Penekanan Sirkuit Terpadu." },
  { "en": "Jenis Analisis Apa Dilakukan SPICE?", "id": "Analisis DC, AC, Transien." },
  { "en": "Apa Itu Analisis DC (DC Analysis) SPICE?", "id": "Menghitung Titik Operasi DC Rangkaian." },
  { "en": "Apa Itu Analisis AC (AC Analysis) SPICE?", "id": "Menghitung Respon Frekuensi Rangkaian." },
  { "en": "Apa Itu Analisis Transien (Transient Analysis) SPICE?", "id": "Menghitung Respon Rangkaian Terhadap Waktu." },
  { "en": "Apa Itu Netlist (Netlist) SPICE?", "id": "Deskripsi Tekstual Rangkaian Untuk SPICE." },
  { "en": "Apa Itu Model (Model) Komponen SPICE?", "id": "Representasi Matematis Perilaku Komponen." },
  { "en": "Apa Itu Temperatur Sweep (Sapuan Suhu) SPICE?", "id": "Simulasi Perilaku Rangkaian Berbagai Suhu." },
  { "en": "Apa Itu Parameter Sweep (Sapuan Parameter) SPICE?", "id": "Simulasi Variasi Nilai Komponen." },
  { "en": "Apa Itu Listrik Dasar (Basic Electricity)?", "id": "Konsep Fundamental Fenomena Listrik." },
  { "en": "Apa Itu Muatan (Charge) Listrik?", "id": "Sifat Dasar Materi (Positif/Negatif)." },
  { "en": "Apa Satuan Muatan (Charge) Listrik?", "id": "Coulomb (C)." },
  { "en": "Berapa Muatan Elementer (Elementary Charge) (e)?", "id": "Muatan Satu Elektron/Proton." },
  { "en": "Apa Itu Hukum Kekekalan Muatan (Conservation of Charge)?", "id": "Muatan Total Sistem Terisolasi Konstan." },
  { "en": "Apa Itu Medan Listrik (Electric Field) Seragam?", "id": "Medan Listrik Sama Arah, Besarnya." },
  { "en": "Bagaimana Medan Listrik (Electric Field) Dalam Konduktor Ideal?", "id": "Medan Listrik Di Dalamnya Nol." },
  { "en": "Apa Itu Kandang Faraday (Faraday Cage)?", "id": "Selubung Konduktif Menghalangi Medan Listrik." },
  { "en": "Apa Itu Induksi Elektrostatis (Electrostatic Induction)?", "id": "Pemisahan Muatan Konduktor Medan Listrik." },
  { "en": "Apa Itu Generator Van de Graaff (Van de Graaff)?", "id": "Penghasil Tegangan Statis Sangat Tinggi." },
  { "en": "Apa Itu Petir (Lightning)?", "id": "Pelepasan Listrik Statis Skala Besar Atmosfer." },
  { "en": "Apa Itu Penangkal Petir (Lightning Rod)?", "id": "Melindungi Bangunan Dari Sambaran Langsung." },
  { "en": "Apa Itu Arus Konveksi (Convection Current)?", "id": "Aliran Muatan Akibat Pergerakan Medium." },
  { "en": "Apa Itu Arus Konduksi (Conduction Current)?", "id": "Aliran Muatan Lewat Konduktor (Elektron Bebas)." },
  { "en": "Apa Itu Mobilitas (Mobility) Elektron?", "id": "Kemudahan Elektron Bergerak Medan Listrik." },
  { "en": "Apa Itu Kecepatan Hanyut (Drift Velocity) Elektron?", "id": "Kecepatan Rata-Rata Elektron Akibat Medan." },
  { "en": "Apa Itu Efek Joule (Joule Effect) (Pemanasan)?", "id": "Energi Listrik Jadi Panas Hambatan." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Efek Seebeck." },
  { "en": "Apa Itu Efek Seebeck (Seebeck Effect)?", "id": "Beda Suhu Hasilkan Tegangan Sambungan Logam." },
  { "en": "Apa Itu Efek Peltier (Peltier Effect)?", "id": "Arus Listrik Sebabkan Transfer Panas Sambungan." },
  { "en": "Apa Itu Bahan Superkonduktor (Superconductor)?", "id": "Bahan Hambatan Nol Dibawah Suhu Kritis." },
  { "en": "Apa Itu Suhu Kritis (Critical Temperature) (Tc)?", "id": "Suhu Transisi Menjadi Superkonduktor." },
  { "en": "Apa Itu Medan Magnet Kritis (Critical Magnetic Field)?", "id": "Medan Magnet Hancurkan Superkonduktivitas." },
  { "en": "Apa Itu Arus Kritis (Critical Current) Superkonduktor?", "id": "Arus Maksimum Superkonduktor." },
  { "en": "Apa Itu Efek Meissner (Meissner Effect)?", "id": "Penolakan Sempurna Medan Magnet Superkonduktor." },
  { "en": "Apa Itu Magnet Permanen (Permanent Magnet)?", "id": "Bahan Menghasilkan Medan Magnet Sendiri." },
  { "en": "Apa Itu Kutub (Pole) Magnet?", "id": "Daerah Medan Magnet Terkuat (Utara/Selatan)." },
  { "en": "Bagaimana Interaksi Kutub (Pole) Magnet?", "id": "Sama Tolak Menolak, Beda Tarik Menarik." },
  { "en": "Apa Itu Garis Medan Magnet (Magnetic Field Lines)?", "id": "Garis Arah Medan Magnet (Utara Ke Selatan)." },
  { "en": "Apakah Garis Medan Magnet (Magnetic Field Lines) Tertutup?", "id": "Ya, Selalu Membentuk Loop Tertutup." },
  { "en": "Apa Itu Fluks (Flux) Magnetik?", "id": "Ukuran Jumlah Medan Magnet Menembus Luas." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan Fluks (Flux Density) Magnetik (B)?", "id": "Fluks Magnet Per Satuan Luas." },
  { "en": "Apa Satuan Kerapatan Fluks (Flux Density) Magnetik (B)?", "id": "Tesla (T)." },
  { "en": "Apa Itu Gaya Magnetik (Magnetic Force) Kawat Berarus?", "id": "Gaya Dialami Kawat Medan Magnet (F=BIL)." },
  { "en": "Aturan Apa Menentukan Arah Gaya Magnetik?", "id": "Aturan Tangan Kanan (Kaidah Tangan Kanan)." },
  { "en": "Apa Itu Gaya Lorentz (Lorentz Force)?", "id": "Gaya Total Listrik, Magnet Muatan Bergerak." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan GGL Akibat Perubahan Fluks." },
  { "en": "Apa Itu Hukum Faraday (Faraday's Law) Induksi?", "id": "Besar GGL Induksi Sebanding Laju Fluks." },
  { "en": "Apa Itu Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Penyebabnya." },
  { "en": "Apa Itu Induktansi Diri (Self-Inductance)?", "id": "GGL Induksi Diri Akibat Perubahan Arus." },
  { "en": "Apa Itu Induktansi Mutual (Mutual Inductance)?", "id": "GGL Induksi Kumparan Akibat Kumparan Lain." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Induksi Berputar Dalam Konduktor." },
  { "en": "Dimana Arus Eddy (Eddy Current) Tidak Diinginkan?", "id": "Inti Transformator (Sebabkan Pemanasan)." },
  { "en": "Dimana Arus Eddy (Eddy Current) Diinginkan?", "id": "Pengereman Induksi, Pemanasan Induksi." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Cenderung Mengalir Permukaan." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Terganggu Konduktor Dekat." },
  { "en": "Apa Itu Elektrolisis (Electrolysis)?", "id": "Penguraian Senyawa Kimia Arus Listrik." },
  { "en": "Apa Itu Hukum Faraday (Faraday's Law) Elektrolisis?", "id": "Massa Zat Terendapkan/Terlarut Proporsional Muatan." },
  { "en": "Apa Itu Sel Galvanik (Galvanic Cell)?", "id": "Hasilkan Listrik Dari Reaksi Kimia Spontan." },
  { "en": "Apa Itu Baterai (Battery)?", "id": "Satu Atau Lebih Sel Galvanik." },
  { "en": "Apa Itu Baterai Primer (Primary Battery)?", "id": "Baterai Sekali Pakai." },
  { "en": "Apa Itu Baterai Sekunder (Secondary Battery)?", "id": "Baterai Dapat Diisi Ulang." },
  { "en": "Contoh Baterai Primer (Primary Battery)?", "id": "Baterai Alkaline, Baterai Zinc-Carbon." },
  { "en": "Contoh Baterai Sekunder (Secondary Battery)?", "id": "Baterai Aki (Lead-Acid), Lithium-Ion." },
  { "en": "Apa Itu Elektrolit (Electrolyte)?", "id": "Larutan/Pasta Konduktif Ion." },
  { "en": "Apa Itu Anoda (Anode) Baterai?", "id": "Elektroda Negatif (Tempat Oksidasi)." },
  { "en": "Apa Itu Katoda (Cathode) Baterai?", "id": "Elektroda Positif (Tempat Reduksi)." },
  { "en": "Apa Itu Kapasitas (Capacity) Baterai?", "id": "Jumlah Muatan Dapat Disimpan (Ah)." },
  { "en": "Apa Satuan Kapasitas (Capacity) Baterai?", "id": "Ampere-Hour (Ah)." },
  { "en": "Apa Itu Tegangan Nominal (Nominal Voltage) Baterai?", "id": "Tegangan Rata-Rata Baterai." },
  { "en": "Apa Itu Siklus Hidup (Cycle Life) Baterai Isi Ulang?", "id": "Jumlah Siklus Isi-Kosong Maksimal." },
  { "en": "Apa Itu Self-Discharge (Pengosongan Diri) Baterai?", "id": "Kehilangan Muatan Baterai Saat Disimpan." },
  { "en": "Apa Itu Sel Bahan Bakar (Fuel Cell)?", "id": "Hasilkan Listrik Reaksi Kimia Kontinu." },
  { "en": "Apa Bahan Bakar (Fuel) Umum Sel Bahan Bakar?", "id": "Hidrogen (H₂)." },
  { "en": "Apa Produk Sampingan (Byproduct) Sel Bahan Bakar Hidrogen?", "id": "Air (H₂O)." },
  { "en": "Apa Itu Rangkaian RC (Resistor Kapasitor)?", "id": "Rangkaian Resistor, Kapasitor." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RC (τ)?", "id": "τ = R * C." },
  { "en": "Apa Itu Respon Pengisian (Charging Response) Kapasitor?", "id": "Tegangan Kapasitor Naik Eksponensial." },
  { "en": "Apa Itu Respon Pengosongan (Discharging Response) Kapasitor?", "id": "Tegangan Kapasitor Turun Eksponensial." },
  { "en": "Apa Itu Rangkaian RL (Resistor Induktor)?", "id": "Rangkaian Resistor, Induktor." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RL (τ)?", "id": "τ = L / R." },
  { "en": "Apa Itu Respon Arus (Current Response) Induktor?", "id": "Arus Induktor Naik/Turun Eksponensial." },
  { "en": "Apa Itu Rangkaian RLC (Resistor Induktor Kapasitor)?", "id": "Rangkaian Mengandung R, L, C." },
  { "en": "Apa Itu Frekuensi Resonansi (Resonance Frequency) Alami RLC?", "id": "Frekuensi Osilasi Tanpa Redaman." },
  { "en": "Apa Itu Redaman (Damping) Rangkaian RLC?", "id": "Pengurangan Amplitudo Osilasi (Oleh R)." },
  { "en": "Apa Itu Osilasi Teredam (Damped Oscillation)?", "id": "Osilasi Amplitudo Mengecil Seiring Waktu." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Kompleks Sinyal Sinusoidal." },
  { "en": "Apa Keuntungan Menggunakan Fasor (Phasor)?", "id": "Menyederhanakan Analisis Rangkaian AC." },
  { "en": "Apa Itu Impedansi (Impedance) Kompleks (Z)?", "id": "Z = R + jX." },
  { "en": "Apa Itu Reaktansi (Reactance) (X)?", "id": "Bagian Imajiner Impedansi." },
  { "en": "Apa Reaktansi Induktif (Inductive Reactance) (Xl)?", "id": "jωL." },
  { "en": "Apa Reaktansi Kapasitif (Capacitive Reactance) (Xc)?", "id": "1 / (jωC) = -j / (ωC)." },
  { "en": "Apa Itu Admitansi (Admittance) Kompleks (Y)?", "id": "Y = G + jB." },
  { "en": "Apa Itu Konduktansi (Conductance) (G)?", "id": "Bagian Riil Admitansi." },
  { "en": "Apa Itu Suseptansi (Susceptance) (B)?", "id": "Bagian Imajiner Admitansi." },
  { "en": "Apa Itu Daya Rata-Rata (Average Power) (P)?", "id": "Daya Nyata Diserap Rangkaian." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Osilasi Komponen Reaktif." },
  { "en": "Apa Itu Daya Semu (Apparent Power) (S)?", "id": "Magnitudo Daya Kompleks (|S|)." },
  { "en": "Apa Itu Daya Kompleks (Complex Power) (S)?", "id": "S = P + jQ = V * I*." },
  { "en": "Apa Itu Faktor Daya (Power Factor) (PF)?", "id": "Cos(θ) = P / |S|." },
  { "en": "Apa Itu Resonansi (Resonance) Seri?", "id": "Impedansi Minimum (Z = R)." },
  { "en": "Apa Itu Resonansi (Resonance) Paralel?", "id": "Impedansi Maksimum (Z = R)." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Selektif Frekuensi." },
  { "en": "Apa Itu Filter Lolos Rendah (Low-Pass Filter)?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter Lolos Tinggi (High-Pass Filter)?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Lolos Pita (Band-Pass Filter)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Tolak Pita (Band-Stop Filter)?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Filter (-3dB)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Filter?", "id": "Lebar Rentang Frekuensi Dilewatkan/Ditolak." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Induksi Mutual." },
  { "en": "Apa Itu Rasio Transformasi (Transformation Ratio) (a)?", "id": "Np / Ns." },
  { "en": "Apa Itu Trafo Ideal (Ideal Transformer)?", "id": "Tanpa Rugi, Kopling Sempurna." },
  { "en": "Apa Itu Sistem Tiga Fasa (Three-Phase System)?", "id": "Tiga Sumber AC Beda Fasa 120°." },
  { "en": "Apa Itu Koneksi Bintang (Wye/Y)?", "id": "Terhubung Ke Titik Netral Bersama." },
  { "en": "Apa Itu Koneksi Delta (Delta/Δ)?", "id": "Terhubung Membentuk Segitiga." },
  { "en": "Apa Itu Tegangan Saluran (Line Voltage) (V_L)?", "id": "Tegangan Antar Dua Saluran Fasa." },
  { "en": "Apa Itu Tegangan Fasa (Phase Voltage) (V_Ph)?", "id": "Tegangan Antar Saluran Fasa, Netral (Y)." },
  { "en": "Apa Itu Arus Saluran (Line Current) (I_L)?", "id": "Arus Mengalir Di Saluran Fasa." },
  { "en": "Apa Itu Arus Fasa (Phase Current) (I_Ph)?", "id": "Arus Mengalir Lewat Beban Fasa." },
  { "en": "Apa Rumus Daya (Power) Total Tiga Fasa?", "id": "P = √3 * V_L * I_L * PF." },
  { "en": "Apa Itu Deret Fourier (Fourier Series)?", "id": "Representasi Sinyal Periodik Jumlahan Sinusoid." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Representasi Frekuensi Sinyal Non-Periodik." },
  { "en": "Apa Itu Spektrum Frekuensi (Frequency Spectrum)?", "id": "Plot Amplitudo/Fasa Vs Frekuensi." },
  { "en": "Apa Itu Transformasi Laplace (Laplace Transform)?", "id": "Alat Analisis Rangkaian Domain-s." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Rasio Output/Input Domain-s." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Nilai 's' Penyebut Nol." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Nilai 's' Pembilang Nol." },
  { "en": "Apa Hubungan Pole (Kutub), Kestabilan (Stability)?", "id": "Stabil Jika Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Bagaimana Dapat Respon Frekuensi (Frequency Response) Dari H(s)?", "id": "Substitusi s = jω." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Respon Frekuensi." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Rangkaian Dengan Input, Output Port." },
  { "en": "Parameter Apa Digunakan Jaringan Dua Port?", "id": "Parameter Z, Y, h, ABCD, S." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Jaringan Analisis Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna (On/Off)." },
  { "en": "Tegangan Maju (Forward Voltage) Dioda Silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Apa Itu Daerah Operasi (Operating Region) Transistor?", "id": "Cutoff, Aktif, Saturasi." },
  { "en": "Transistor Apa Terkontrol Arus (Current Controlled)?", "id": "Transistor BJT (Bipolar Junction Transistor)." },
  { "en": "Transistor Apa Terkontrol Tegangan (Voltage Controlled)?", "id": "Transistor FET (Field Effect Transistor) (JFET, MOSFET)." },
  { "en": "Apa Itu Amplifier (Amplifier)?", "id": "Rangkaian Penguat Sinyal." },
  { "en": "Apa Itu Gain (Gain)?", "id": "Rasio Penguatan Sinyal." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Output Ke Input." },
  { "en": "Apa Efek Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Gain, Mengurangi Distorsi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Osilasi (Oscillation)?", "id": "Kriteria Barkhausen (Gain Loop=1, Fasa=0)." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Selektif Frekuensi." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Dasar Sirkuit Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Digital Dasar." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Counter (Counter)?", "id": "Rangkaian Penghitung Pulsa." },
  { "en": "Apa Itu Multiplexer (Multiplexer)?", "id": "Pemilih Data Input." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer)?", "id": "Distributor Data Output." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Pengubah Input Aktif Ke Kode Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Pengubah Kode Biner Ke Output Aktif." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sirkuit Rangkaian Cetak Fleksibel (FPC)?", "id": "Flexible Printed Circuit (PCB Fleksibel)." },
  { "en": "Apa Keuntungan FPC (Flexible Printed Circuit)?", "id": "Fleksibel, Ringan, Hemat Ruang." },
  { "en": "Apa Itu Konektor ZIF (Zero Insertion Force)?", "id": "Konektor FPC Tanpa Gaya Pemasangan." },
  { "en": "Apa Itu Pengujian Flying Probe (Flying Probe Test)?", "id": "Pengujian PCB Otomatis Tanpa Fixture." },
  { "en": "Apa Itu Pengujian Batas Pindai (Boundary Scan)?", "id": "Metode Pengujian IC (Integrated Circuit) Terpasang (JTAG)." },
  { "en": "Apa Kepanjangan JTAG (Joint Test Action Group)?", "id": "Standar Pengujian Batas Pindai." },
  { "en": "Apa Fungsi JTAG (Joint Test Action Group)?", "id": "Pengujian, Debugging, Pemrograman IC." },
  { "en": "Apa Itu Penguat Logaritmik (Logarithmic Amplifier)?", "id": "Output Proporsional Logaritma Input." },
  { "en": "Dimana Penguat Logaritmik (Logarithmic Amplifier) Digunakan?", "id": "Pengukuran Sinyal Rentang Dinamis Lebar." },
  { "en": "Apa Itu Penguat Antilogaritmik (Antilog Amplifier)?", "id": "Output Proporsional Eksponensial Input." },
  { "en": "Apa Itu Penguat Terprogram (Programmable Gain Amplifier)?", "id": "PGA (Programmable Gain Amplifier)." },
  { "en": "Apa Kepanjangan PGA (Programmable Gain Amplifier)?", "id": "Penguat Penguatan Terprogram." },
  { "en": "Apa Fungsi PGA (Programmable Gain Amplifier)?", "id": "Mengatur Penguatan Secara Digital." },
  { "en": "Apa Itu Atenuator (Attenuator) Terprogram?", "id": "Pelemah Sinyal Dapat Diatur Digital." },
  { "en": "Apa Itu Penguat Isolasi (Isolation Amplifier)?", "id": "Penguat Dengan Isolasi Galvanik Input-Output." },
  { "en": "Mengapa Perlu Penguat Isolasi (Isolation Amplifier)?", "id": "Keamanan, Menghilangkan Ground Loop Medis." },
  { "en": "Apa Itu Modulator (Modulator) Optik?", "id": "Mengubah Sinyal Listrik Ke Cahaya Termodulasi." },
  { "en": "Apa Itu Demodulator (Demodulator) Optik?", "id": "Mengubah Cahaya Termodulasi Ke Listrik." },
  { "en": "Apa Itu Transceiver (Transceiver) Optik?", "id": "Gabungan Pemancar, Penerima Optik." },
  { "en": "Apa Itu Multiplexer (Multiplexer) Optik (MUX)?", "id": "Menggabungkan Banyak Panjang Gelombang." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) Optik (DEMUX)?", "id": "Memisahkan Banyak Panjang Gelombang." },
  { "en": "Apa Itu Penguat Serat Optik (Optical Fiber Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung Di Serat." },
  { "en": "Contoh Penguat Serat Optik (Optical Fiber Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Saklar Optik (Optical Switch)?", "id": "Mengalihkan Jalur Sinyal Cahaya." },
  { "en": "Apa Itu Sirkulator (Circulator) Optik?", "id": "Mengalirkan Cahaya Arah Tertentu Antar Port." },
  { "en": "Apa Itu Isolator (Isolator) Optik?", "id": "Melewatkan Cahaya Hanya Satu Arah." },
  { "en": "Apa Itu Filter Optik (Optical Filter)?", "id": "Melewatkan Panjang Gelombang Tertentu." },
  { "en": "Apa Itu Interferometer (Interferometer)?", "id": "Alat Ukur Berbasis Interferensi Gelombang." },
  { "en": "Contoh Interferometer (Interferometer)?", "id": "Michelson, Fabry-Perot." },
  { "en": "Apa Itu Giroskop Serat Optik (FOG)?", "id": "Fiber Optic Gyroscope (FOG)." },
  { "en": "Apa Kepanjangan FOG (Fiber Optic Gyroscope)?", "id": "Giroskop Serat Optik." },
  { "en": "Apa Prinsip Kerja FOG (Fiber Optic Gyroscope)?", "id": "Efek Sagnac (Pergeseran Fasa Cahaya)." },
  { "en": "Apa Itu Sensor Serat Optik (Fiber Optic Sensor)?", "id": "Sensor Gunakan Serat Optik Ukur Fisik." },
  { "en": "Keunggulan Sensor Serat Optik (Fiber Optic Sensor)?", "id": "Kebal EMI, Aman Lingkungan Berbahaya." },
  { "en": "Apa Itu Sensor FBG (Fiber Bragg Grating)?", "id": "Sensor Suhu, Regangan Berbasis Kisi Serat." },
  { "en": "Apa Kepanjangan FBG (Fiber Bragg Grating)?", "id": "Kisi Bragg Serat." },
  { "en": "Apa Itu Kabel Bawah Laut (Submarine Cable)?", "id": "Kabel Komunikasi (Serat Optik) Dasar Laut." },
  { "en": "Apa Itu Repeater (Repeater) Bawah Laut?", "id": "Penguat Sinyal Optik Kabel Bawah Laut." },
  { "en": "Apa Itu Fusi Nuklir (Nuclear Fusion)?", "id": "Penggabungan Inti Atom Hasilkan Energi." },
  { "en": "Apa Reaksi Fusi (Fusion Reaction) Di Matahari?", "id": "Fusi Hidrogen Menjadi Helium." },
  { "en": "Apa Itu Plasma (Plasma)?", "id": "Wujud Materi Keempat (Gas Terionisasi)." },
  { "en": "Apa Itu Tokamak (Tokamak)?", "id": "Desain Reaktor Fusi Bentuk Donat." },
  { "en": "Apa Itu Stellarator (Stellarator)?", "id": "Desain Reaktor Fusi Bentuk Kompleks." },
  { "en": "Tantangan Utama Reaktor Fusi (Fusion Reactor)?", "id": "Menahan Plasma Sangat Panas Stabil." },
  { "en": "Apa Itu Radioaktivitas (Radioactivity)?", "id": "Peluruhan Spontan Inti Atom Tak Stabil." },
  { "en": "Apa Tiga Jenis Radiasi (Radiation) Utama?", "id": "Alfa (α), Beta (β), Gama (γ)." },
  { "en": "Apa Itu Partikel Alfa (Alpha Particle)?", "id": "Inti Helium (Dua Proton, Dua Neutron)." },
  { "en": "Apa Itu Partikel Beta (Beta Particle)?", "id": "Elektron Atau Positron Energi Tinggi." },
  { "en": "Apa Itu Sinar Gama (Gamma Ray)?", "id": "Radiasi Elektromagnetik Energi Sangat Tinggi." },
  { "en": "Mana Daya Tembus Radiasi Paling Rendah?", "id": "Radiasi Alfa (α)." },
  { "en": "Mana Daya Tembus Radiasi Paling Tinggi?", "id": "Radiasi Gama (γ)." },
  { "en": "Apa Itu Waktu Paruh (Half-Life)?", "id": "Waktu Separuh Inti Radioaktif Meluruh." },
  { "en": "Apa Itu Detektor Radiasi (Radiation Detector)?", "id": "Alat Ukur Keberadaan, Intensitas Radiasi." },
  { "en": "Contoh Detektor Radiasi (Radiation Detector)?", "id": "Pencacah Geiger-Muller, Detektor Sintilasi." },
  { "en": "Apa Itu Dosis Radiasi (Radiation Dose)?", "id": "Jumlah Energi Radiasi Diserap Materi." },
  { "en": "Apa Satuan Dosis Serap (Absorbed Dose)?", "id": "Gray (Gy)." },
  { "en": "Apa Satuan Dosis Ekuivalen (Equivalent Dose)?", "id": "Sievert (Sv)." },
  { "en": "Apa Itu Proteksi Radiasi (Radiation Protection)?", "id": "Tindakan Minimalisasi Paparan Radiasi." },
  { "en": "Tiga Prinsip Proteksi Radiasi (Radiation Protection)?", "id": "Waktu, Jarak, Perisai." },
  { "en": "Apa Itu Perisai (Shielding) Radiasi?", "id": "Bahan Penyerap Radiasi (Timbal, Beton)." },
  { "en": "Apa Itu Reaksi Fisi (Fission Reaction) Nuklir?", "id": "Pembelahan Inti Atom Berat." },
  { "en": "Apa Itu Reaksi Berantai (Chain Reaction)?", "id": "Neutron Fisi Picu Fisi Berikutnya." },
  { "en": "Apa Itu Massa Kritis (Critical Mass)?", "id": "Massa Minimum Reaksi Berantai Berkelanjutan." },
  { "en": "Apa Itu Reaktor Nuklir (Nuclear Reactor)?", "id": "Pembangkit Energi Fisi Terkendali." },
  { "en": "Apa Fungsi Moderator (Moderator) Reaktor?", "id": "Memperlambat Neutron Cepat." },
  { "en": "Apa Fungsi Batang Kendali (Control Rod)?", "id": "Menyerap Neutron (Kontrol Laju Reaksi)." },
  { "en": "Apa Fungsi Pendingin (Coolant) Reaktor?", "id": "Memindahkan Panas Dari Inti Reaktor." },
  { "en": "Apa Itu Bahan Bakar (Fuel) Nuklir?", "id": "Material Fisil (Uranium, Plutonium)." },
  { "en": "Apa Itu Pengayaan Uranium (Uranium Enrichment)?", "id": "Meningkatkan Konsentrasi Isotop U-235." },
  { "en": "Apa Itu Limbah Nuklir (Nuclear Waste)?", "id": "Sisa Bahan Bakar Radioaktif." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir (PLTN)?", "id": "Pembangkit Listrik Energi Fisi Nuklir." },
  { "en": "Apa Itu Siklus Bahan Bakar (Fuel Cycle) Nuklir?", "id": "Proses Dari Penambangan Hingga Limbah." },
  { "en": "Apa Itu Kecelakaan Chernobyl (Chernobyl Accident)?", "id": "Kecelakaan Reaktor Nuklir Parah 1986." },
  { "en": "Apa Itu Kecelakaan Fukushima (Fukushima Accident)?", "id": "Kecelakaan Nuklir Akibat Tsunami 2011." },
  { "en": "Apa Itu Keamanan Nuklir (Nuclear Security)?", "id": "Pencegahan Akses Ilegal Material Nuklir." },
  { "en": "Apa Itu Keselamatan Nuklir (Nuclear Safety)?", "id": "Pencegahan Kecelakaan Operasi Reaktor." },
  { "en": "Apa Itu Badan Tenaga Nuklir Internasional (IAEA)?", "id": "International Atomic Energy Agency (IAEA)." },
  { "en": "Apa Kepanjangan IAEA (International Atomic Energy Agency)?", "id": "Badan Tenaga Atom Internasional." },
  { "en": "Apa Itu Radioisotop (Radioisotope)?", "id": "Isotop Radioaktif Suatu Unsur." },
  { "en": "Aplikasi Radioisotop (Radioisotope) Medis?", "id": "Diagnostik (Pencitraan), Terapi (Radioterapi)." },
  { "en": "Aplikasi Radioisotop (Radioisotope) Industri?", "id": "Pengukur Ketebalan, Sterilisasi, Radiografi." },
  { "en": "Apa Itu Penanggalan Radiokarbon (Radiocarbon Dating)?", "id": "Menentukan Usia Bahan Organik (Karbon-14)." },
  { "en": "Apa Itu Iradiasi (Irradiation) Pangan?", "id": "Penggunaan Radiasi Awetkan Makanan." },
  { "en": "Apa Itu Akselerator Partikel (Particle Accelerator)?", "id": "Mesin Percepat Partikel Bermuatan." },
  { "en": "Contoh Akselerator Partikel (Particle Accelerator)?", "id": "Siklotron (Cyclotron), Sinkrotron (Synchrotron), Linac." },
  { "en": "Apa Kepanjangan Linac (Linear Accelerator)?", "id": "Akselerator Linear." },
  { "en": "Apa Fungsi Akselerator Partikel (Particle Accelerator)?", "id": "Penelitian Fisika, Produksi Radioisotop, Terapi." },
  { "en": "Apa Itu Large Hadron Collider (LHC)?", "id": "Akselerator Partikel Terbesar Dunia (CERN)." },
  { "en": "Apa Kepanjangan LHC (Large Hadron Collider)?", "id": "Penumbuk Hadron Besar." },
  { "en": "Apa Kepanjangan CERN (European Organization for Nuclear Research)?", "id": "Organisasi Eropa Riset Nuklir." },
  { "en": "Partikel Apa Ditemukan Di LHC?", "id": "Boson Higgs (Higgs Boson)." },
  { "en": "Apa Itu Sinar Kosmik (Cosmic Ray)?", "id": "Partikel Energi Tinggi Dari Luar Angkasa." },
  { "en": "Apa Itu Materi Gelap (Dark Matter)?", "id": "Materi Tak Terlihat Interaksi Gravitasi." },
  { "en": "Apa Itu Energi Gelap (Dark Energy)?", "id": "Penyebab Percepatan Ekspansi Alam Semesta." },
  { "en": "Apa Itu Teori String (String Theory)?", "id": "Teori Fisika (Partikel Adalah Getaran String)." },
  { "en": "Apa Itu Mekanika Statistik (Statistical Mechanics)?", "id": "Menghubungkan Sifat Mikroskopik, Makroskopik." },
  { "en": "Apa Itu Ansambel (Ensemble) Statistik?", "id": "Kumpulan Sistem Mikroskopik Identik." },
  { "en": "Apa Itu Distribusi Maxwell-Boltzmann (Maxwell-Boltzmann)?", "id": "Distribusi Kecepatan Partikel Gas Ideal." },
  { "en": "Apa Itu Fungsi Partisi (Partition Function)?", "id": "Kuantitas Pusat Mekanika Statistik." },
  { "en": "Apa Itu Kapasitas Panas (Heat Capacity)?", "id": "Ukuran Kemampuan Zat Simpan Panas." },
  { "en": "Apa Itu Konduktivitas Termal (Thermal Conductivity)?", "id": "Kemampuan Bahan Hantarkan Panas." },
  { "en": "Apa Itu Difusi (Diffusion)?", "id": "Pergerakan Partikel Konsentrasi Tinggi Rendah." },
  { "en": "Apa Itu Osmosis (Osmosis)?", "id": "Difusi Pelarut Lewati Membran Semipermeabel." },
  { "en": "Apa Itu Tekanan Osmotik (Osmotic Pressure)?", "id": "Tekanan Hentikan Aliran Osmosis." },
  { "en": "Apa Itu Elektronika Analog (Analog Electronics)?", "id": "Elektronika Berbasis Sinyal Kontinu." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Elektronika Berbasis Sinyal Diskrit (0, 1)." },
  { "en": "Apa Keuntungan Elektronika Digital?", "id": "Kebal Noise, Mudah Diproses, Disimpan." },
  { "en": "Apa Keuntungan Elektronika Analog?", "id": "Representasi Sinyal Dunia Nyata Langsung." },
  { "en": "Apa Itu Sinyal (Signal)?", "id": "Besaran Fisik Bawa Informasi." },
  { "en": "Apa Itu Noise (Derau) Elektronik?", "id": "Sinyal Acak Tak Diinginkan Mengganggu." },
  { "en": "Sumber Noise (Derau) Elektronik?", "id": "Termal, Shot Noise, Flicker Noise." },
  { "en": "Apa Itu Noise Termal (Thermal Noise)?", "id": "Noise Akibat Gerak Termal Elektron (Johnson)." },
  { "en": "Apa Itu Shot Noise (Shot Noise)?", "id": "Noise Akibat Sifat Diskrit Muatan Listrik." },
  { "en": "Apa Itu Flicker Noise (Flicker Noise) (1/f Noise)?", "id": "Noise Frekuensi Rendah Perangkat Semikonduktor." },
  { "en": "Apa Itu Rasio Sinyal-Derau (SNR)?", "id": "Ukuran Kekuatan Sinyal Relatif Terhadap Derau." },
  { "en": "Apa Itu Distorsi (Distortion)?", "id": "Perubahan Bentuk Sinyal Tak Diinginkan." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi Sistem." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian Terhadap Arus AC." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Transfer Daya Maksimal." },
  { "en": "Apa Itu Ground (Ground) / Tanah?", "id": "Titik Referensi Tegangan Nol Rangkaian." },
  { "en": "Apa Itu Ground Loop (Loop Tanah)?", "id": "Jalur Ground Membentuk Loop (Sumber Noise)." },
  { "en": "Apa Itu Shielding (Perisaian) EMI?", "id": "Melindungi Rangkaian Interferensi Elektromagnetik." },
  { "en": "Bahan Apa Digunakan Shielding (Perisaian)?", "id": "Bahan Konduktif (Logam)." },
  { "en": "Apa Itu Ferrite Bead (Manik Ferit)?", "id": "Komponen Penekan Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor Bypass (Bypass Capacitor)?", "id": "Kapasitor Shunt Frekuensi Tinggi Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Bypass (Bypass Capacitor)?", "id": "Menyediakan Jalur Impedansi Rendah Noise HF." },
  { "en": "Apa Itu Kapasitor Decoupling (Decoupling Capacitor)?", "id": "Menstabilkan Catu Daya Lokal IC." },
  { "en": "Dimana Kapasitor Decoupling (Decoupling Capacitor) Dipasang?", "id": "Sangat Dekat Dengan Pin Daya IC." },
  { "en": "Nilai Kapasitor Decoupling (Decoupling Capacitor) Umum?", "id": "0.1 µF (100 nF) Keramik." },
  { "en": "Apa Itu Layout (Tata Letak) PCB Yang Baik?", "id": "Meminimalkan Noise, Interferensi, Masalah Lain." },
  { "en": "Mengapa Jalur Ground (Ground Trace) Harus Lebar?", "id": "Mengurangi Impedansi Jalur Ground." },
  { "en": "Mengapa Hindari Sudut 90 Derajat Jalur PCB?", "id": "Menyebabkan Refleksi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Crosstalk (Crosstalk) PCB?", "id": "Induksi Sinyal Antar Jalur Berdekatan." },
  { "en": "Bagaimana Mengurangi Crosstalk (Crosstalk)?", "id": "Jaga Jarak Antar Jalur, Gunakan Ground Plane." },
  { "en": "Apa Itu Power Integrity (Integritas Daya)?", "id": "Kualitas Distribusi Daya Di PCB." },
  { "en": "Apa Itu Signal Integrity (Integritas Sinyal)?", "id": "Kualitas Sinyal Listrik Di PCB." },
  { "en": "Masalah Signal Integrity (Integritas Sinyal) Umum?", "id": "Refleksi, Ringing, Ground Bounce." },
  { "en": "Apa Itu Ringing (Deringan) Sinyal?", "id": "Osilasi Sinyal Setelah Transisi Cepat." },
  { "en": "Apa Itu Ground Bounce (Pantulan Ground)?", "id": "Fluktuasi Tegangan Ground Saat Switching Cepat." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf)?", "id": "Jatuh Tegangan Dioda Saat Konduksi." },
  { "en": "Apa Itu Arus Bocor Balik (Reverse Leakage Current)?", "id": "Arus Kecil Saat Dioda Bias Mundur." },
  { "en": "Apa Itu Tegangan Puncak Balik (PIV)?", "id": "Peak Inverse Voltage (PIV)." },
  { "en": "Apa Kepanjangan PIV (Peak Inverse Voltage)?", "id": "Tegangan Puncak Terbalik Maksimum." },
  { "en": "Apa Terjadi Jika PIV (Peak Inverse Voltage) Terlampaui?", "id": "Dioda Rusak (Breakdown)." },
  { "en": "Apa Fungsi Dioda Zener (Zener Diode)?", "id": "Regulator Tegangan Paralel." },
  { "en": "Bagaimana Dioda Zener (Zener Diode) Dibiaskan?", "id": "Bias Mundur (Reverse Bias)." },
  { "en": "Apa Fungsi Dioda Schottky (Schottky Diode)?", "id": "Penyearah Cepat, Switching Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Varactor (Varactor Diode)?", "id": "Kapasitansi Diatur Tegangan Mundur." },
  { "en": "Apa Fungsi LED (Light Emitting Diode)?", "id": "Indikator Cahaya, Sumber Cahaya." },
  { "en": "Mengapa LED (Light Emitting Diode) Perlu Resistor Pembatas?", "id": "Membatasi Arus Agar Tidak Rusak." },
  { "en": "Apa Itu BJT (Bipolar Junction Transistor)?", "id": "Transistor Terkontrol Arus." },
  { "en": "Apa Tiga Daerah Operasi BJT?", "id": "Cutoff, Aktif, Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Sebagai Penguat?", "id": "Saat Berada Di Daerah Aktif." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Sebagai Saklar?", "id": "Saat Cutoff (Off) Atau Saturasi (On)." },
  { "en": "Apa Itu Penguatan Arus (Beta/hFE) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET (Field Effect Transistor) Paling Umum." },
  { "en": "Apa Keunggulan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Impedansi Input Tinggi, Daya Rendah." },
  { "en": "Apa Itu Tegangan Threshold (Vt) MOSFET?", "id": "Tegangan Gate Minimum Mulai Konduksi." },
  { "en": "Kapan MOSFET (Metal-Oxide-Semiconductor FET) Sebagai Saklar?", "id": "Tegangan Gate Di Atas/Bawah Threshold." },
  { "en": "Apa Itu RDS(on) (Resistansi On) MOSFET?", "id": "Resistansi Drain-Source Saat On Penuh." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Penguatan Loop Terbuka (Open Loop Gain) Op-Amp?", "id": "Sangat Besar (Ideal Tak Terhingga)." },
  { "en": "Bagaimana Penguatan Op-Amp Diatur?", "id": "Menggunakan Umpan Balik Negatif." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Tidak Membalik Fasa Sinyal Output." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Non-Inverting Gain = 1." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Buffer Impedansi (Input Tinggi, Output Rendah)." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio) Op-Amp?", "id": "Kemampuan Menolak Sinyal Sama Kedua Input." },
  { "en": "Apa Itu Slew Rate (Slew Rate) Op-Amp?", "id": "Kecepatan Perubahan Output Maksimal." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Op-Amp?", "id": "Rentang Frekuensi Operasi Penguat." },
  { "en": "Apa Itu Gain Bandwidth Product (GBP)?", "id": "Hasil Kali Gain Loop Tertutup, Bandwidth." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter RC (Resistor Kapasitor) Sederhana?", "id": "Filter Pasif Lolos Rendah/Tinggi." },
  { "en": "Apa Itu Filter LC (Induktor Kapasitor)?", "id": "Filter Pasif (Bisa Lebih Tajam)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Op-Amp." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC (Integrated Circuit) Untuk Timer, Osilator." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Nilai Digital." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah Bit Output (Akurasi Kuantisasi)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Nilai Digital Ke Sinyal Analog." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Komponen Utama Mikrokontroler?", "id": "CPU, Memori (RAM, Flash), Peripheral I/O." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Mikrokontroler Serbaguna (Input/Output)." },
  { "en": "Apa Kepanjangan GPIO (General Purpose Input Output)?", "id": "Input Output Tujuan Umum." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Output Mikrokontroler?", "id": "Mengatur Daya Output Sinyal Kotak." },
  { "en": "Apa Itu Antarmuka Serial (Serial Interface)?", "id": "Komunikasi Data Bit Per Bit." },
  { "en": "Contoh Antarmuka Serial (Serial Interface)?", "id": "UART, SPI, I2C." },
  { "en": "Apa Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Penerima-Pemancar Asinkron Universal." },
  { "en": "Apa Kepanjangan SPI (Serial Peripheral Interface)?", "id": "Antarmuka Periferal Serial." },
  { "en": "Apa Kepanjangan I2C (Inter-Integrated Circuit)?", "id": "Antar Sirkuit Terpadu." },
  { "en": "Apa Itu Interrupt (Interupsi) Mikrokontroler?", "id": "Sinyal Menghentikan Program Utama Sementara." },
  { "en": "Apa Fungsi Interrupt (Interupsi)?", "id": "Respon Cepat Kejadian Eksternal." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Pemantau Jika Program Macet." },
  { "en": "Apa Itu Bahasa Assembly (Assembly Language)?", "id": "Bahasa Pemrograman Level Rendah." },
  { "en": "Apa Itu Bahasa C/C++ (C/C++ Language)?", "id": "Bahasa Pemrograman Umum Mikrokontroler." },
  { "en": "Apa Itu Kompilator (Compiler)?", "id": "Penerjemah Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu Debugger (Debugger)?", "id": "Alat Bantu Cari Kesalahan Program." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Lingkungan Pengembangan Terpadu." },
  { "en": "Contoh IDE (Integrated Development Environment) Mikrokontroler?", "id": "Arduino IDE, MPLAB X, Keil MDK." },
  { "en": "Apa Itu Bootloader (Bootloader)?", "id": "Program Kecil Memuat Program Utama." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengukur Besaran Suhu." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor)?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Sensor Jarak (Distance Sensor)?", "id": "Mengukur Jarak Ke Objek." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Aktuator Putar Arus Searah." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Sudut." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Per Langkah Sudut." },
  { "en": "Apa Itu Driver (Driver) Motor?", "id": "Rangkaian Pengendali Arus Ke Motor." },
  { "en": "Mengapa Perlu Driver (Driver) Motor?", "id": "Mikrokontroler Tidak Cukup Kuat Langsung." },
  { "en": "Apa Itu H-Bridge (Jembatan H)?", "id": "Rangkaian Driver Kontrol Arah Motor DC." },
  { "en": "Apa Fungsi Kapasitor Bypass (Bypass Capacitor)?", "id": "Menyaring Noise Frekuensi Tinggi Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Kopling (Coupling Capacitor)?", "id": "Memblokir DC Melewatkan Sinyal AC." },
  { "en": "Apa Fungsi Kapasitor Smoothing (Smoothing Capacitor)?", "id": "Meratakan Tegangan DC Hasil Penyearah." },
  { "en": "Apa Nama Lain Kapasitor Smoothing (Smoothing Capacitor)?", "id": "Kapasitor Filter (Filter Capacitor)." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Kebocoran Dielektrik (Dielectric Leakage)?", "id": "Arus Kecil Lewat Isolator Kapasitor." },
  { "en": "Apa Itu Absorpsi Dielektrik (Dielectric Absorption)?", "id": "Efek Sisa Muatan Setelah Pengosongan." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Apa Inti (Core) Induktor?", "id": "Bahan Pusat Kumparan (Udara, Ferit)." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor) Induktor?", "id": "Ukuran Efisiensi Induktor (Rugi Rendah)." },
  { "en": "Apa Itu Self Resonant Frequency (SRF) Induktor?", "id": "Frekuensi Resonansi Parasitik Induktor." },
  { "en": "Apa Kepanjangan SRF (Self Resonant Frequency)?", "id": "Frekuensi Resonansi Diri." },
  { "en": "Di Atas SRF (Self Resonant Frequency), Induktor Bersifat Apa?", "id": "Bersifat Kapasitif (Akibat Parasitik)." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung Jalur PCB Hijau." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Label Teks Putih Komponen PCB." },
  { "en": "Apa Itu Annular Ring (Cincin Anular) Via?", "id": "Area Tembaga Sekitar Lubang Via." },
  { "en": "Apa Itu Tenting (Tenting) Via?", "id": "Menutup Lubang Via Dengan Solder Mask." },
  { "en": "Apa Itu Panelisasi (Panelization) PCB?", "id": "Menggabungkan Beberapa Desain PCB Satu Panel." },
  { "en": "Apa Itu V-Score (V-Score) Panelisasi?", "id": "Alur V Memudahkan Pemisahan PCB." },
  { "en": "Apa Itu Tab Routing (Tab Routing) Panelisasi?", "id": "Jembatan Kecil Menghubungkan PCB Panel." },
  { "en": "Apa Itu Footprint (Jejak Kaki) Komponen?", "id": "Pola Pad PCB Untuk Komponen." },
  { "en": "Apa Itu Library (Perpustakaan) Komponen PCB?", "id": "Kumpulan Skematik Simbol, Footprint." },
  { "en": "Apa Itu Ground Pour (Tuangan Ground) PCB?", "id": "Mengisi Area Kosong Dengan Ground Plane." },
  { "en": "Apa Itu Thermal Relief (Relief Termal) Pad?", "id": "Koneksi Pad Ke Plane (Mudah Solder)." },
  { "en": "Apa Itu Test Point (Titik Uji) PCB?", "id": "Pad Khusus Akses Pengujian Sirkuit." },
  { "en": "Apa Itu Fiducial Mark (Tanda Fidusia)?", "id": "Tanda Referensi Perakitan Otomatis PCB." },
  { "en": "Apa Itu Solderability (Kemampuan Solder)?", "id": "Kemudahan Permukaan Logam Dibasahi Solder." },
  { "en": "Apa Itu Wetting (Pembasahan) Solder?", "id": "Solder Cair Menyebar Rata Permukaan." },
  { "en": "Apa Itu Non-Wetting (Tidak Membasahi) Solder?", "id": "Solder Tidak Menyebar Permukaan." },
  { "en": "Apa Itu Dewetting (Pembasahan Kembali) Solder?", "id": "Solder Awalnya Membasahi Lalu Menarik Diri." },
  { "en": "Apa Itu Tombstoning (Tombstoning) Komponen SMD?", "id": "Komponen SMD Berdiri Satu Ujung." },
  { "en": "Penyebab Tombstoning (Tombstoning)?", "id": "Pemanasan Tidak Merata Saat Reflow." },
  { "en": "Apa Itu Jembatan Solder (Solder Bridge)?", "id": "Hubung Singkat Timah Antar Pin/Pad." },
  { "en": "Apa Itu Bola Solder (Solder Ball)?", "id": "Bola Timah Kecil Menempel PCB." },
  { "en": "Apa Itu Void (Rongga) Solder?", "id": "Gelembung Udara Terperangkap Sambungan Solder." },
  { "en": "Apa Itu Inspeksi Optik Otomatis (AOI)?", "id": "Pemeriksaan Kualitas Perakitan PCB Kamera." },
  { "en": "Apa Kepanjangan AOI (Automated Optical Inspection)?", "id": "Inspeksi Optik Otomatis." },
  { "en": "Apa Itu Inspeksi Sinar-X (X-Ray Inspection) PCB?", "id": "Pemeriksaan Sambungan Tersembunyi (BGA)." },
  { "en": "Apa Itu Rework (Pengerjaan Ulang) PCB?", "id": "Memperbaiki Kesalahan Perakitan PCB." },
  { "en": "Alat Apa Untuk Rework (Pengerjaan Ulang) SMD?", "id": "Hot Air Rework Station." },
  { "en": "Apa Itu Keandalan (Reliability)?", "id": "Probabilitas Perangkat Berfungsi Sesuai Waktu." },
  { "en": "Apa Itu Laju Kegagalan (Failure Rate) (λ)?", "id": "Frekuensi Kegagalan Komponen Per Waktu." },
  { "en": "Apa Itu Kurva Bak Mandi (Bathtub Curve)?", "id": "Grafik Laju Kegagalan Seiring Waktu." },
  { "en": "Apa Fase Awal Kurva Bak Mandi?", "id": "Kegagalan Dini (Infant Mortality)." },
  { "en": "Apa Fase Tengah Kurva Bak Mandi?", "id": "Masa Pakai Berguna (Useful Life)." },
  { "en": "Apa Fase Akhir Kurva Bak Mandi?", "id": "Kegagalan Aus (Wear-Out)." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antar Kegagalan (Bisa Diperbaiki)." },
  { "en": "Apa Itu MTTF (Mean Time To Failure)?", "id": "Waktu Rata-Rata Menuju Kegagalan (Tidak Diperbaiki)." },
  { "en": "Apa Itu Derating (Derating)?", "id": "Mengoperasikan Komponen Di Bawah Rating Maks." },
  { "en": "Tujuan Derating (Derating)?", "id": "Meningkatkan Keandalan, Umur Komponen." },
  { "en": "Apa Itu Analisis FMEA (Failure Modes and Effects Analysis)?", "id": "Analisis Potensi Mode Kegagalan." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (FTA)?", "id": "Fault Tree Analysis (FTA)." },
  { "en": "Apa Kepanjangan FTA (Fault Tree Analysis)?", "id": "Analisis Pohon Kegagalan." },
  { "en": "Apa Itu Redundansi (Redundancy)?", "id": "Duplikasi Komponen Untuk Keandalan." },
  { "en": "Apa Itu Toleransi Kesalahan (Fault Tolerance)?", "id": "Kemampuan Sistem Tetap Berfungsi Saat Gagal." },
  { "en": "Apa Itu Pengujian Lingkungan (Environmental Testing)?", "id": "Menguji Perangkat Kondisi Lingkungan Ekstrem." },
  { "en": "Contoh Pengujian Lingkungan (Environmental Testing)?", "id": "Uji Suhu, Kelembaban, Getaran." },
  { "en": "Apa Itu Siklus Termal (Thermal Cycling)?", "id": "Pengujian Perubahan Suhu Berulang." },
  { "en": "Apa Itu Pengujian Getaran (Vibration Testing)?", "id": "Menguji Ketahanan Perangkat Terhadap Getaran." },
  { "en": "Apa Itu Pengujian Kejut (Shock Testing)?", "id": "Menguji Ketahanan Perangkat Benturan Tiba-Tiba." },
  { "en": "Apa Itu Pengujian Kelembaban (Humidity Testing)?", "id": "Menguji Perangkat Kondisi Lembab." },
  { "en": "Apa Itu Pengujian Semprotan Garam (Salt Spray Test)?", "id": "Menguji Ketahanan Korosi." },
  { "en": "Apa Itu Pengujian HALT (Highly Accelerated Life Test)?", "id": "Tes Dipercepat Temukan Batas Operasi." },
  { "en": "Apa Kepanjangan HALT (Highly Accelerated Life Test)?", "id": "Tes Hidup Sangat Dipercepat." },
  { "en": "Apa Itu Pengujian HASS (Highly Accelerated Stress Screen)?", "id": "Screening Produksi Stres Dipercepat." },
  { "en": "Apa Kepanjangan HASS (Highly Accelerated Stress Screen)?", "id": "Screening Stres Sangat Dipercepat." },
  { "en": "Apa Itu Manajemen Termal (Thermal Management)?", "id": "Mengelola Panas Dihasilkan Elektronika." },
  { "en": "Metode Pembuangan Panas (Heat Dissipation)?", "id": "Konduksi, Konveksi, Radiasi." },
  { "en": "Apa Itu Konduksi (Conduction) Termal?", "id": "Transfer Panas Lewat Kontak Langsung." },
  { "en": "Apa Itu Konveksi (Convection) Termal?", "id": "Transfer Panas Lewat Aliran Fluida." },
  { "en": "Apa Itu Radiasi (Radiation) Termal?", "id": "Transfer Panas Lewat Gelombang Elektromagnetik." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance) (θ)?", "id": "Hambatan Aliran Panas." },
  { "en": "Satuan Resistansi Termal (Thermal Resistance)?", "id": "Derajat Celcius Per Watt (°C/W)." },
  { "en": "Apa Itu Heatsink (Heatsink)?", "id": "Komponen Logam Pembuang Panas Pasif." },
  { "en": "Apa Itu Kipas Pendingin (Cooling Fan)?", "id": "Meningkatkan Pendinginan Konveksi Paksa." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Transfer Panas Efisien (Perubahan Fasa)." },
  { "en": "Apa Itu Pendingin Termoelektrik (TEC)?", "id": "Thermoelectric Cooler (TEC) (Efek Peltier)." },
  { "en": "Apa Kepanjangan TEC (Thermoelectric Cooler)?", "id": "Pendingin Termoelektrik." },
  { "en": "Apa Itu Pasta Termal (Thermal Paste)?", "id": "Mengisi Celah Udara Tingkatkan Konduksi." },
  { "en": "Apa Itu Pad Termal (Thermal Pad)?", "id": "Bahan Lunak Konduktif Termal." },
  { "en": "Apa Itu Suhu Junction (Junction Temperature) (Tj)?", "id": "Suhu Internal Chip Semikonduktor." },
  { "en": "Mengapa Suhu Junction (Junction Temperature) Penting?", "id": "Mempengaruhi Kinerja, Keandalan Komponen." },
  { "en": "Apa Itu Daya Disipasi (Power Dissipation) Komponen?", "id": "Energi Listrik Diubah Jadi Panas." },
  { "en": "Bagaimana Hitung Kenaikan Suhu Junction?", "id": "ΔT = Daya Disipasi * Resistansi Termal." },
  { "en": "Apa Itu Simulasi Termal (Thermal Simulation)?", "id": "Analisis Distribusi Suhu Komputer." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Apa Tipe Motor Stepper (Stepper Motor)?", "id": "Magnet Permanen, Reluktansi Variabel, Hibrida." },
  { "en": "Apa Itu Sudut Langkah (Step Angle)?", "id": "Sudut Putaran Per Langkah Motor." },
  { "en": "Apa Itu Mode Full Step (Langkah Penuh)?", "id": "Mengaktifkan Dua Fasa Sekaligus." },
  { "en": "Apa Itu Mode Half Step (Setengah Langkah)?", "id": "Mengaktifkan Satu Atau Dua Fasa." },
  { "en": "Apa Keuntungan Mode Half Step (Setengah Langkah)?", "id": "Resolusi Lebih Halus, Getaran Kurang." },
  { "en": "Apa Itu Mode Microstepping (Langkah Mikro)?", "id": "Membagi Langkah Menjadi Sub-Langkah." },
  { "en": "Apa Keuntungan Mode Microstepping (Langkah Mikro)?", "id": "Gerakan Sangat Halus, Resolusi Tinggi." },
  { "en": "Apa Itu Driver (Driver) Motor Stepper?", "id": "Rangkaian Kontrol Urutan Pulsa Fasa." },
  { "en": "Contoh IC (Integrated Circuit) Driver Stepper Populer?", "id": "A4988, DRV8825." },
  { "en": "Apa Itu Torsi Tahan (Holding Torque)?", "id": "Torsi Menahan Posisi Tanpa Gerak." },
  { "en": "Apa Itu Torsi Tarik Masuk (Pull-In Torque)?", "id": "Torsi Maksimal Start Tanpa Kehilangan Langkah." },
  { "en": "Apa Itu Torsi Tarik Keluar (Pull-Out Torque)?", "id": "Torsi Maksimal Jalan Tanpa Kehilangan Langkah." },
  { "en": "Apa Itu Kehilangan Langkah (Missing Steps)?", "id": "Motor Gagal Bergerak Sesuai Pulsa." },
  { "en": "Penyebab Kehilangan Langkah (Missing Steps)?", "id": "Beban Berlebih, Akselerasi Terlalu Cepat." },
  { "en": "Apa Itu Kontrol Loop Terbuka (Open Loop)?", "id": "Kontrol Stepper Tanpa Umpan Balik Posisi." },
  { "en": "Apa Itu Kontrol Loop Tertutup (Closed Loop) Stepper?", "id": "Menggunakan Encoder Umpan Balik Posisi." },
  { "en": "Apa Keuntungan Loop Tertutup (Closed Loop) Stepper?", "id": "Tidak Ada Kehilangan Langkah, Presisi." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi/Kecepatan Loop Tertutup." },
  { "en": "Komponen Utama Motor Servo (Servo Motor)?", "id": "Motor DC, Gearbox, Sensor Posisi, Kontroler." },
  { "en": "Apa Sensor Posisi (Position Sensor) Servo Umum?", "id": "Potensiometer Atau Encoder." },
  { "en": "Bagaimana Sinyal Kontrol Servo RC?", "id": "Sinyal PWM (Pulse Width Modulation) Lebar Pulsa Variabel." },
  { "en": "Apa Itu Lebar Pulsa (Pulse Width) PWM Servo?", "id": "Mengontrol Posisi Sudut Poros Servo." },
  { "en": "Berapa Rentang Lebar Pulsa (Pulse Width) Servo RC Umum?", "id": "Sekitar 1000 Hingga 2000 Mikrosekon." },
  { "en": "Apa Itu Dead Band (Zona Mati) Servo?", "id": "Rentang Input Kecil Tanpa Gerakan." },
  { "en": "Apa Itu Motor DC (Direct Current) Tanpa Inti (Coreless)?", "id": "Rotor Ringan Respons Sangat Cepat." },
  { "en": "Dimana Motor Coreless (Tanpa Inti) Digunakan?", "id": "Servo Cepat, Drone Kecil." },
  { "en": "Apa Itu Aktuator Linier (Linear Actuator)?", "id": "Penggerak Menghasilkan Gerakan Lurus." },
  { "en": "Jenis Aktuator Linier (Linear Actuator) Umum?", "id": "Sekrup Bola (Ball Screw), Solenoida." },
  { "en": "Apa Itu Sekrup Bola (Ball Screw)?", "id": "Mekanisme Ubah Gerak Putar Ke Linear." },
  { "en": "Apa Itu Motor Linier (Linear Motor)?", "id": "Motor Hasilkan Gerak Linear Langsung." },
  { "en": "Apa Itu Voice Coil Actuator (VCA)?", "id": "Aktuator Linear Elektromagnetik Presisi." },
  { "en": "Dimana VCA (Voice Coil Actuator) Digunakan?", "id": "Hard Disk Drive (Head Actuator)." },
  { "en": "Apa Itu Sistem Mekatronika (Mechatronics)?", "id": "Integrasi Mekanik, Elektronik, Kontrol, Komputer." },
  { "en": "Apa Komponen Utama Sistem Mekatronika?", "id": "Sensor, Aktuator, Kontroler, Mekanisme." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Cabang Mekatronika Fokus Robot." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Mekanisme?", "id": "Jumlah Gerakan Independen Yang Mungkin." },
  { "en": "Apa Itu Kinematika (Kinematics)?", "id": "Studi Gerakan Tanpa Memperhitungkan Gaya." },
  { "en": "Apa Itu Dinamika (Dynamics)?", "id": "Studi Gerakan Memperhitungkan Gaya Penyebab." },
  { "en": "Apa Itu Analisis Elemen Hingga (FEA)?", "id": "Finite Element Analysis (FEA)." },
  { "en": "Apa Kepanjangan FEA (Finite Element Analysis)?", "id": "Analisis Elemen Hingga." },
  { "en": "Apa Fungsi FEA (Finite Element Analysis)?", "id": "Simulasi Perilaku Mekanis Struktur Komputer." },
  { "en": "Apa Itu Tegangan (Stress) Mekanis?", "id": "Gaya Internal Per Satuan Luas." },
  { "en": "Apa Itu Regangan (Strain) Mekanis?", "id": "Deformasi Relatif Material." },
  { "en": "Apa Itu Modulus Young (Young's Modulus)?", "id": "Ukuran Kekakuan Elastis Material." },
  { "en": "Apa Itu Kekuatan Luluh (Yield Strength)?", "id": "Batas Tegangan Deformasi Plastis Dimulai." },
  { "en": "Apa Itu Kekuatan Tarik (Tensile Strength)?", "id": "Tegangan Maksimum Material Sebelum Patah." },
  { "en": "Apa Itu Kelelahan (Fatigue) Material?", "id": "Kerusakan Akibat Beban Siklik Berulang." },
  { "en": "Apa Itu Getaran (Vibration)?", "id": "Osilasi Mekanis Sekitar Titik Setimbang." },
  { "en": "Apa Itu Frekuensi Alami (Natural Frequency) Mekanis?", "id": "Frekuensi Getaran Bebas Sistem." },
  { "en": "Apa Itu Resonansi (Resonance) Mekanis?", "id": "Amplitudo Getaran Besar Frekuensi Alami." },
  { "en": "Apa Itu Redaman (Damping) Mekanis?", "id": "Disipasi Energi Getaran." },
  { "en": "Apa Itu Analisis Modal (Modal Analysis)?", "id": "Menentukan Frekuensi Alami, Mode Getar." },
  { "en": "Apa Itu Sensor Getaran (Vibration Sensor)?", "id": "Mengukur Percepatan Atau Perpindahan Getaran." },
  { "en": "Contoh Sensor Getaran (Vibration Sensor)?", "id": "Akselerometer Piezoelektrik." },
  { "en": "Apa Itu Keseimbangan (Balancing) Rotor?", "id": "Mendistribusikan Massa Rotor Secara Merata." },
  { "en": "Mengapa Keseimbangan (Balancing) Rotor Penting?", "id": "Mengurangi Getaran, Meningkatkan Umur Mesin." },
  { "en": "Apa Itu Bearing (Bantalan)?", "id": "Elemen Mesin Mengurangi Gesekan Gerak." },
  { "en": "Jenis Bearing (Bantalan) Utama?", "id": "Bantalan Gelinding (Rolling), Luncur (Plain)." },
  { "en": "Contoh Bantalan Gelinding (Rolling Bearing)?", "id": "Ball Bearing, Roller Bearing." },
  { "en": "Apa Itu Pelumasan (Lubrication)?", "id": "Mengurangi Gesekan, Keausan Antar Permukaan." },
  { "en": "Jenis Pelumas (Lubricant) Umum?", "id": "Oli (Oil), Gemuk (Grease)." },
  { "en": "Apa Itu Gear (Roda Gigi)?", "id": "Roda Bergerigi Transmisi Gerak Putar." },
  { "en": "Apa Itu Rasio Gear (Gear Ratio)?", "id": "Perbandingan Jumlah Gigi Atau Kecepatan." },
  { "en": "Apa Fungsi Gearbox (Gearbox)?", "id": "Mengubah Kecepatan, Torsi Putaran." },
  { "en": "Apa Itu Kopling (Coupling)?", "id": "Menghubungkan Dua Poros." },
  { "en": "Apa Itu Rem (Brake)?", "id": "Menghentikan Atau Memperlambat Gerakan." },
  { "en": "Apa Itu Rem Elektromagnetik (Electromagnetic Brake)?", "id": "Rem Menggunakan Prinsip Elektromagnet." },
  { "en": "Apa Itu Aktuator Pneumatik (Pneumatic Actuator)?", "id": "Penggerak Menggunakan Udara Bertekanan." },
  { "en": "Apa Itu Aktuator Hidrolik (Hydraulic Actuator)?", "id": "Penggerak Menggunakan Cairan Bertekanan (Oli)." },
  { "en": "Apa Keunggulan Hidrolik (Hydraulic)?", "id": "Gaya Sangat Besar." },
  { "en": "Apa Keunggulan Pneumatik (Pneumatic)?", "id": "Cepat, Bersih, Relatif Murah." },
  { "en": "Apa Itu Silinder (Cylinder) Pneumatik/Hidrolik?", "id": "Aktuator Gerak Linear." },
  { "en": "Apa Itu Katup Kontrol Arah (Directional Control Valve)?", "id": "Mengatur Arah Aliran Fluida Aktuator." },
  { "en": "Apa Itu Katup Kontrol Aliran (Flow Control Valve)?", "id": "Mengatur Kecepatan Gerak Aktuator." },
  { "en": "Apa Itu Katup Kontrol Tekanan (Pressure Control Valve)?", "id": "Mengatur Batas Tekanan Sistem." },
  { "en": "Apa Itu Kompresor (Compressor) Udara?", "id": "Menghasilkan Udara Bertekanan Sistem Pneumatik." },
  { "en": "Apa Itu Unit Tenaga Hidrolik (Hydraulic Power Unit)?", "id": "Sumber Tekanan Sistem Hidrolik (Pompa)." },
  { "en": "Apa Itu Filter (Filter) Udara/Oli?", "id": "Membersihkan Fluida Dari Kontaminan." },
  { "en": "Apa Itu Regulator (Regulator) Tekanan?", "id": "Menjaga Tekanan Kerja Konstan." },
  { "en": "Apa Itu Pelumas (Lubricator) Udara Pneumatik?", "id": "Menambahkan Pelumas Ke Udara Bertekanan." },
  { "en": "Apa Itu Manometer (Manometer)?", "id": "Alat Ukur Tekanan Fluida." },
  { "en": "Apa Itu Saklar Tekanan (Pressure Switch)?", "id": "Saklar Aktif Berdasarkan Level Tekanan." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Elektronika Berbasis Sinyal Diskrit." },
  { "en": "Apa Itu Bit (Bit)?", "id": "Unit Informasi Digital (0 Atau 1)." },
  { "en": "Apa Itu Logika Boolean (Boolean Logic)?", "id": "Aljabar Untuk Operasi Logika Biner." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Implementasi Fungsi Logika Boolean." },
  { "en": "Apa Itu Sirkuit Kombinasional (Combinational Circuit)?", "id": "Output Hanya Tergantung Input Saat Ini." },
  { "en": "Apa Itu Sirkuit Sekuensial (Sequential Circuit)?", "id": "Output Tergantung Input, Keadaan Sebelumnya." },
  { "en": "Apa Itu Elemen Memori (Memory Element)?", "id": "Menyimpan Keadaan Biner (Flip-Flop)." },
  { "en": "Apa Itu Sinyal Clock (Clock Signal)?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Frekuensi Clock (Clock Frequency)?", "id": "Kecepatan Pulsa Sinyal Clock (Hz)." },
  { "en": "Apa Itu Periode Clock (Clock Period)?", "id": "Waktu Satu Siklus Sinyal Clock." },
  { "en": "Apa Itu Siklus Kerja (Duty Cycle) Clock?", "id": "Persentase Waktu Clock Tinggi (High)." },
  { "en": "Apa Itu Pemicuan Tepi (Edge Triggering)?", "id": "Flip-Flop Berubah Keadaan Tepi Clock." },
  { "en": "Apa Itu Pemicuan Level (Level Triggering)?", "id": "Latch Berubah Keadaan Selama Level Clock." },
  { "en": "Apa Itu Waktu Setup (Setup Time)?", "id": "Waktu Input Stabil Sebelum Clock Aktif." },
  { "en": "Apa Itu Waktu Tahan (Hold Time)?", "id": "Waktu Input Stabil Setelah Clock Aktif." },
  { "en": "Apa Itu Metastabilitas (Metastability)?", "id": "Keadaan Tidak Stabil Flip-Flop (Violasi Timing)." },
  { "en": "Apa Itu Sinkronizer (Synchronizer)?", "id": "Rangkaian Menyelaraskan Sinyal Asinkron Ke Clock." },
  { "en": "Apa Itu Reset (Reset) Asinkron?", "id": "Reset Langsung Tanpa Menunggu Clock." },
  { "en": "Apa Itu Reset (Reset) Sinkron?", "id": "Reset Terjadi Pada Tepi Clock Aktif." },
  { "en": "Apa Itu Enable (Enable) Sinyal?", "id": "Sinyal Mengaktifkan/Menonaktifkan Fungsi Rangkaian." },
  { "en": "Apa Itu Arsitektur Komputer (Computer Architecture)?", "id": "Desain Konseptual, Struktur Operasional Komputer." },
  { "en": "Apa Itu Set Instruksi (Instruction Set Architecture) (ISA)?", "id": "Kumpulan Perintah Dipahami Prosesor." },
  { "en": "Apa Kepanjangan ISA (Instruction Set Architecture)?", "id": "Arsitektur Set Instruksi." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Arsitektur Set Instruksi Sederhana." },
  { "en": "Apa Kepanjangan RISC (Reduced Instruction Set Computer)?", "id": "Komputer Set Instruksi Tereduksi." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Arsitektur Set Instruksi Kompleks." },
  { "en": "Apa Kepanjangan CISC (Complex Instruction Set Computer)?", "id": "Komputer Set Instruksi Kompleks." },
  { "en": "Arsitektur Apa Digunakan Prosesor x86 (Intel/AMD)?", "id": "Arsitektur CISC (Complex Instruction Set Computer)." },
  { "en": "Arsitektur Apa Digunakan Prosesor ARM?", "id": "Arsitektur RISC (Reduced Instruction Set Computer)." },
  { "en": "Apa Itu Pipelining (Pipelining) Instruksi?", "id": "Teknik Eksekusi Instruksi Tumpang Tindih." },
  { "en": "Apa Tujuan Pipelining (Pipelining)?", "id": "Meningkatkan Throughput (Jumlah Instruksi/Detik)." },
  { "en": "Apa Itu Hazard (Hazard) Pipeline?", "id": "Masalah Dependensi Hambat Pipeline." },
  { "en": "Jenis Hazard (Hazard) Pipeline?", "id": "Struktural, Data, Kontrol." },
  { "en": "Apa Itu Hazard Data (Data Hazard)?", "id": "Instruksi Butuh Hasil Instruksi Sebelumnya." },
  { "en": "Apa Itu Hazard Kontrol (Control Hazard)?", "id": "Instruksi Percabangan (Branch) Ubah Alur." },
  { "en": "Apa Itu Prediksi Percabangan (Branch Prediction)?", "id": "Menebak Arah Percabangan Optimasi Pipeline." },
  { "en": "Apa Itu Eksekusi Superskalar (Superscalar Execution)?", "id": "Eksekusi Lebih Dari Satu Instruksi/Siklus." },
  { "en": "Apa Itu Eksekusi Out-of-Order (Out-of-Order Execution)?", "id": "Eksekusi Instruksi Tidak Sesuai Urutan Program." },
  { "en": "Apa Itu Memori Cache (Cache Memory)?", "id": "Memori Kecil, Cepat Dekat CPU." },
  { "en": "Apa Fungsi Memori Cache (Cache Memory)?", "id": "Mengurangi Latensi Akses Memori Utama." },
  { "en": "Apa Itu Tingkatan Cache (Cache Level) (L1, L2, L3)?", "id": "Hierarki Cache Berdasarkan Kecepatan, Ukuran." },
  { "en": "Cache (Cache) Mana Paling Cepat, Paling Kecil?", "id": "Cache L1 (Level 1)." },
  { "en": "Apa Itu Cache Hit (Cache Hit)?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss (Cache Miss)?", "id": "Data Yang Dicari Tidak Ada Cache." },
  { "en": "Apa Itu Koherensi Cache (Cache Coherence)?", "id": "Memastikan Konsistensi Data Multi Cache." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Teknik Abstraksi Memori Fisik." },
  { "en": "Apa Itu Paging (Paging) Memori Virtual?", "id": "Membagi Memori Virtual, Fisik Jadi Halaman." },
  { "en": "Apa Itu Tabel Halaman (Page Table)?", "id": "Struktur Data Pemetaan Alamat Virtual-Fisik." },
  { "en": "Apa Itu Page Fault (Page Fault)?", "id": "Akses Halaman Tidak Ada Memori Fisik." },
  { "en": "Apa Itu Translation Lookaside Buffer (TLB)?", "id": "Cache Cepat Untuk Tabel Halaman." },
  { "en": "Apa Kepanjangan TLB (Translation Lookaside Buffer)?", "id": "Buffer Pelihat Terjemahan." },
  { "en": "Apa Itu Direct Memory Access (DMA)?", "id": "Akses Memori Langsung Tanpa Libatkan CPU." },
  { "en": "Apa Kepanjangan DMA (Direct Memory Access)?", "id": "Akses Memori Langsung." },
  { "en": "Apa Fungsi Kontroler DMA (DMA Controller)?", "id": "Mengatur Transfer Data DMA." },
  { "en": "Apa Itu Interrupt (Interupsi)?", "id": "Sinyal Meminta Perhatian CPU Segera." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Kode Ditangani Saat Interrupt Terjadi." },
  { "en": "Apa Kepanjangan ISR (Interrupt Service Routine)?", "id": "Rutinitas Layanan Interupsi." },
  { "en": "Apa Itu Vektor Interrupt (Interrupt Vector)?", "id": "Alamat ISR (Interrupt Service Routine) Sesuai Sumber Interrupt." },
  { "en": "Apa Itu Non-Maskable Interrupt (NMI)?", "id": "Interrupt Prioritas Tinggi Tak Bisa Diabaikan." },
  { "en": "Apa Kepanjangan NMI (Non-Maskable Interrupt)?", "id": "Interupsi Tak Dapat Ditutupi." },
  { "en": "Apa Itu Polling (Polling) I/O?", "id": "CPU Periodik Memeriksa Status Perangkat I/O." },
  { "en": "Apa Kerugian Polling (Polling) I/O?", "id": "Membuang Waktu CPU Jika Perangkat Lambat." },
  { "en": "Apa Itu Bus (Bus) Komputer?", "id": "Jalur Komunikasi Berbagi Komponen." },
  { "en": "Jenis Bus (Bus) Komputer Utama?", "id": "Bus Alamat, Data, Kontrol." },
  { "en": "Apa Itu Lebar Bus (Bus Width)?", "id": "Jumlah Bit Dapat Ditransfer Sekaligus." },
  { "en": "Apa Itu Kecepatan Bus (Bus Speed)?", "id": "Frekuensi Clock Operasi Bus (MHz)." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Bus?", "id": "Laju Transfer Data Maksimum Bus." },
  { "en": "Apa Itu Arbitrasi Bus (Bus Arbitration)?", "id": "Mekanisme Penentuan Master Bus." },
  { "en": "Apa Itu Bus Sinkron (Synchronous Bus)?", "id": "Transfer Data Dikoordinasi Sinyal Clock." },
  { "en": "Apa Itu Bus Asinkron (Asynchronous Bus)?", "id": "Transfer Data Menggunakan Sinyal Handshake." },
  { "en": "Contoh Bus Internal (Internal Bus)?", "id": "Bus Antara CPU, Cache, Memori Utama." },
  { "en": "Contoh Bus Eksternal (External Bus)?", "id": "Bus Menghubungkan Periferal (USB, PCIe)." },
  { "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Antarmuka Serial Standar Periferal." },
  { "en": "Apa Itu PCIe (PCI Express)?", "id": "Antarmuka Bus Kecepatan Tinggi (Kartu Grafis)." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Khusus Fungsi Tertentu." },
  { "en": "Contoh Sistem Tertanam (Embedded System)?", "id": "Mikrokontroler Di Mesin Cuci, Mobil." },
  { "en": "Apa Karakteristik Sistem Tertanam (Embedded System)?", "id": "Real-Time, Daya Rendah, Ukuran Kecil." },
  { "en": "Apa Itu Sistem Operasi Real-Time (RTOS)?", "id": "Sistem Operasi Respon Waktu Deterministik." },
  { "en": "Apa Kepanjangan RTOS (Real-Time Operating System)?", "id": "Sistem Operasi Waktu Nyata." },
  { "en": "Apa Perbedaan RTOS (Real-Time Operating System), OS Umum?", "id": "RTOS Fokus Ketepatan Waktu Respon." },
  { "en": "Apa Itu Task (Tugas) RTOS?", "id": "Unit Eksekusi Independen Dalam RTOS." },
  { "en": "Apa Itu Penjadwal (Scheduler) RTOS?", "id": "Komponen Mengatur Eksekusi Task." },
  { "en": "Apa Itu Prioritas (Priority) Task?", "id": "Tingkat Kepentingan Eksekusi Task." },
  { "en": "Apa Itu Semaphore (Semaphore) RTOS?", "id": "Mekanisme Sinkronisasi, Komunikasi Antar Task." },
  { "en": "Apa Itu Mutex (Mutex) RTOS?", "id": "Mutual Exclusion (Pengaman Akses Sumber Daya)." },
  { "en": "Apa Itu Antrian Pesan (Message Queue) RTOS?", "id": "Mekanisme Komunikasi Data Antar Task." },
  { "en": "Apa Itu Jitter (Jitter) RTOS?", "id": "Variasi Waktu Eksekusi Task Periodik." },
  { "en": "Apa Itu Deadline (Batas Waktu) RTOS?", "id": "Batas Waktu Maksimum Eksekusi Task." },
  { "en": "Apa Itu Sistem Hard Real-Time?", "id": "Kegagalan Memenuhi Deadline Fatal." },
  { "en": "Apa Itu Sistem Soft Real-Time?", "id": "Kegagalan Memenuhi Deadline Degradasi Kinerja." },
  { "en": "Apa Itu Cross-Compiler (Kompilator Silang)?", "id": "Kompilator Jalan Satu Arsitektur Hasilkan Kode Lain." },
  { "en": "Mengapa Perlu Cross-Compiler (Kompilator Silang) Embedded?", "id": "Kompilasi Di Host PC Target Embedded." },
  { "en": "Apa Itu In-Circuit Emulator (ICE)?", "id": "Alat Debugging Hardware Sistem Tertanam." },
  { "en": "Apa Kepanjangan ICE (In-Circuit Emulator)?", "id": "Emulator Dalam Sirkuit." },
  { "en": "Apa Itu JTAG (Joint Test Action Group) Debugger?", "id": "Antarmuka Debugging Hardware Populer." },
  { "en": "Apa Itu Firmware (Perangkat Tegar)?", "id": "Perangkat Lunak Tertanam Di Hardware." },
  { "en": "Apa Itu Bootloader (Bootloader)?", "id": "Program Inisialisasi, Memuat Firmware Utama." },
  { "en": "Apa Itu Pembaruan Over-The-Air (OTA)?", "id": "Pembaruan Firmware Nirkabel." },
  { "en": "Apa Kepanjangan OTA (Over-The-Air)?", "id": "Lewat Udara." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Pengiriman Data Bit Per Bit Satu Jalur." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Pengiriman Data Multi Bit Bersamaan." },
  { "en": "Mana Lebih Cepat Jarak Pendek, Serial Atau Paralel?", "id": "Paralel Umumnya Lebih Cepat." },
  { "en": "Mana Lebih Baik Jarak Jauh, Serial Atau Paralel?", "id": "Serial (Masalah Skew Paralel)." },
  { "en": "Apa Itu Skew (Skew) Sinyal?", "id": "Perbedaan Waktu Tiba Sinyal Paralel." },
  { "en": "Apa Itu Komunikasi Sinkron (Synchronous Communication)?", "id": "Transfer Data Dikoordinasi Sinyal Clock." },
  { "en": "Apa Itu Komunikasi Asinkron (Asynchronous Communication)?", "id": "Transfer Data Tanpa Clock Bersama (Start/Stop Bit)." },
  { "en": "Protokol Apa Yang Sinkron?", "id": "SPI (Serial Peripheral Interface), I2C (Inter-Integrated Circuit)." },
  { "en": "Protokol Apa Yang Asinkron?", "id": "UART (Universal Asynchronous Receiver-Transmitter) (RS-232)." },
  { "en": "Apa Itu Laju Baud (Baud Rate)?", "id": "Jumlah Simbol Ditransfer Per Detik." },
  { "en": "Apa Itu Bit Rate (Laju Bit)?", "id": "Jumlah Bit Ditransfer Per Detik." },
  { "en": "Apakah Laju Baud (Baud Rate) Selalu Sama Bit Rate?", "id": "Tidak Selalu (Tergantung Pengkodean)." },
  { "en": "Apa Itu Paritas (Parity) Bit Komunikasi Serial?", "id": "Bit Tambahan Deteksi Error Sederhana." },
  { "en": "Apa Itu Protokol Handshake (Handshake Protocol)?", "id": "Sinyal Kontrol Koordinasi Transfer Data." },
  { "en": "Contoh Handshake (Handshake) RS-232?", "id": "RTS (Request To Send) / CTS (Clear To Send)." },
  { "en": "Apa Itu Antarmuka Diferensial (Differential Signaling)?", "id": "Menggunakan Sepasang Kawat Sinyal Berlawanan." },
  { "en": "Apa Keuntungan Sinyal Diferensial?", "id": "Lebih Kebal Terhadap Noise Mode Bersama." },
  { "en": "Contoh Antarmuka Diferensial?", "id": "RS-485, LVDS (Low Voltage Differential Signaling), Ethernet." },
  { "en": "Apa Kepanjangan LVDS (Low Voltage Differential Signaling)?", "id": "Sinyal Diferensial Tegangan Rendah." },
  { "en": "Apa Itu CAN (Controller Area Network) Bus?", "id": "Bus Komunikasi Kuat Kendaraan Otomotif." },
  { "en": "Apa Kepanjangan CAN (Controller Area Network)?", "id": "Jaringan Area Kontroler." },
  { "en": "Apa Karakteristik CAN (Controller Area Network) Bus?", "id": "Diferensial, Multi-Master, Deteksi Error." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Jaringan Kabel LAN Paling Umum." },
  { "en": "Apa Standar IEEE (Institute of Electrical and Electronics Engineers) Ethernet?", "id": "IEEE 802.3." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Kepanjangan MAC (Media Access Control)?", "id": "Kontrol Akses Media." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Alamat Logis Perangkat Jaringan IP." },
  { "en": "Apa Beda IPv4 (Internet Protocol version 4), IPv6 (Internet Protocol version 6)?", "id": "Panjang Alamat (32 bit vs 128 bit)." },
  { "en": "Apa Itu Protokol TCP (Transmission Control Protocol)?", "id": "Protokol Transport Andal Berorientasi Koneksi." },
  { "en": "Apa Kepanjangan TCP (Transmission Control Protocol)?", "id": "Protokol Kontrol Transmisi." },
  { "en": "Apa Itu Protokol UDP (User Datagram Protocol)?", "id": "Protokol Transport Cepat Tanpa Koneksi." },
  { "en": "Apa Kepanjangan UDP (User Datagram Protocol)?", "id": "Protokol Datagram Pengguna." },
  { "en": "Mana Lebih Andal, TCP (Transmission Control Protocol) Atau UDP (User Datagram Protocol)?", "id": "TCP (Transmission Control Protocol) (Ada Error Checking, Urutan)." },
  { "en": "Aplikasi Apa Menggunakan UDP (User Datagram Protocol)?", "id": "Streaming Video, Game Online, DNS." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Referensi Tujuh Lapisan Jaringan." },
  { "en": "Sebutkan Lapisan (Layer) Model OSI?", "id": "Fisik, Data Link, Jaringan, Transport, Sesi, Presentasi, Aplikasi." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Referensi Empat/Lima Lapisan Internet." },
  { "en": "Apa Itu Enkapsulasi (Encapsulation) Data Jaringan?", "id": "Pembungkusan Data Dengan Header Tiap Lapisan." },
  { "en": "Apa Itu Dekapsulasi (Decapsulation) Data Jaringan?", "id": "Pelepasan Header Data Tiap Lapisan." },
  { "en": "Perangkat Apa Bekerja Di Lapisan Fisik (Physical Layer)?", "id": "Hub (Hub), Repeater (Pengulang), Kabel." },
  { "en": "Perangkat Apa Bekerja Di Lapisan Data Link?", "id": "Switch (Saklar), Bridge (Jembatan), Kartu Jaringan (NIC)." },
  { "en": "Perangkat Apa Bekerja Di Lapisan Jaringan (Network Layer)?", "id": "Router (Router)." },
  { "en": "Apa Fungsi Router (Router)?", "id": "Menghubungkan Jaringan Berbeda, Routing Paket." },
  { "en": "Apa Fungsi Switch (Saklar)?", "id": "Menghubungkan Perangkat Dalam Satu LAN." },
  { "en": "Apa Beda Hub (Hub) Dan Switch (Saklar)?", "id": "Hub (Broadcast), Switch (Kirim Ke Tujuan)." },
  { "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Area Lokal (Gedung, Kantor)." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Area Luas (Antar Kota/Negara)." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Global Terhubung." },
  { "en": "Apa Itu Intranet (Intranet)?", "id": "Jaringan Privat Internal Organisasi." },
  { "en": "Apa Itu Extranet (Extranet)?", "id": "Intranet Diperluas Akses Pihak Eksternal." },
  { "en": "Apa Itu Topologi Jaringan (Network Topology)?", "id": "Susunan Fisik Atau Logis Jaringan." },
  { "en": "Contoh Topologi Fisik (Physical Topology)?", "id": "Bus, Star (Bintang), Ring (Cincin), Mesh." },
  { "en": "Topologi Apa Paling Umum LAN Modern?", "id": "Topologi Bintang (Star Topology)." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Jaringan Pasangan Terpilin Umum." },
  { "en": "Kategori Kabel UTP (Unshielded Twisted Pair) Umum?", "id": "Cat 5e (Category 5e), Cat 6 (Category 6)." },
  { "en": "Apa Itu Konektor RJ-45 (Registered Jack 45)?", "id": "Konektor Standar Kabel Ethernet UTP." },
  { "en": "Apa Itu PoE (Power over Ethernet)?", "id": "Menyalurkan Daya Listrik Lewat Kabel Ethernet." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Nirkabel Lokal (WLAN)." },
  { "en": "Apa Standar IEEE (Institute of Electrical and Electronics Engineers) Wi-Fi?", "id": "IEEE 802.11." },
  { "en": "Apa Itu Access Point (Titik Akses) (AP)?", "id": "Perangkat Penghubung Nirkabel Ke Jaringan Kabel." },
  { "en": "Apa Itu SSID (Service Set Identifier)?", "id": "Nama Jaringan Nirkabel Wi-Fi." },
  { "en": "Metode Keamanan Wi-Fi (Wi-Fi Security) Umum?", "id": "WPA2 (Wi-Fi Protected Access 2), WPA3 (Wi-Fi Protected Access 3)." },
  { "en": "Apa Itu Bluetooth (Bluetooth)?", "id": "Standar Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Pendek." },
  { "en": "Apa Frekuensi Operasi NFC (Near Field Communication)?", "id": "Tiga Belas Koma Lima Enam Megahertz (13.56 MHz)." },
  { "en": "Apa Mode Operasi NFC (Near Field Communication)?", "id": "Reader/Writer, Peer-to-Peer, Card Emulation." },
  { "en": "Apa Itu RFID (Radio Frequency Identification)?", "id": "Identifikasi Menggunakan Gelombang Radio." },
  { "en": "Apa Komponen Sistem RFID (Radio Frequency Identification)?", "id": "Tag (Transponder), Reader (Interrogator), Antena." },
  { "en": "Apa Itu Tag RFID (Radio Frequency Identification) Pasif?", "id": "Tag Tanpa Sumber Daya Internal." },
  { "en": "Apa Itu Tag RFID (Radio Frequency Identification) Aktif?", "id": "Tag Dengan Sumber Daya Internal (Baterai)." },
  { "en": "Apa Rentang Frekuensi RFID (Radio Frequency Identification)?", "id": "LF (Low Frequency), HF (High Frequency), UHF (Ultra High Frequency)." },
  { "en": "Apa Itu Zigbee (Zigbee)?", "id": "Protokol Jaringan Nirkabel Daya Rendah." },
  { "en": "Standar IEEE (Institute of Electrical and Electronics Engineers) Apa Dasar Zigbee?", "id": "IEEE 802.15.4." },
  { "en": "Apa Topologi Jaringan Zigbee (Zigbee)?", "id": "Star (Bintang), Tree (Pohon), Mesh." },
  { "en": "Dimana Zigbee (Zigbee) Umum Digunakan?", "id": "Otomatisasi Rumah, Jaringan Sensor Nirkabel." },
  { "en": "Apa Itu Z-Wave (Z-Wave)?", "id": "Protokol Nirkabel Otomatisasi Rumah Lainnya." },
  { "en": "Apa Beda Frekuensi Zigbee, Z-Wave?", "id": "Zigbee (2.4 GHz), Z-Wave (Sub-GHz)." },
  { "en": "Apa Itu LoRa (Long Range)?", "id": "Teknologi Modulasi Nirkabel Jarak Jauh." },
  { "en": "Apa Kepanjangan LoRaWAN (Long Range Wide Area Network)?", "id": "Jaringan Area Luas Jarak Jauh." },
  { "en": "Apa Karakteristik LoRaWAN (Long Range Wide Area Network)?", "id": "Jangkauan Jauh, Daya Rendah, Bandwidth Rendah." },
  { "en": "Dimana LoRaWAN (Long Range Wide Area Network) Digunakan?", "id": "Aplikasi IoT (Internet of Things) Skala Besar." },
  { "en": "Apa Itu Sigfox (Sigfox)?", "id": "Teknologi Jaringan IoT (Internet of Things) Daya Rendah Lainnya." },
  { "en": "Apa Itu NB-IoT (Narrowband IoT)?", "id": "Standar Seluler IoT (Internet of Things) Pita Sempit." },
  { "en": "Apa Kepanjangan NB-IoT (Narrowband Internet of Things)?", "id": "Internet Untuk Segala Pita Sempit." },
  { "en": "Apa Itu LTE-M (LTE for Machines)?", "id": "Standar Seluler IoT (Internet of Things) Lainnya." },
  { "en": "Apa Itu Sistem Kontrol Versi (VCS)?", "id": "Version Control System (VCS)." },
  { "en": "Apa Kepanjangan VCS (Version Control System)?", "id": "Sistem Kontrol Versi." },
  { "en": "Apa Fungsi VCS (Version Control System)?", "id": "Melacak Perubahan Kode Sumber." },
  { "en": "Contoh VCS (Version Control System) Terdistribusi?", "id": "Git (Git), Mercurial (Mercurial)." },
  { "en": "Contoh VCS (Version Control System) Terpusat?", "id": "Subversion (SVN), CVS (Concurrent Versions System)." },
  { "en": "Apa Itu Repositori (Repository) Git?", "id": "Tempat Penyimpanan Proyek Git." },
  { "en": "Apa Itu Commit (Commit) Git?", "id": "Snapshot Perubahan Kode Proyek." },
  { "en": "Apa Itu Branch (Cabang) Git?", "id": "Garis Pengembangan Independen." },
  { "en": "Apa Itu Merge (Menggabungkan) Git?", "id": "Menggabungkan Perubahan Antar Branch." },
  { "en": "Apa Itu Platform Hosting Git Populer?", "id": "GitHub, GitLab, Bitbucket." },
  { "en": "Apa Itu Permintaan Tarik (Pull Request)?", "id": "Mekanisme Review, Merge Kode Kolaboratif." },
  { "en": "Apa Itu Fork (Fork) Repositori?", "id": "Membuat Salinan Repositori Milik Sendiri." },
  { "en": "Apa Itu Klon (Clone) Repositori?", "id": "Membuat Salinan Lokal Repositori Remote." },
  { "en": "Apa Itu Lisensi Perangkat Lunak (Software License)?", "id": "Izin Penggunaan, Distribusi Perangkat Lunak." },
  { "en": "Apa Itu Lisensi Open Source (Sumber Terbuka)?", "id": "Lisensi Izinkan Akses Kode Sumber." },
  { "en": "Contoh Lisensi Open Source Permisif?", "id": "MIT (Massachusetts Institute of Technology), BSD (Berkeley Software Distribution), Apache." },
  { "en": "Contoh Lisensi Open Source Copyleft?", "id": "GPL (General Public License)." },
  { "en": "Apa Perbedaan Utama Lisensi Permisif, Copyleft?", "id": "Copyleft Wajibkan Turunan Tetap Open Source." },
  { "en": "Apa Itu Lisensi Proprietary (Proprietary License)?", "id": "Lisensi Perangkat Lunak Sumber Tertutup." },
  { "en": "Apa Itu EULA (End User License Agreement)?", "id": "Perjanjian Lisensi Pengguna Akhir." },
  { "en": "Apa Kepanjangan EULA (End User License Agreement)?", "id": "Perjanjian Lisensi Pengguna Akhir." },
  { "en": "Apa Itu Paten (Patent) Perangkat Lunak?", "id": "Hak Eksklusif Atas Algoritma/Ide Software." },
  { "en": "Apa Itu Hak Cipta (Copyright) Perangkat Lunak?", "id": "Hak Eksklusif Atas Ekspresi Kode." },
  { "en": "Apa Itu Merek Dagang (Trademark) Perangkat Lunak?", "id": "Nama Atau Logo Pembeda Software." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak (Software Engineering)?", "id": "Disiplin Pengembangan Perangkat Lunak Sistematis." },
  { "en": "Apa Itu Siklus Hidup Pengembangan Perangkat Lunak (SDLC)?", "id": "Tahapan Proses Pengembangan Software." },
  { "en": "Sebutkan Tahapan Umum SDLC (Software Development Life Cycle)?", "id": "Perencanaan, Analisis, Desain, Implementasi, Pengujian, Pemeliharaan." },
  { "en": "Apa Itu Model Air Terjun (Waterfall Model)?", "id": "Model SDLC (Software Development Life Cycle) Sekuensial Linear." },
  { "en": "Apa Itu Model Agile (Agile Model)?", "id": "Model SDLC (Software Development Life Cycle) Iteratif, Inkremental." },
  { "en": "Apa Prinsip Utama Agile (Agile)?", "id": "Kolaborasi, Respon Cepat Perubahan." },
  { "en": "Apa Itu Scrum (Scrum)?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
  { "en": "Apa Itu Sprint (Sprint) Dalam Scrum?", "id": "Iterasi Pengembangan Waktu Tetap." },
  { "en": "Apa Itu Product Backlog (Product Backlog)?", "id": "Daftar Fitur, Kebutuhan Produk." },
  { "en": "Apa Itu Sprint Backlog (Sprint Backlog)?", "id": "Tugas Dipilih Dikerjakan Dalam Sprint." },
  { "en": "Apa Itu Daily Standup (Rapat Harian)?", "id": "Rapat Singkat Update Progres Harian Tim." },
  { "en": "Apa Itu Kanban (Kanban)?", "id": "Metode Visualisasi Alur Kerja Agile." },
  { "en": "Apa Itu Pengujian Perangkat Lunak (Software Testing)?", "id": "Proses Verifikasi, Validasi Software." },
  { "en": "Apa Itu Pengujian Kotak Putih (White Box Testing)?", "id": "Pengujian Berdasarkan Pengetahuan Kode Internal." },
  { "en": "Apa Itu Pengujian Kotak Hitam (Black Box Testing)?", "id": "Pengujian Berdasarkan Fungsionalitas Eksternal." },
  { "en": "Apa Itu Pengujian Unit (Unit Testing)?", "id": "Pengujian Komponen Kode Terkecil." },
  { "en": "Apa Itu Pengujian Integrasi (Integration Testing)?", "id": "Pengujian Interaksi Antar Komponen." },
  { "en": "Apa Itu Pengujian Sistem (System Testing)?", "id": "Pengujian Sistem Software Keseluruhan." },
  { "en": "Apa Itu Pengujian Penerimaan Pengguna (UAT)?", "id": "User Acceptance Testing (UAT)." },
  { "en": "Apa Kepanjangan UAT (User Acceptance Testing)?", "id": "Pengujian Penerimaan Pengguna." },
  { "en": "Apa Itu Pengujian Regresi (Regression Testing)?", "id": "Memastikan Perubahan Kode Tidak Merusak Fitur Lama." },
  { "en": "Apa Itu Pengujian Kinerja (Performance Testing)?", "id": "Mengukur Responsivitas, Stabilitas Software." },
  { "en": "Apa Itu Pengujian Beban (Load Testing)?", "id": "Menguji Kinerja Beban Pengguna Normal." },
  { "en": "Apa Itu Pengujian Stres (Stress Testing)?", "id": "Menguji Kinerja Di Luar Batas Normal." },
  { "en": "Apa Itu Otomatisasi Pengujian (Test Automation)?", "id": "Menggunakan Alat Jalankan Tes Otomatis." },
  { "en": "Apa Itu Bug (Bug) Software?", "id": "Kesalahan Atau Cacat Perangkat Lunak." },
  { "en": "Apa Itu Debugging (Debugging)?", "id": "Proses Menemukan, Memperbaiki Bug." },
  { "en": "Apa Itu Pelacakan Bug (Bug Tracking System)?", "id": "Sistem Pencatatan, Pengelolaan Laporan Bug." },
  { "en": "Apa Itu Kualitas Perangkat Lunak (Software Quality)?", "id": "Ukuran Seberapa Baik Software Penuhi Kebutuhan." },
  { "en": "Atribut Kualitas Perangkat Lunak (Software Quality)?", "id": "Fungsionalitas, Keandalan, Efisiensi, Pemeliharaan." },
  { "en": "Apa Itu Pemeliharaan Perangkat Lunak (Software Maintenance)?", "id": "Modifikasi Software Setelah Rilis." },
  { "en": "Jenis Pemeliharaan Perangkat Lunak (Software Maintenance)?", "id": "Korektif, Adaptif, Perfektif, Preventif." },
  { "en": "Apa Itu Pemeliharaan Korektif (Corrective Maintenance)?", "id": "Memperbaiki Bug Yang Ditemukan." },
  { "en": "Apa Itu Pemeliharaan Adaptif (Adaptive Maintenance)?", "id": "Menyesuaikan Software Lingkungan Baru." },
  { "en": "Apa Itu Pemeliharaan Perfektif (Perfective Maintenance)?", "id": "Menambah Fitur Baru, Meningkatkan Kinerja." },
  { "en": "Apa Itu Pemeliharaan Preventif (Preventive Maintenance)?", "id": "Meningkatkan Kemudahan Pemeliharaan Masa Depan." },
  { "en": "Apa Itu Utang Teknis (Technical Debt)?", "id": "Implikasi Pilihan Desain Cepat Buruk." },
  { "en": "Apa Itu Refactoring (Refaktorisasi) Kode?", "id": "Memperbaiki Struktur Internal Kode." },
  { "en": "Apa Tujuan Refactoring (Refaktorisasi)?", "id": "Meningkatkan Keterbacaan, Pemeliharaan Kode." },
  { "en": "Apa Itu Kode Bersih (Clean Code)?", "id": "Kode Mudah Dibaca, Dimengerti." },
  { "en": "Apa Itu Prinsip DRY (Don't Repeat Yourself)?", "id": "Jangan Ulangi Dirimu Sendiri (Hindari Duplikasi)." },
  { "en": "Apa Kepanjangan DRY (Don't Repeat Yourself)?", "id": "Jangan Ulangi Dirimu Sendiri." },
  { "en": "Apa Itu Prinsip KISS (Keep It Simple Stupid)?", "id": "Jaga Tetap Sederhana, Bodoh." },
  { "en": "Apa Kepanjangan KISS (Keep It Simple Stupid)?", "id": "Jaga Tetap Sederhana, Bodoh." },
  { "en": "Apa Itu Prinsip YAGNI (You Ain't Gonna Need It)?", "id": "Anda Tidak Akan Membutuhkannya." },
  { "en": "Apa Kepanjangan YAGNI (You Ain't Gonna Need It)?", "id": "Anda Tidak Akan Membutuhkannya." },
  { "en": "Apa Maksud Prinsip YAGNI (You Ain't Gonna Need It)?", "id": "Jangan Implementasi Fitur Belum Dibutuhkan." },
  { "en": "Apa Itu Pola Desain (Design Pattern)?", "id": "Solusi Teruji Masalah Desain Umum." },
  { "en": "Apa Itu Arsitektur Perangkat Lunak (Software Architecture)?", "id": "Struktur Tingkat Tinggi Sistem Software." },
  { "en": "Apa Itu Arsitektur Monolitik (Monolithic Architecture)?", "id": "Aplikasi Dibangun Unit Tunggal Besar." },
  { "en": "Apa Itu Arsitektur Microservices (Microservices Architecture)?", "id": "Aplikasi Dibangun Kumpulan Layanan Kecil." },
  { "en": "Apa Keuntungan Microservices (Layanan Mikro)?", "id": "Skalabilitas, Fleksibilitas Teknologi, Deployment Independen." },
  { "en": "Apa Kerugian Microservices (Layanan Mikro)?", "id": "Kompleksitas Operasional, Jaringan." },
  { "en": "Apa Itu API (Application Programming Interface) Gateway?", "id": "Titik Masuk Tunggal Untuk Klien Microservices." },
  { "en": "Apa Itu Penemuan Layanan (Service Discovery)?", "id": "Mekanisme Layanan Menemukan Lokasi Lainnya." },
  { "en": "Apa Itu Komunikasi Sinkron (Synchronous Communication) Microservices?", "id": "Klien Menunggu Respon Layanan (HTTP)." },
  { "en": "Apa Itu Komunikasi Asinkron (Asynchronous Communication) Microservices?", "id": "Klien Tidak Menunggu Respon (Message Queue)." },
  { "en": "Apa Itu Antrian Pesan (Message Queue)?", "id": "Broker Pesan Komunikasi Asinkron." },
  { "en": "Contoh Antrian Pesan (Message Queue)?", "id": "RabbitMQ, Kafka, ActiveMQ." },
  { "en": "Apa Itu Event-Driven Architecture (EDA)?", "id": "Arsitektur Berbasis Kejadian (Event)." },
  { "en": "Apa Kepanjangan EDA (Event-Driven Architecture)?", "id": "Arsitektur Berbasis Kejadian." }



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
