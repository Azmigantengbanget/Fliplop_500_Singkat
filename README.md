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

  { "en": "Apa itu flip-flop?", "id": "Rangkaian elektronika dua kondisi stabil." },
  { "en": "Berapa output flip-flop?", "id": "Dua output saling komplemen." },
  { "en": "Siapa penemu flip-flop?", "id": "William Eccles dan F.W. Jordan." },
  { "en": "Kapan flip-flop ditemukan?", "id": "Tahun 1918 oleh fisikawan Inggris." },
  { "en": "Apa fungsi flip-flop?", "id": "Menyimpan informasi dalam sistem biner." },
  { "en": "Apa sifat multivibrator bistabil?", "id": "Dua tingkat tegangan keluaran stabil." },
  { "en": "Kapan flip-flop berubah?", "id": "Saat dipicu oleh sinyal trigger." },
  { "en": "Berapa jenis flip-flop utama?", "id": "Empat jenis utama flip-flop." },
  { "en": "Apa kepanjangan SR flip-flop?", "id": "Set dan Reset flip-flop." },
  { "en": "Berapa input SR flip-flop?", "id": "Dua input S dan R." },
  { "en": "Apa arti S pada SR?", "id": "S berarti Set atau mengatur." },
  { "en": "Apa arti R pada SR?", "id": "R berarti Reset atau mereset." },
  { "en": "Berapa output SR flip-flop?", "id": "Dua output Q dan Q'." },
  { "en": "Apa hubungan Q dan Q'?", "id": "Q' adalah komplemen dari Q." },
  { "en": "Gerbang apa untuk SR?", "id": "Gerbang NOR atau NAND." },
  { "en": "Apa kondisi S=1, R=0?", "id": "Q=1 dan Q'=0 (Set)." },
  { "en": "Apa kondisi S=0, R=1?", "id": "Q=0 dan Q'=1 (Reset)." },
  { "en": "Apa kondisi S=0, R=0?", "id": "Keadaan sebelumnya tetap dipertahankan." },
  { "en": "Apa kondisi S=1, R=1?", "id": "Kondisi terlarang atau tidak diizinkan." },
  { "en": "Apa dasar D flip-flop?", "id": "Modifikasi dari SR flip-flop." },
  { "en": "Berapa input D flip-flop?", "id": "Satu input yaitu D." },
  { "en": "Apa tambahan D flip-flop?", "id": "Gerbang NOT dari S ke R." },
  { "en": "Apa fungsi gerbang NOT?", "id": "Membalik sinyal input S." },
  { "en": "Apa keunggulan D flip-flop?", "id": "Tidak ada kondisi terlarang." },
  { "en": "Apa kepanjangan JK flip-flop?", "id": "Jack Kilby flip-flop elektronik." },
  { "en": "Berapa input JK flip-flop?", "id": "Tiga input J, K, Clock." },
  { "en": "Apa kelebihan JK flip-flop?", "id": "Tidak ada kondisi terlarang." },
  { "en": "Apa dasar JK flip-flop?", "id": "Pengembangan dari SR flip-flop." },
  { "en": "Kapan JK berubah?", "id": "Saat ada sinyal clock." },
  { "en": "Apa kondisi J=0, K=0?", "id": "Keadaan sebelumnya tetap dipertahankan." },
  { "en": "Apa kondisi J=0, K=1?", "id": "Output Q menjadi 0 (Reset)." },
  { "en": "Apa kondisi J=1, K=0?", "id": "Output Q menjadi 1 (Set)." },
  { "en": "Apa kondisi J=1, K=1?", "id": "Output berubah menjadi kebalikannya." },
  { "en": "Apa dasar T flip-flop?", "id": "Bentuk sederhana JK flip-flop." },
  { "en": "Bagaimana T flip-flop dibuat?", "id": "Menghubungkan input J dan K." },
  { "en": "Apa nama lain T?", "id": "Single JK flip-flop elektronik." },
  { "en": "Apa fungsi T flip-flop?", "id": "Membalik output sebelumnya." },
  { "en": "Kapan T berubah?", "id": "Saat input T berlogika tinggi." },
  { "en": "Kapan T tetap?", "id": "Saat input T berlogika rendah." },
  { "en": "Apa aplikasi flip-flop?", "id": "Memory komputer dan smartphone." },
  { "en": "Apa fungsi lain flip-flop?", "id": "Penghitung detak dan sinkronisasi." },
  { "en": "Apa itu Master-Slave?", "id": "JK flip-flop dengan dua tahap." },
  { "en": "Mengapa disebut flip-flop?", "id": "Output selalu dalam keadaan berlawanan." },
  { "en": "Apa keadaan flip?", "id": "Level satu atau logika tinggi." },
  { "en": "Apa keadaan flop?", "id": "Level nol atau logika rendah." },
  { "en": "Apa itu latch?", "id": "Rangkaian dasar flip-flop sederhana." },
  { "en": "Apa beda latch dan flip-flop?", "id": "Latch tidak memerlukan sinyal clock." },
  { "en": "Apa itu clock?", "id": "Sinyal pemicu perubahan flip-flop." },
  { "en": "Apa fungsi clock?", "id": "Mengatur waktu perubahan output." },
  { "en": "Apa itu trigger?", "id": "Sinyal pemicu perubahan keadaan." },
  { "en": "Apa itu edge trigger?", "id": "Pemicu pada tepi sinyal clock." },
  { "en": "Apa itu level trigger?", "id": "Pemicu pada level sinyal clock." },
  { "en": "Apa itu positive edge?", "id": "Pemicu pada tepi naik clock." },
  { "en": "Apa itu negative edge?", "id": "Pemicu pada tepi turun clock." },
  { "en": "Apa itu setup time?", "id": "Waktu input stabil sebelum clock." },
  { "en": "Apa itu hold time?", "id": "Waktu input stabil setelah clock." },
  { "en": "Apa itu propagation delay?", "id": "Waktu tunda perubahan output." },
  { "en": "Apa komponen dasar flip-flop?", "id": "Transistor dan resistor." },
  { "en": "Berapa transistor flip-flop diskrit?", "id": "Dua transistor bipolar." },
  { "en": "Apa fungsi resistor kolektor?", "id": "Membatasi arus pada kolektor." },
  { "en": "Apa fungsi resistor basis?", "id": "Membatasi arus pada basis." },
  { "en": "Apa prinsip kerja flip-flop?", "id": "Transistor sebagai saklar elektronik." },
  { "en": "Kapan transistor ON?", "id": "Saat basis diberi tegangan positif." },
  { "en": "Kapan transistor OFF?", "id": "Saat basis tidak diberi tegangan." },
  { "en": "Apa itu keadaan stabil?", "id": "Output tidak berubah tanpa trigger." },
  { "en": "Berapa keadaan stabil flip-flop?", "id": "Dua keadaan stabil." },
  { "en": "Apa keadaan stabil pertama?", "id": "Q=1 dan Q'=0." },
  { "en": "Apa keadaan stabil kedua?", "id": "Q=0 dan Q'=1." },
  { "en": "Apa itu memory element?", "id": "Elemen penyimpan dalam sistem biner." },
  { "en": "Apa aplikasi dalam komputer?", "id": "Penyimpan data memory komputer." },
  { "en": "Apa aplikasi dalam counter?", "id": "Penghitung pulsa atau detak." },
  { "en": "Apa itu register?", "id": "Kumpulan flip-flop untuk data." },
  { "en": "Apa itu shift register?", "id": "Register yang dapat menggeser data." },
  { "en": "Apa itu synchronous?", "id": "Semua flip-flop dikontrol clock sama." },
  { "en": "Apa itu asynchronous?", "id": "Flip-flop tidak dikontrol clock sama." },
  { "en": "Apa itu preset?", "id": "Mengatur output Q=1 langsung." },
  { "en": "Apa itu clear?", "id": "Mengatur output Q=0 langsung." },
  { "en": "Apa beda preset dan set?", "id": "Preset tidak memerlukan clock." },
  { "en": "Apa beda clear dan reset?", "id": "Clear tidak memerlukan clock." },
  { "en": "Apa itu race condition?", "id": "Kondisi tidak stabil pada output." },
  { "en": "Mengapa ada race condition?", "id": "Delay berbeda pada gerbang logika." },
  { "en": "Bagaimana mengatasi race condition?", "id": "Menggunakan Master-Slave flip-flop." },
  { "en": "Apa itu metastability?", "id": "Keadaan tidak stabil sementara." },
  { "en": "Kapan terjadi metastability?", "id": "Saat input berubah dekat clock." },
  { "en": "Apa bahaya metastability?", "id": "Output tidak dapat diprediksi." },
  { "en": "Apa itu glitch?", "id": "Pulsa pendek tidak diinginkan." },
  { "en": "Apa penyebab glitch?", "id": "Delay berbeda pada jalur sinyal." },
  { "en": "Apa itu fan-out?", "id": "Jumlah input yang dapat dikontrol." },
  { "en": "Apa itu fan-in?", "id": "Jumlah input pada satu gerbang." },
  { "en": "Apa itu noise margin?", "id": "Toleransi terhadap gangguan sinyal." },
  { "en": "Apa itu power consumption?", "id": "Konsumsi daya flip-flop." },
  { "en": "Apa itu operating frequency?", "id": "Frekuensi maksimum operasi flip-flop." },
  { "en": "Apa teknologi TTL?", "id": "Transistor-Transistor Logic flip-flop." },
  { "en": "Apa teknologi CMOS?", "id": "Complementary Metal-Oxide Semiconductor." },
  { "en": "Apa keunggulan CMOS?", "id": "Konsumsi daya rendah." },
  { "en": "Apa keunggulan TTL?", "id": "Kecepatan switching tinggi." },
  { "en": "Apa itu IC flip-flop?", "id": "Integrated Circuit flip-flop." },
  { "en": "Contoh IC SR flip-flop?", "id": "74LS279 quad SR latch." },
  { "en": "Contoh IC D flip-flop?", "id": "7474 dual D flip-flop." },
  { "en": "Contoh IC JK flip-flop?", "id": "7476 dual JK flip-flop." },
  { "en": "Apa itu quad flip-flop?", "id": "Empat flip-flop dalam satu IC." },
  { "en": "Apa itu dual flip-flop?", "id": "Dua flip-flop dalam satu IC." },
  { "en": "Apa itu hex flip-flop?", "id": "Enam flip-flop dalam satu IC." },
  { "en": "Apa itu octal flip-flop?", "id": "Delapan flip-flop dalam satu IC." },
  { "en": "Apa simbol Q?", "id": "Output normal flip-flop." },
  { "en": "Apa simbol Q'?", "id": "Output komplemen flip-flop." },
  { "en": "Apa simbol CLK?", "id": "Input clock flip-flop." },
  { "en": "Apa simbol PR?", "id": "Input preset flip-flop." },
  { "en": "Apa simbol CLR?", "id": "Input clear flip-flop." },
  { "en": "Apa logika positif?", "id": "Logika 1 adalah tegangan tinggi." },
  { "en": "Apa logika negatif?", "id": "Logika 1 adalah tegangan rendah." },
  { "en": "Apa itu truth table?", "id": "Tabel kebenaran flip-flop." },
  { "en": "Apa itu state diagram?", "id": "Diagram keadaan flip-flop." },
  { "en": "Apa itu timing diagram?", "id": "Diagram waktu sinyal flip-flop." },
  { "en": "Apa itu characteristic equation?", "id": "Persamaan karakteristik flip-flop." },
  { "en": "Apa itu excitation table?", "id": "Tabel eksitasi flip-flop." },
  { "en": "Apa fungsi excitation table?", "id": "Menentukan input untuk transisi output." },
  { "en": "Apa itu state transition?", "id": "Perubahan keadaan flip-flop." },
  { "en": "Apa itu next state?", "id": "Keadaan flip-flop setelah clock." },
  { "en": "Apa itu present state?", "id": "Keadaan flip-flop saat ini." },
  { "en": "Apa itu binary counter?", "id": "Pencacah biner menggunakan flip-flop." },
  { "en": "Apa itu ripple counter?", "id": "Counter dengan propagasi berurutan." },
  { "en": "Apa itu synchronous counter?", "id": "Counter dengan clock bersamaan." },
  { "en": "Apa itu up counter?", "id": "Counter yang menghitung naik." },
  { "en": "Apa itu down counter?", "id": "Counter yang menghitung turun." },
  { "en": "Apa itu ring counter?", "id": "Counter dengan output melingkar." },
  { "en": "Apa itu Johnson counter?", "id": "Counter dengan feedback terbalik." },
  { "en": "Apa itu frequency divider?", "id": "Pembagi frekuensi menggunakan flip-flop." },
  { "en": "Apa itu decade counter?", "id": "Counter yang menghitung 0-9." },
  { "en": "Apa itu modulo counter?", "id": "Counter dengan modulus tertentu." },
  { "en": "Apa itu parallel load?", "id": "Memuat data secara bersamaan." },
  { "en": "Apa itu serial load?", "id": "Memuat data secara berurutan." },
  { "en": "Apa itu SISO?", "id": "Serial In Serial Out." },
  { "en": "Apa itu SIPO?", "id": "Serial In Parallel Out." },
  { "en": "Apa itu PISO?", "id": "Parallel In Serial Out." },
  { "en": "Apa itu PIPO?", "id": "Parallel In Parallel Out." },
  { "en": "Apa itu left shift?", "id": "Menggeser data ke kiri." },
  { "en": "Apa itu right shift?", "id": "Menggeser data ke kanan." },
  { "en": "Apa itu circular shift?", "id": "Menggeser data secara melingkar." },
  { "en": "Apa itu arithmetic shift?", "id": "Menggeser dengan tanda bilangan." },
  { "en": "Apa itu logical shift?", "id": "Menggeser tanpa mempertahankan tanda." },
  { "en": "Apa itu bidirectional shift?", "id": "Menggeser ke kiri dan kanan." },
  { "en": "Apa itu universal shift?", "id": "Register dengan semua fungsi shift." },
  { "en": "Apa itu storage register?", "id": "Register untuk menyimpan data." },
  { "en": "Apa itu buffer register?", "id": "Register untuk menahan data sementara." },
  { "en": "Apa itu accumulator?", "id": "Register untuk operasi aritmatika." },
  { "en": "Apa itu program counter?", "id": "Register untuk alamat instruksi." },
  { "en": "Apa itu instruction register?", "id": "Register untuk menyimpan instruksi." },
  { "en": "Apa itu address register?", "id": "Register untuk menyimpan alamat." },
  { "en": "Apa itu data register?", "id": "Register untuk menyimpan data." },
  { "en": "Apa itu status register?", "id": "Register untuk menyimpan status." },
  { "en": "Apa itu flag register?", "id": "Register untuk menyimpan flag." },
  { "en": "Apa itu stack pointer?", "id": "Register untuk alamat stack." },
  { "en": "Apa itu index register?", "id": "Register untuk pengindeksan." },
  { "en": "Apa itu base register?", "id": "Register untuk alamat dasar." },
  { "en": "Apa itu general purpose register?", "id": "Register untuk keperluan umum." },
  { "en": "Apa itu special purpose register?", "id": "Register untuk keperluan khusus." },
  { "en": "Apa itu volatile memory?", "id": "Memory yang hilang saat power off." },
  { "en": "Apa itu non-volatile memory?", "id": "Memory yang tidak hilang power off." },
  { "en": "Apa itu SRAM?", "id": "Static Random Access Memory." },
  { "en": "Apa itu DRAM?", "id": "Dynamic Random Access Memory." },
  { "en": "Apa itu ROM?", "id": "Read Only Memory." },
  { "en": "Apa itu PROM?", "id": "Programmable Read Only Memory." },
  { "en": "Apa itu EPROM?", "id": "Erasable Programmable Read Only Memory." },
  { "en": "Apa itu EEPROM?", "id": "Electrically Erasable Programmable ROM." },
  { "en": "Apa itu flash memory?", "id": "Non-volatile memory yang dapat ditulis." },
  { "en": "Apa itu cache memory?", "id": "Memory cepat untuk data sering diakses." },
  { "en": "Apa itu virtual memory?", "id": "Memory virtual menggunakan storage." },
  { "en": "Apa itu memory hierarchy?", "id": "Hierarki memory berdasarkan kecepatan." },
  { "en": "Apa itu memory mapping?", "id": "Pemetaan alamat memory." },
  { "en": "Apa itu memory protection?", "id": "Proteksi akses memory." },
  { "en": "Apa itu memory management?", "id": "Manajemen penggunaan memory." },
  { "en": "Apa itu memory allocation?", "id": "Alokasi ruang memory." },
  { "en": "Apa itu memory deallocation?", "id": "Pembebasan ruang memory." },
  { "en": "Apa itu memory leak?", "id": "Kebocoran memory yang tidak dibebaskan." },
  { "en": "Apa itu memory fragmentation?", "id": "Fragmentasi ruang memory." },
  { "en": "Apa itu memory compaction?", "id": "Pemadatan memory untuk defragmentasi." },
  { "en": "Apa itu memory bank?", "id": "Kelompok memory yang diakses bersamaan." },
  { "en": "Apa itu memory interleaving?", "id": "Teknik akses memory bergantian." },
  { "en": "Apa itu memory controller?", "id": "Kontroler untuk mengatur akses memory." },
  { "en": "Apa itu memory bus?", "id": "Bus untuk komunikasi dengan memory." },
  { "en": "Apa itu address bus?", "id": "Bus untuk mengirim alamat memory." },
  { "en": "Apa itu data bus?", "id": "Bus untuk mengirim data memory." },
  { "en": "Apa itu control bus?", "id": "Bus untuk sinyal kontrol memory." },
  { "en": "Apa itu memory cycle?", "id": "Siklus akses memory." },
  { "en": "Apa itu access time?", "id": "Waktu akses memory." },
  { "en": "Apa itu cycle time?", "id": "Waktu siklus memory." },
  { "en": "Apa itu refresh time?", "id": "Waktu refresh DRAM." },
  { "en": "Apa itu retention time?", "id": "Waktu penyimpanan data memory." },
  { "en": "Apa itu bandwidth?", "id": "Lebar pita transfer data." },
  { "en": "Apa itu throughput?", "id": "Jumlah data yang ditransfer." },
  { "en": "Apa itu latency?", "id": "Waktu tunda akses memory." },
  { "en": "Apa itu burst mode?", "id": "Mode transfer data beruntun." },
  { "en": "Apa itu page mode?", "id": "Mode akses memory per halaman." },
  { "en": "Apa itu nibble mode?", "id": "Mode akses memory 4 bit." },
  { "en": "Apa itu static mode?", "id": "Mode akses memory statis." },
  { "en": "Apa itu dynamic mode?", "id": "Mode akses memory dinamis." },
  { "en": "Apa itu synchronous mode?", "id": "Mode akses memory sinkron." },
  { "en": "Apa itu asynchronous mode?", "id": "Mode akses memory asinkron." },
  { "en": "Apa itu pipeline mode?", "id": "Mode akses memory pipeline." },
  { "en": "Apa itu interleaved mode?", "id": "Mode akses memory interleaved." },
  { "en": "Apa itu dual port?", "id": "Memory dengan dua port akses." },
  { "en": "Apa itu multi port?", "id": "Memory dengan banyak port akses." },
  { "en": "Apa itu single port?", "id": "Memory dengan satu port akses." },
  { "en": "Apa itu read operation?", "id": "Operasi membaca data memory." },
  { "en": "Apa itu write operation?", "id": "Operasi menulis data memory." },
  { "en": "Apa itu read-modify-write?", "id": "Operasi baca-ubah-tulis memory." },
  { "en": "Apa itu memory test?", "id": "Pengujian fungsi memory." },
  { "en": "Apa itu memory diagnostic?", "id": "Diagnosis kerusakan memory." },
  { "en": "Apa itu memory error?", "id": "Kesalahan pada memory." },
  { "en": "Apa itu parity bit?", "id": "Bit untuk deteksi error memory." },
  { "en": "Apa itu ECC?", "id": "Error Correcting Code memory." },
  { "en": "Apa itu checksum?", "id": "Jumlah kontrol untuk verifikasi data." },
  { "en": "Apa itu CRC?", "id": "Cyclic Redundancy Check." },
  { "en": "Apa itu hamming code?", "id": "Kode Hamming untuk koreksi error." },
  { "en": "Apa itu redundancy?", "id": "Duplikasi untuk keandalan system." },
  { "en": "Apa itu fault tolerance?", "id": "Toleransi terhadap kesalahan system." },
  { "en": "Apa itu reliability?", "id": "Keandalan system elektronik." },
  { "en": "Apa itu availability?", "id": "Ketersediaan system untuk digunakan." },
  { "en": "Apa itu maintainability?", "id": "Kemudahan perawatan system." },
  { "en": "Apa itu testability?", "id": "Kemudahan pengujian system." },
  { "en": "Apa itu observability?", "id": "Kemudahan observasi internal system." },
  { "en": "Apa itu controllability?", "id": "Kemudahan kontrol internal system." },
  { "en": "Apa itu design for test?", "id": "Desain untuk kemudahan pengujian." },
  { "en": "Apa itu built-in test?", "id": "Pengujian terintegrasi dalam system." },
  { "en": "Apa itu boundary scan?", "id": "Teknik pengujian menggunakan scan chain." },
  { "en": "Apa itu scan chain?", "id": "Rantai scan untuk pengujian." },
  { "en": "Apa itu test vector?", "id": "Vektor uji untuk pengujian." },
  { "en": "Apa itu test pattern?", "id": "Pola uji untuk pengujian." },
  { "en": "Apa itu test coverage?", "id": "Cakupan pengujian system." },
  { "en": "Apa itu fault coverage?", "id": "Cakupan deteksi kesalahan." },
  { "en": "Apa itu stuck-at fault?", "id": "Kesalahan stuck pada level tertentu." },
  { "en": "Apa itu bridging fault?", "id": "Kesalahan hubung singkat antar jalur." },
  { "en": "Apa itu delay fault?", "id": "Kesalahan waktu tunda sinyal." },
  { "en": "Apa itu parametric fault?", "id": "Kesalahan parameter listrik." },
  { "en": "Apa itu functional fault?", "id": "Kesalahan fungsi logika." },
  { "en": "Apa itu structural fault?", "id": "Kesalahan struktur fisik." },
  { "en": "Apa itu intermittent fault?", "id": "Kesalahan yang muncul sesekali." },
  { "en": "Apa itu permanent fault?", "id": "Kesalahan yang bersifat permanen." },
  { "en": "Apa itu transient fault?", "id": "Kesalahan yang bersifat sementara." },
  { "en": "Apa itu soft error?", "id": "Error sementara karena radiasi." },
  { "en": "Apa itu hard error?", "id": "Error permanen karena kerusakan fisik." },
  { "en": "Apa itu single event upset?", "id": "Gangguan tunggal karena radiasi." },
  { "en": "Apa itu multiple event upset?", "id": "Gangguan ganda karena radiasi." },
  { "en": "Apa itu latch-up?", "id": "Kondisi parasitik pada CMOS." },
  { "en": "Apa itu electrostatic discharge?", "id": "Pelepasan muatan listrik statis." },
  { "en": "Apa itu electromagnetic interference?", "id": "Gangguan elektromagnetik." },
  { "en": "Apa itu crosstalk?", "id": "Gangguan antar jalur sinyal." },
  { "en": "Apa itu ground bounce?", "id": "Gangguan pada jalur ground." },
  { "en": "Apa itu power supply noise?", "id": "Gangguan pada jalur power supply." },
  { "en": "Apa itu thermal noise?", "id": "Gangguan karena suhu." },
  { "en": "Apa itu shot noise?", "id": "Gangguan karena diskretisasi muatan." },
  { "en": "Apa itu flicker noise?", "id": "Gangguan frekuensi rendah." },
  { "en": "Apa itu white noise?", "id": "Gangguan dengan spektrum datar." },
  { "en": "Apa itu pink noise?", "id": "Gangguan dengan spektrum 1/f." },
  { "en": "Apa itu signal integrity?", "id": "Integritas sinyal dalam transmisi." },
  { "en": "Apa itu power integrity?", "id": "Integritas distribusi daya." },
  { "en": "Apa itu timing integrity?", "id": "Integritas waktu sinyal." },
  { "en": "Apa itu jitter?", "id": "Variasi waktu sinyal clock." },
  { "en": "Apa itu skew?", "id": "Perbedaan waktu antar sinyal." },
  { "en": "Apa itu duty cycle?", "id": "Perbandingan waktu high dan low." },
  { "en": "Apa itu rise time?", "id": "Waktu naik sinyal." },
  { "en": "Apa itu fall time?", "id": "Waktu turun sinyal." },
  { "en": "Apa itu slew rate?", "id": "Laju perubahan sinyal." },
  { "en": "Apa itu overshoot?", "id": "Sinyal melebihi level target." },
  { "en": "Apa itu undershoot?", "id": "Sinyal kurang dari level target." },
  { "en": "Apa itu ringing?", "id": "Osilasi sinyal setelah transisi." },
  { "en": "Apa itu reflection?", "id": "Pantulan sinyal pada jalur transmisi." },
  { "en": "Apa itu impedance matching?", "id": "Penyesuaian impedansi jalur transmisi." },
  { "en": "Apa itu termination?", "id": "Terminasi jalur transmisi." },
  { "en": "Apa itu characteristic impedance?", "id": "Impedansi karakteristik jalur transmisi." },
  { "en": "Apa itu transmission line?", "id": "Jalur transmisi sinyal." },
  { "en": "Apa itu stripline?", "id": "Jalur transmisi terkubur." },
  { "en": "Apa itu microstrip?", "id": "Jalur transmisi permukaan." },
  { "en": "Apa itu coplanar waveguide?", "id": "Jalur transmisi coplanar." },
  { "en": "Apa itu differential pair?", "id": "Pasangan jalur sinyal diferensial." },
  { "en": "Apa itu single-ended?", "id": "Sinyal referensi ground tunggal." },
  { "en": "Apa itu differential?", "id": "Sinyal dengan referensi satu sama lain." },
  { "en": "Apa itu common mode?", "id": "Sinyal yang sama pada kedua jalur." },
  { "en": "Apa itu differential mode?", "id": "Sinyal yang berlawanan pada jalur." },
  { "en": "Apa itu LVDS?", "id": "Low Voltage Differential Signaling." },
  { "en": "Apa itu PECL?", "id": "Positive Emitter Coupled Logic." },
  { "en": "Apa itu CML?", "id": "Current Mode Logic." },
  { "en": "Apa itu ECL?", "id": "Emitter Coupled Logic." },
  { "en": "Apa itu GTL?", "id": "Gunning Transceiver Logic." },
  { "en": "Apa itu HSTL?", "id": "High Speed Transceiver Logic." },
  { "en": "Apa itu SSTL?", "id": "Stub Series Terminated Logic." },
  { "en": "Apa itu LVCMOS?", "id": "Low Voltage CMOS." },
  { "en": "Apa itu LVTTL?", "id": "Low Voltage TTL." },
  { "en": "Apa keunggulan LVDS?", "id": "Konsumsi daya rendah, kecepatan tinggi." },
  { "en": "Apa keunggulan ECL?", "id": "Kecepatan switching sangat tinggi." },
  { "en": "Apa keunggulan CMOS?", "id": "Konsumsi daya sangat rendah." },
  { "en": "Apa keunggulan TTL?", "id": "Kompatibilitas dan ketersediaan luas." },
  { "en": "Apa itu voltage swing?", "id": "Rentang tegangan sinyal." },
  { "en": "Apa itu current drive?", "id": "Kemampuan mengeluarkan arus." },
  { "en": "Apa itu input impedance?", "id": "Impedansi input gerbang logika." },
  { "en": "Apa itu output impedance?", "id": "Impedansi output gerbang logika." },
  { "en": "Apa itu loading effect?", "id": "Efek pembebanan pada sinyal." },
  { "en": "Apa itu buffering?", "id": "Penguatan sinyal menggunakan buffer." },
  { "en": "Apa itu isolation?", "id": "Isolasi antar bagian rangkaian." },
  { "en": "Apa itu level shifting?", "id": "Pergeseran level tegangan sinyal." },
  { "en": "Apa itu voltage translation?", "id": "Translasi tegangan antar domain." },
  { "en": "Apa itu clock domain?", "id": "Domain dengan clock yang sama." },
  { "en": "Apa itu clock domain crossing?", "id": "Penyeberangan antar domain clock." },
  { "en": "Apa itu metastability resolution?", "id": "Resolusi kondisi metastabil." },
  { "en": "Apa itu synchronizer?", "id": "Rangkaian untuk sinkronisasi sinyal." },
  { "en": "Apa itu arbiter?", "id": "Rangkaian untuk arbitrasi akses." },
  { "en": "Apa itu mutex?", "id": "Mutual exclusion untuk akses eksklusif." },
  { "en": "Apa itu handshaking?", "id": "Protokol komunikasi dua arah." },
  { "en": "Apa itu flow control?", "id": "Kontrol aliran data." },
  { "en": "Apa itu back pressure?", "id": "Tekanan balik untuk kontrol aliran." },
  { "en": "Apa itu credit-based?", "id": "Sistem kontrol berdasarkan kredit." },
  { "en": "Apa itu token-based?", "id": "Sistem kontrol berdasarkan token." },
  { "en": "Apa itu FIFO?", "id": "First In First Out." },
  { "en": "Apa itu LIFO?", "id": "Last In First Out." },
  { "en": "Apa itu queue?", "id": "Antrian data FIFO." },
  { "en": "Apa itu stack?", "id": "Tumpukan data LIFO." },
  { "en": "Apa itu circular buffer?", "id": "Buffer melingkar untuk data." },
  { "en": "Apa itu ring buffer?", "id": "Buffer cincin untuk data." },
  { "en": "Apa itu double buffer?", "id": "Dua buffer untuk ping-pong." },
  { "en": "Apa itu triple buffer?", "id": "Tiga buffer untuk pipeline." },
  { "en": "Apa itu ping-pong buffer?", "id": "Buffer bergantian untuk kontinuitas." },
  { "en": "Apa itu mailbox?", "id": "Kotak surat untuk komunikasi." },
  { "en": "Apa itu semaphore?", "id": "Semafor untuk sinkronisasi." },
  { "en": "Apa itu event?", "id": "Kejadian untuk sinkronisasi." },
  { "en": "Apa itu interrupt?", "id": "Interupsi untuk penanganan kejadian." },
  { "en": "Apa itu polling?", "id": "Pengecekan status secara berkala." },
  { "en": "Apa itu DMA?", "id": "Direct Memory Access." },
  { "en": "Apa itu bus mastering?", "id": "Penguasaan bus oleh perangkat." },
  { "en": "Apa itu arbitration?", "id": "Arbitrasi akses bus." },
  { "en": "Apa itu priority?", "id": "Prioritas akses bus." },
  { "en": "Apa itu round-robin?", "id": "Algoritma penjadwalan bergilir." },
  { "en": "Apa itu fair queuing?", "id": "Antrian yang adil." },
  { "en": "Apa itu weighted fair queuing?", "id": "Antrian adil berbobot." },
  { "en": "Apa itu quality of service?", "id": "Kualitas layanan sistem." },
  { "en": "Apa itu real-time?", "id": "Sistem dengan batasan waktu." },
  { "en": "Apa itu hard real-time?", "id": "Real-time dengan deadline keras." },
  { "en": "Apa itu soft real-time?", "id": "Real-time dengan deadline lunak." },
  { "en": "Apa itu deterministic?", "id": "Sistem dengan hasil dapat diprediksi." },
  { "en": "Apa itu non-deterministic?", "id": "Sistem dengan hasil tidak pasti." },
  { "en": "Apa itu stochastic?", "id": "Sistem dengan elemen acak." },
  { "en": "Apa itu probabilistic?", "id": "Sistem berdasarkan probabilitas." },
  { "en": "Apa itu statistical?", "id": "Analisis berdasarkan statistik." },
  { "en": "Apa itu monte carlo?", "id": "Simulasi menggunakan sampling acak." },
  { "en": "Apa itu worst-case?", "id": "Analisis kasus terburuk." },
  { "en": "Apa itu best-case?", "id": "Analisis kasus terbaik." },
  { "en": "Apa itu average-case?", "id": "Analisis kasus rata-rata." },
  { "en": "Apa itu typical-case?", "id": "Analisis kasus tipikal." },
  { "en": "Apa itu corner case?", "id": "Kasus ekstrem dalam analisis." },
  { "en": "Apa itu stress test?", "id": "Pengujian dengan beban maksimal." },
  { "en": "Apa itu burn-in test?", "id": "Pengujian dengan operasi lama." },
  { "en": "Apa itu accelerated test?", "id": "Pengujian dengan kondisi dipercepat." },
  { "en": "Apa itu life test?", "id": "Pengujian umur komponen." },
  { "en": "Apa itu reliability test?", "id": "Pengujian keandalan sistem." },
  { "en": "Apa itu MTBF?", "id": "Mean Time Between Failures." },
  { "en": "Apa itu MTTR?", "id": "Mean Time To Repair." },
  { "en": "Apa itu MTTF?", "id": "Mean Time To Failure." },
  { "en": "Apa itu availability calculation?", "id": "Perhitungan ketersediaan sistem." },
  { "en": "Apa itu failure rate?", "id": "Laju kegagalan komponen." },
  { "en": "Apa itu bathtub curve?", "id": "Kurva laju kegagalan komponen." },
  { "en": "Apa itu infant mortality?", "id": "Kegagalan dini komponen." },
  { "en": "Apa itu wear-out?", "id": "Kegagalan karena keausan." },
  { "en": "Apa itu random failure?", "id": "Kegagalan acak komponen." },
  { "en": "Apa itu systematic failure?", "id": "Kegagalan sistematis komponen." },
  { "en": "Apa itu common cause failure?", "id": "Kegagalan dengan penyebab sama." },
  { "en": "Apa itu cascade failure?", "id": "Kegagalan berantai sistem." },
  { "en": "Apa itu single point failure?", "id": "Kegagalan pada satu titik." },
  { "en": "Apa itu graceful degradation?", "id": "Degradasi bertahap kinerja sistem." },
  { "en": "Apa itu fail-safe?", "id": "Sistem gagal ke kondisi aman." },
  { "en": "Apa itu fail-secure?", "id": "Sistem gagal ke kondisi aman." },
  { "en": "Apa itu fail-operational?", "id": "Sistem tetap beroperasi saat gagal." },
  { "en": "Apa itu redundancy dalam flip-flop?", "id": "Duplikasi untuk keandalan rangkaian." },
  { "en": "Apa itu asynchronous input?", "id": "Input tanpa sinkronisasi clock." },
  { "en": "Apa itu synchronous input?", "id": "Input disinkronkan dengan clock." },
  { "en": "Apa itu forbidden state?", "id": "Kondisi output tidak terdefinisi." },
  { "en": "Apa itu toggle mode?", "id": "Mode membalik keadaan output." },
  { "en": "Apa fungsi utama flip-flop?", "id": "Penyimpanan satu bit data." },
  { "en": "Apa itu set-dominant?", "id": "Set lebih prioritas dari reset." },
  { "en": "Apa itu reset-dominant?", "id": "Reset lebih prioritas dari set." },
  { "en": "Apa itu race around condition?", "id": "Output berubah cepat berulang-ulang." },
  { "en": "Bagaimana mencegah race around?", "id": "Gunakan master-slave atau edge trigger." },
  { "en": "Apa itu pulse-triggered?", "id": "Dipicu oleh pulsa clock lebar." },
  { "en": "Apa itu edge-triggered?", "id": "Dipicu pada tepi clock." },
  { "en": "Apa itu positive logic?", "id": "Logika tinggi sebagai aktif." },
  { "en": "Apa itu negative logic?", "id": "Logika rendah sebagai aktif." },
  { "en": "Apa itu Q output?", "id": "Output utama flip-flop." },
  { "en": "Apa itu Q bar?", "id": "Output komplemen flip-flop." },
  { "en": "Apa itu asynchronous reset?", "id": "Reset tanpa menunggu clock." },
  { "en": "Apa itu synchronous reset?", "id": "Reset saat clock aktif." },
  { "en": "Apa itu asynchronous set?", "id": "Set tanpa menunggu clock." },
  { "en": "Apa itu synchronous set?", "id": "Set saat clock aktif." },
  { "en": "Apa itu active high?", "id": "Aktif pada level logika tinggi." },
  { "en": "Apa itu active low?", "id": "Aktif pada level logika rendah." },
  { "en": "Apa itu preset pada flip-flop?", "id": "Langsung mengatur Q ke 1." },
  { "en": "Apa itu clear pada flip-flop?", "id": "Langsung mengatur Q ke 0." },
  { "en": "Apa itu hold state?", "id": "Keadaan output tetap stabil." },
  { "en": "Apa itu memory cell?", "id": "Sel penyimpan satu bit data." },
  { "en": "Apa peran flip-flop di register?", "id": "Sebagai elemen penyimpan bit." },
  { "en": "Apa itu clock enable?", "id": "Mengaktifkan atau menonaktifkan clock." },
  { "en": "Apa itu scan flip-flop?", "id": "Flip-flop dengan mode scan." },
  { "en": "Apa itu scan chain pada IC?", "id": "Rangkaian flip-flop untuk test." },
  { "en": "Apa itu scan mode?", "id": "Mode pengujian rangkaian digital." },
  { "en": "Apa itu shift register?", "id": "Rangkaian flip-flop untuk geser data." },
  { "en": "Apa itu bit shifting?", "id": "Menggeser posisi bit data." },
  { "en": "Apa itu serial data?", "id": "Data dikirim satu per satu." },
  { "en": "Apa itu parallel data?", "id": "Data dikirim secara bersamaan." },
  { "en": "Apa itu D latch?", "id": "Latch dengan satu input D." },
  { "en": "Apa itu transparent latch?", "id": "Latch transparan saat enable aktif." },
  { "en": "Apa itu gated latch?", "id": "Latch dengan sinyal kendali." },
  { "en": "Apa itu enable pada latch?", "id": "Mengontrol kapan latch aktif." },
  { "en": "Apa itu level sensitive?", "id": "Responsif pada level sinyal." },
  { "en": "Apa itu edge sensitive?", "id": "Responsif pada tepi sinyal." },
  { "en": "Apa itu master flip-flop?", "id": "Bagian pertama master-slave." },
  { "en": "Apa itu slave flip-flop?", "id": "Bagian kedua master-slave." },
  { "en": "Apa itu propagation time?", "id": "Waktu propagasi perubahan output." },
  { "en": "Apa itu setup violation?", "id": "Setup time tidak terpenuhi." },
  { "en": "Apa itu hold violation?", "id": "Hold time tidak terpenuhi." },
  { "en": "Apa itu data retention?", "id": "Kemampuan menyimpan data stabil." },
  { "en": "Apa itu clock skew?", "id": "Perbedaan waktu antar clock." },
  { "en": "Apa itu metastable state?", "id": "Keadaan output tidak pasti." },
  { "en": "Apa itu pulse stretcher?", "id": "Memperpanjang lebar pulsa input." },
  { "en": "Apa itu debouncer?", "id": "Menghilangkan bouncing pada tombol." },
  { "en": "Apa itu clock divider?", "id": "Pembagi frekuensi clock." },
  { "en": "Apa itu frequency multiplier?", "id": "Pengganda frekuensi clock." },
  { "en": "Apa itu synchronous reset?", "id": "Reset pada tepi clock." },
  { "en": "Apa itu asynchronous clear?", "id": "Clear tanpa menunggu clock." },
  { "en": "Apa itu synchronous clear?", "id": "Clear pada tepi clock." },
  { "en": "Apa itu asynchronous preset?", "id": "Preset tanpa menunggu clock." },
  { "en": "Apa itu synchronous preset?", "id": "Preset pada tepi clock." },
  { "en": "Apa itu positive edge clock?", "id": "Tepi naik sinyal clock." },
  { "en": "Apa itu negative edge clock?", "id": "Tepi turun sinyal clock." },
  { "en": "Apa itu dual edge trigger?", "id": "Responsif pada dua tepi clock." },
  { "en": "Apa itu clock gating?", "id": "Menghemat daya dengan menonaktifkan clock." },
  { "en": "Apa itu flip-flop array?", "id": "Kumpulan flip-flop dalam satu chip." },
  { "en": "Apa itu scan path?", "id": "Jalur scan untuk pengujian IC." },
  { "en": "Apa itu stuck-at-1 fault?", "id": "Output selalu logika 1." },
  { "en": "Apa itu stuck-at-0 fault?", "id": "Output selalu logika 0." },
  { "en": "Apa itu clock tree?", "id": "Jalur distribusi clock pada chip." },
  { "en": "Apa itu clock domain?", "id": "Wilayah dengan clock berbeda." },
  { "en": "Apa itu clock crossing?", "id": "Perpindahan data antar clock domain." },
  { "en": "Apa itu synchronizer chain?", "id": "Rangkaian sinkronisasi antar domain clock." },
  { "en": "Apa itu data hazard?", "id": "Bahaya data tidak sinkron." },
  { "en": "Apa itu control hazard?", "id": "Bahaya kontrol tidak sinkron." },
  { "en": "Apa itu timing hazard?", "id": "Bahaya waktu tidak sinkron." },
  { "en": "Apa itu pulse width?", "id": "Lebar pulsa sinyal input." },
  { "en": "Apa itu signal integrity?", "id": "Kualitas sinyal selama transmisi." },
  { "en": "Apa itu power dissipation?", "id": "Daya yang hilang sebagai panas." },
  { "en": "Apa itu fanout pada flip-flop?", "id": "Jumlah beban output flip-flop." },
  { "en": "Apa itu fanin pada flip-flop?", "id": "Jumlah input ke flip-flop." },
  { "en": "Apa itu noise immunity?", "id": "Ketahanan terhadap gangguan listrik." },
  { "en": "Apa itu static hazard?", "id": "Output berubah sementara tidak diharapkan." },
  { "en": "Apa itu dynamic hazard?", "id": "Output berubah lebih dari sekali." },
  { "en": "Apa itu critical path?", "id": "Jalur waktu terlama dalam rangkaian." },
  { "en": "Apa itu hold path?", "id": "Jalur waktu terpendek dalam rangkaian." },
  { "en": "Apa itu setup path?", "id": "Jalur untuk memenuhi setup time." },
  { "en": "Apa itu hold time violation?", "id": "Pelanggaran waktu penahanan data." },
  { "en": "Apa itu setup time violation?", "id": "Pelanggaran waktu penyiapan data." },
  { "en": "Apa itu clock jitter?", "id": "Variasi waktu clock tidak stabil." },
  { "en": "Apa itu clock uncertainty?", "id": "Ketidakpastian waktu clock." },
  { "en": "Apa itu clock latency?", "id": "Waktu tunda distribusi clock." },
  { "en": "Apa itu clock period?", "id": "Jarak waktu antar pulsa clock." },
  { "en": "Apa itu duty cycle distortion?", "id": "Distorsi rasio high-low clock." },
  { "en": "Apa itu data path?", "id": "Jalur data antar flip-flop." },
  { "en": "Apa itu control path?", "id": "Jalur kontrol antar flip-flop." },
  { "en": "Apa itu synchronous design?", "id": "Desain dengan clock terpusat." },
  { "en": "Apa itu asynchronous design?", "id": "Desain tanpa clock terpusat." },
  { "en": "Apa itu clocked flip-flop?", "id": "Flip-flop dipicu sinyal clock." },
  { "en": "Apa itu transparent mode?", "id": "Mode output mengikuti input." },
  { "en": "Apa itu latched mode?", "id": "Mode output menyimpan input." },
  { "en": "Apa itu data valid window?", "id": "Jendela waktu data valid." },
  { "en": "Apa itu data invalid window?", "id": "Jendela waktu data tidak valid." },
  { "en": "Apa itu synchronous interface?", "id": "Antarmuka dengan clock bersama." },
  { "en": "Apa itu asynchronous interface?", "id": "Antarmuka tanpa clock bersama." },
  { "en": "Apa itu setup margin?", "id": "Cadangan waktu untuk setup." },
  { "en": "Apa itu hold margin?", "id": "Cadangan waktu untuk hold." },
  { "en": "Apa itu clock buffer?", "id": "Penguat sinyal clock." },
  { "en": "Apa itu clock inverter?", "id": "Pembalik sinyal clock." },
  { "en": "Apa itu clock divider?", "id": "Pembagi frekuensi clock." },
  { "en": "Apa itu clock multiplier?", "id": "Pengganda frekuensi clock." },
  { "en": "Apa itu clock synchronizer?", "id": "Sinkronisasi clock antar domain." },
  { "en": "Apa itu clock generator?", "id": "Pembuat sinyal clock." },
  { "en": "Apa itu clock recovery?", "id": "Pengambilan clock dari data." },
  { "en": "Apa itu clock alignment?", "id": "Penyelarasan fase clock." },
  { "en": "Apa itu clock phase?", "id": "Fase sinyal clock." },
  { "en": "Apa itu phase-locked loop?", "id": "Pengunci fase sinyal clock." },
  { "en": "Apa itu delay-locked loop?", "id": "Pengunci delay sinyal clock." },
  { "en": "Apa itu clock tree synthesis?", "id": "Sintesis jalur clock pada chip." },
  { "en": "Apa itu clock mesh?", "id": "Jaringan distribusi clock." },
  { "en": "Apa itu clock domain isolation?", "id": "Isolasi antar domain clock." },
  { "en": "Apa itu clock gating cell?", "id": "Sel untuk mengaktifkan clock." },
  { "en": "Apa itu clock enable signal?", "id": "Sinyal mengaktifkan clock." },
  { "en": "Apa itu scan enable?", "id": "Sinyal mengaktifkan mode scan." },
  { "en": "Apa itu test mode?", "id": "Mode pengujian rangkaian digital." },
  { "en": "Apa itu functional mode?", "id": "Mode operasi normal rangkaian." },
  { "en": "Apa itu scan data input?", "id": "Input data untuk mode scan." },
  { "en": "Apa itu scan data output?", "id": "Output data dari mode scan." },
  { "en": "Apa itu scan clock?", "id": "Clock khusus untuk mode scan." },
  { "en": "Apa itu scan chain insertion?", "id": "Penyisipan jalur scan pada IC." },
  { "en": "Apa itu scan chain testing?", "id": "Pengujian menggunakan jalur scan." },
  { "en": "Apa itu scan flip-flop insertion?", "id": "Penyisipan flip-flop scan." },
  { "en": "Apa itu scan flip-flop extraction?", "id": "Ekstraksi flip-flop scan." },
  { "en": "Apa itu scan path testing?", "id": "Pengujian jalur scan pada IC." },
  { "en": "Apa itu boundary scan cell?", "id": "Sel scan di tepi IC." },
  { "en": "Apa itu boundary scan register?", "id": "Register scan di tepi IC." },
  { "en": "Apa itu boundary scan test?", "id": "Pengujian boundary scan pada IC." },
  { "en": "Apa itu JTAG?", "id": "Standard boundary scan IEEE 1149.1." },
  { "en": "Apa itu TAP controller?", "id": "Pengontrol akses JTAG." },
  { "en": "Apa itu instruction register pada JTAG?", "id": "Register instruksi pada JTAG." },
  { "en": "Apa itu data register pada JTAG?", "id": "Register data pada JTAG." },
  { "en": "Apa itu TDI pada JTAG?", "id": "Test Data Input JTAG." },
  { "en": "Apa itu TDO pada JTAG?", "id": "Test Data Output JTAG." },
  { "en": "Apa itu TCK pada JTAG?", "id": "Test Clock JTAG." },
  { "en": "Apa itu TMS pada JTAG?", "id": "Test Mode Select JTAG." },
  { "en": "Apa itu TRST pada JTAG?", "id": "Test Reset JTAG." },
  { "en": "Apa itu scan insertion tool?", "id": "Alat penyisipan scan pada desain." },
  { "en": "Apa itu scan extraction tool?", "id": "Alat ekstraksi scan pada desain." },
  { "en": "Apa itu scan compression?", "id": "Kompresi data scan untuk test." },
  { "en": "Apa itu scan decompression?", "id": "Dekompresi data scan untuk test." },
  { "en": "Apa itu scan chain optimization?", "id": "Optimasi jalur scan pada IC." },
  { "en": "Apa itu scan chain balancing?", "id": "Penyeimbangan panjang jalur scan." },
  { "en": "Apa itu scan chain partitioning?", "id": "Pembagian jalur scan menjadi bagian." },
  { "en": "Apa itu scan chain reordering?", "id": "Pengurutan ulang jalur scan." },
  { "en": "Apa itu scan chain stitching?", "id": "Penggabungan jalur scan." },
  { "en": "Apa itu scan chain repair?", "id": "Perbaikan jalur scan rusak." },
  { "en": "Apa itu scan chain diagnosis?", "id": "Diagnosis masalah jalur scan." },
  { "en": "Apa itu scan chain coverage?", "id": "Cakupan pengujian jalur scan." },
  { "en": "Apa itu stuck-at fault coverage?", "id": "Cakupan deteksi stuck-at fault." },
  { "en": "Apa itu transition fault coverage?", "id": "Cakupan deteksi transition fault." },
  { "en": "Apa itu path delay fault?", "id": "Kesalahan delay pada jalur." },
  { "en": "Apa itu path delay fault coverage?", "id": "Cakupan deteksi path delay fault." },
  { "en": "Apa itu bridging fault coverage?", "id": "Cakupan deteksi bridging fault." },
  { "en": "Apa itu IDDQ testing?", "id": "Pengujian arus diam IC." },
  { "en": "Apa itu scan ATPG?", "id": "Automatic Test Pattern Generation scan." },
  { "en": "Apa itu test compression ratio?", "id": "Rasio kompresi data pengujian." },
  { "en": "Apa itu scan test time?", "id": "Waktu pengujian scan pada IC." },
  { "en": "Apa itu scan test cost?", "id": "Biaya pengujian scan pada IC." },
  { "en": "Apa itu scan test coverage?", "id": "Cakupan pengujian scan pada IC." },
  { "en": "Apa itu scan test escape?", "id": "Kesalahan lolos dari pengujian scan." },
  { "en": "Apa itu scan test yield?", "id": "Hasil produksi setelah pengujian scan." },
  { "en": "Apa itu scan test quality?", "id": "Kualitas hasil pengujian scan." },
  { "en": "Apa itu stuck-at fault model?", "id": "Model kesalahan stuck-at pada IC." },
  { "en": "Apa itu transition fault model?", "id": "Model kesalahan transisi pada IC." },
  { "en": "Apa itu bridging fault model?", "id": "Model kesalahan bridging pada IC." },
  { "en": "Apa itu delay fault model?", "id": "Model kesalahan delay pada IC." },
  { "en": "Apa itu IDDQ fault model?", "id": "Model kesalahan arus diam IC." },
  { "en": "Apa itu path delay model?", "id": "Model delay pada jalur IC." },
  { "en": "Apa itu stuck-open fault?", "id": "Kesalahan jalur terbuka pada IC." },
  { "en": "Apa itu stuck-short fault?", "id": "Kesalahan jalur pendek pada IC." },
  { "en": "Apa itu stuck-high fault?", "id": "Kesalahan jalur selalu tinggi." },
  { "en": "Apa itu stuck-low fault?", "id": "Kesalahan jalur selalu rendah." },
  { "en": "Apa itu scan chain fault?", "id": "Kesalahan pada jalur scan." },
  { "en": "Apa itu scan flip-flop fault?", "id": "Kesalahan pada flip-flop scan." },
  { "en": "Apa itu scan cell fault?", "id": "Kesalahan pada sel scan." },
  { "en": "Apa itu scan path fault?", "id": "Kesalahan pada jalur scan." },
  { "en": "Apa itu scan enable fault?", "id": "Kesalahan pada sinyal enable scan." },
  { "en": "Apa itu scan clock fault?", "id": "Kesalahan pada clock scan." },
  { "en": "Apa itu scan data fault?", "id": "Kesalahan pada data scan." },
  { "en": "Apa itu scan mode fault?", "id": "Kesalahan pada mode scan." },
  { "en": "Apa itu scan insertion fault?", "id": "Kesalahan pada penyisipan scan." },
  { "en": "Apa itu scan extraction fault?", "id": "Kesalahan pada ekstraksi scan." },
  { "en": "Apa itu scan compression fault?", "id": "Kesalahan pada kompresi scan." },
  { "en": "Apa itu scan decompression fault?", "id": "Kesalahan pada dekompresi scan." },
  { "en": "Apa itu scan optimization fault?", "id": "Kesalahan pada optimasi scan." },
  { "en": "Apa itu scan balancing fault?", "id": "Kesalahan pada penyeimbangan scan." },
  { "en": "Apa itu scan partitioning fault?", "id": "Kesalahan pada pembagian scan." },
  { "en": "Apa itu scan reordering fault?", "id": "Kesalahan pada pengurutan scan." },
  { "en": "Apa itu scan stitching fault?", "id": "Kesalahan pada penggabungan scan." },
  { "en": "Apa itu scan repair fault?", "id": "Kesalahan pada perbaikan scan." },
  { "en": "Apa itu scan diagnosis fault?", "id": "Kesalahan pada diagnosis scan." }



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
            }, 6500);
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
