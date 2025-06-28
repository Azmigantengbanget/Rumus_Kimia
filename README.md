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


  { "en": "Air?", "id": "H₂O." },
  { "en": "Garam Dapur (Natrium Klorida)?", "id": "NaCl." },
  { "en": "Karbon Dioksida?", "id": "CO₂." },
  { "en": "Gas Oksigen?", "id": "O₂." },
  { "en": "Gula Pasir (Sukrosa)?", "id": "C₁₂H₂₂O₁₁." },
  { "en": "Asam Cuka (Asam Asetat)?", "id": "CH₃COOH." },
  { "en": "Soda Kue (Natrium Bikarbonat)?", "id": "NaHCO₃." },
  { "en": "Asam Klorida (Asam Lambung)?", "id": "HCl." },
  { "en": "Asam Sulfat (Air Aki)?", "id": "H₂SO₄." },
  { "en": "Natrium Hidroksida (Soda Api)?", "id": "NaOH." },
  { "en": "Amonia?", "id": "NH₃." },
  { "en": "Gas Metana (Gas Alam)?", "id": "CH₄." },
  { "en": "Etanol (Alkohol)?", "id": "C₂H₅OH." },
  { "en": "Glukosa (Gula Darah)?", "id": "C₆H₁₂O₆." },
  { "en": "Kalsium Karbonat (Batu Kapur)?", "id": "CaCO₃." },
  { "en": "Karbon Monoksida?", "id": "CO." },
  { "en": "Gas Nitrogen?", "id": "N₂." },
  { "en": "Gas Hidrogen?", "id": "H₂." },
  { "en": "Ozon?", "id": "O₃." },
  { "en": "Asam Nitrat?", "id": "HNO₃." },
  { "en": "Kalsium Hidroksida (Kapur Padam)?", "id": "Ca(OH)₂." },
  { "en": "Hidrogen Peroksida?", "id": "H₂O₂." },
  { "en": "Silikon Dioksida (Pasir Kuarsa)?", "id": "SiO₂." },
  { "en": "Besi(III) Oksida (Karat)?", "id": "Fe₂O₃." },
  { "en": "Kalium Hidroksida?", "id": "KOH." },
  { "en": "Asam Fosfat?", "id": "H₃PO₄." },
  { "en": "Propana (Gas LPG)?", "id": "C₃H₈." },
  { "en": "Butana (Gas Korek Api)?", "id": "C₄H₁₀." },
  { "en": "Metanol (Spiritus)?", "id": "CH₃OH." },
  { "en": "Formaldehida (Formalin)?", "id": "CH₂O." },
  { "en": "Aseton (Pembersih Kuteks)?", "id": "C₃H₆O." },
  { "en": "Benzena?", "id": "C₆H₆." },
  { "en": "Hidrogen Sulfida (Bau Telur Busuk)?", "id": "H₂S." },
  { "en": "Sulfur Dioksida?", "id": "SO₂." },
  { "en": "Kalium Nitrat (Sendawa)?", "id": "KNO₃." },
  { "en": "Natrium Karbonat (Soda Cuci)?", "id": "Na₂CO₃." },
  { "en": "Amonium Klorida?", "id": "NH₄Cl." },
  { "en": "Tembaga(II) Sulfat (Terusi)?", "id": "CuSO₄." },
  { "en": "Magnesium Hidroksida (Obat Maag)?", "id": "Mg(OH)₂." },
  { "en": "Asam Karbonat (Minuman Bersoda)?", "id": "H₂CO₃." },
  { "en": "Asetilena (Gas Karbit)?", "id": "C₂H₂." },
  { "en": "Urea?", "id": "CO(NH₂)₂." },
  { "en": "Asam Sitrat (Pada Jeruk)?", "id": "C₆H₈O₇." },
  { "en": "Asam Laktat (Pada Susu)?", "id": "C₃H₆O₃." },
  { "en": "Gliserol (Gliserin)?", "id": "C₃H₈O₃." },
  { "en": "Fenol?", "id": "C₆H₅OH." },
  { "en": "Toluena?", "id": "C₇H₈." },
  { "en": "Kalium Permanganat?", "id": "KMnO₄." },
  { "en": "Natrium Tiosulfat?", "id": "Na₂S₂O₃." },
  { "en": "Fruktosa (Gula Buah)?", "id": "C₆H₁₂O₆." },
  { "en": "Kalsium Oksida (Kapur Tohor)?", "id": "CaO." },
  { "en": "Aluminium Oksida?", "id": "Al₂O₃." },
  { "en": "Perak Nitrat?", "id": "AgNO₃." },
  { "en": "Asam Formiat (Asam Semut)?", "id": "HCOOH." },
  { "en": "Etana?", "id": "C₂H₆." },
  { "en": "Etena (Etilena)?", "id": "C₂H₄." },
  { "en": "Isopropil Alkohol?", "id": "C₃H₇OH." },
  { "en": "Magnesium Oksida?", "id": "MgO." },
  { "en": "Natrium Sulfat?", "id": "Na₂SO₄." },
  { "en": "Kalium Klorida?", "id": "KCl." },
  { "en": "Barium Sulfat?", "id": "BaSO₄." },
  { "en": "Amonium Sulfat (Pupuk ZA)?", "id": " (NH₄)₂SO₄." },
  { "en": "Nitrogen Dioksida?", "id": "NO₂." },
  { "en": "Dinitrogen Monoksida (Gas Tawa)?", "id": "N₂O." },
  { "en": "Hidrogen Sianida?", "id": "HCN." },
  { "en": "Asam Benzoat (Pengawet)?", "id": "C₇H₆O₂." },
  { "en": "Kafein?", "id": "C₈H₁₀N₄O₂." },
  { "en": "Asam Askorbat (Vitamin C)?", "id": "C₆H₈O₆." },
  { "en": "Aspirin (Asam Asetilsalisilat)?", "id": "C₉H₈O₄." },
  { "en": "Parasetamol?", "id": "C₈H₉NO₂." },
  { "en": "Gas Helium?", "id": "He." },
  { "en": "Gas Neon?", "id": "Ne." },
  { "en": "Gas Argon?", "id": "Ar." },
  { "en": "Kalsium Sulfat (Gipsum)?", "id": "CaSO₄." },
  { "en": "Natrium Fosfat?", "id": "Na₃PO₄." },
  { "en": "Besi(II) Sulfat?", "id": "FeSO₄." },
  { "en": "Seng Oksida?", "id": "ZnO." },
  { "en": "Timbal(II) Nitrat?", "id": "Pb(NO₃)₂." },
  { "en": "Kalium Dikromat?", "id": "K₂Cr₂O₇." },
  { "en": "Asam Oksalat?", "id": "C₂H₂O₄." },
  { "en": "Glisin (Asam Amino)?", "id": "C₂H₅NO₂." },
  { "en": "Kloroform?", "id": "CHCl₃." },
  { "en": "Karbon Tetraklorida?", "id": "CCl₄." },
  { "en": "Anilina?", "id": "C₆H₅NH₂." },
  { "en": "Piridina?", "id": "C₅H₅N." },
  { "en": "Naftalena (Kamper)?", "id": "C₁₀H₈." },
  { "en": "Hidrazin?", "id": "N₂H₄." },
  { "en": "Fosfin?", "id": "PH₃." },
  { "en": "Silan?", "id": "SiH₄." },
  { "en": "Asam Borat?", "id": "H₃BO₃." },
  { "en": "Litium Hidroksida?", "id": "LiOH." },
  { "en": "Perak Klorida?", "id": "AgCl." },
  { "en": "Barium Hidroksida?", "id": "Ba(OH)₂." },
  { "en": "Kalium Iodida?", "id": "KI." },
  { "en": "Natrium Sianida?", "id": "NaCN." },
  { "en": "Hidrogen Iodida?", "id": "HI." },
  { "en": "Hidrogen Bromida?", "id": "HBr." },
  { "en": "Asam Sulfit?", "id": "H₂SO₃." },
  { "en": "Kalium Kromat?", "id": "K₂CrO₄." },
  { "en": "Asam Hipoklorit?", "id": "HClO." },
  { "en": "Etilena Glikol (Pendingin Radiator)?", "id": "C₂H₆O₂." },
  { "en": "Vanilin?", "id": "C₈H₈O₃." },
  { "en": "Natrium Hipoklorit (Bahan aktif pemutih)?", "id": "NaClO." },
  { "en": "Kalsium Hipoklorit (Kaporit)?", "id": "Ca(ClO)₂." },
  { "en": "Titanium Dioksida (Pigmen putih)?", "id": "TiO₂." },
  { "en": "Hidrogen Sianida (Asam Sianida)?", "id": "HCN." },
  { "en": "Kalium Klorat?", "id": "KClO₃." },
  { "en": "Natrium Asetat?", "id": "CH₃COONa." },
  { "en": "Ammonium Nitrat?", "id": "NH₄NO₃." },
  { "en": "Pirit (Emas Bodoh)?", "id": "FeS₂." },
  { "en": "Asam Oksalat (pada bayam)?", "id": "H₂C₂O₄." },
  { "en": "Boraks (Natrium Tetraborat)?", "id": "Na₂B₄O₇·10H₂O." },
  { "en": "Metil Salisilat (Minyak Gandapura)?", "id": "C₈H₈O₃." },
  { "en": "Ibuprofen?", "id": "C₁₃H₁₈O₂." },
  { "en": "Dietil Eter?", "id": "(C₂H₅)₂O." },
  { "en": "Asetaldehida?", "id": "CH₃CHO." },
  { "en": "Benzaldehida (Aroma almon)?", "id": "C₇H₆O." },
  { "en": "Asam Tartarat (dalam buah anggur)?", "id": "C₄H₆O₆." },
  { "en": "Magnesium Sulfat (Garam Epsom)?", "id": "MgSO₄." },
  { "en": "Natrium Sulfit?", "id": "Na₂SO₃." },
  { "en": "Kalium Iodat?", "id": "KIO₃." },
  { "en": "Seng Sulfida?", "id": "ZnS." },
  { "en": "Aluminium Sulfat?", "id": "Al₂(SO₄)₃." },
  { "en": "Besi(II) Klorida?", "id": "FeCl₂." },
  { "en": "Timah(II) Klorida?", "id": "SnCl₂." },
  { "en": "Asam Kromat?", "id": "H₂CrO₄." },
  { "en": "Litium Karbonat?", "id": "Li₂CO₃." },
  { "en": "Stronsium Klorida?", "id": "SrCl₂." },
  { "en": "Sikloheksana?", "id": "C₆H₁₂." },
  { "en": "Alanin (Asam Amino)?", "id": "C₃H₇NO₂." },
  { "en": "Vinil Klorida (Monomer PVC)?", "id": "C₂H₃Cl." },
  { "en": "Stirena (Monomer Polistirena)?", "id": "C₈H₈." },
  { "en": "Xenon Tetrafluorida?", "id": "XeF₄." },
  { "en": "Klorin Trifluorida?", "id": "ClF₃." },
  { "en": "Magnetit (Bijih besi magnetik)?", "id": "Fe₃O₄." },
  { "en": "Galena (Bijih timbal)?", "id": "PbS." },
  { "en": "Kalkopirit (Bijih tembaga)?", "id": "CuFeS₂." },
  { "en": "Tembaga(I) Oksida?", "id": "Cu₂O." },
  { "en": "Mangan Dioksida?", "id": "MnO₂." },
  { "en": "Asam Perklorat?", "id": "HClO₄." },
  { "en": "Akrilonitril?", "id": "C₃H₃N." },
  { "en": "Glutamin (Asam Amino)?", "id": "C₅H₁₀N₂O₃." },
  { "en": "Teobromin (dalam cokelat)?", "id": "C₇H₈N₄O₂." },
  { "en": "Xilena?", "id": "C₈H₁₀." },
  { "en": "Kalium Sianida?", "id": "KCN." },
  { "en": "Natrium Bromida?", "id": "NaBr." },
  { "en": "Kalsium Klorida?", "id": "CaCl₂." },
  { "en": "Ammonium Fosfat?", "id": " (NH₄)₃PO₄." },
  { "en": "Diborana?", "id": "B₂H₆." },
  { "en": "Fosfor Triklorida?", "id": "PCl₃." },
  { "en": "Fosfor Pentaklorida?", "id": "PCl₅." },
  { "en": "Belerang Heksafluorida?", "id": "SF₆." },
  { "en": "Kadmium Sulfida?", "id": "CdS." },
  { "en": "Merkuri(II) Klorida?", "id": "HgCl₂." },
  { "en": "Etilena Oksida?", "id": "C₂H₄O." },
  { "en": "Asam Adipat (Bahan Nilon)?", "id": "C₆H₁₀O₄." },
  { "en": "Kaprolaktam?", "id": "C₆H₁₁NO." },
  { "en": "Melamin?", "id": "C₃H₆N₆." },
  { "en": "Triptofan (Asam Amino)?", "id": "C₁₁H₁₂N₂O₂." },
  { "en": "Histamin?", "id": "C₅H₉N₃." },
  { "en": "Dopamin?", "id": "C₈H₁₁NO₂." },
  { "en": "Serotonin?", "id": "C₁₀H₁₂N₂O." },
  { "en": "Adrenalin (Epinefrin)?", "id": "C₉H₁₃NO₃." },
  { "en": "Indol?", "id": "C₈H₇N." },
  { "en": "Asam Oleat (dalam minyak zaitun)?", "id": "C₁₈H₃₄O₂." },
  { "en": "Asam Palmitat (dalam minyak sawit)?", "id": "C₁₆H₃₂O₂." },
  { "en": "Asam Stearat?", "id": "C₁₈H₃₆O₂." },
  { "en": "Amonium Perklorat (Bahan bakar padat roket)?", "id": "NH₄ClO₄." },
  { "en": "Trinitrotoluena (TNT)?", "id": "C₇H₅N₃O₅." },
  { "en": "Nitrogliserin?", "id": "C₃H₅N₃O₉." },
  { "en": "Kalsium Karbida (Karbit)?", "id": "CaC₂." },
  { "en": "Natrium Azida (Untuk kantung udara mobil)?", "id": "NaN₃." },
  { "en": "Monosodium Glutamat (MSG)?", "id": "C₅H₈NNaO₄." },
  { "en": "Aspartam (Pemanis buatan)?", "id": "C₁₄H₁₈N₂O₅." },
  { "en": "Sakarin?", "id": "C₇H₅NO₃S." },
  { "en": "Retinol (Vitamin A)?", "id": "C₂₀H₃₀O." },
  { "en": "Tiamin (Vitamin B1)?", "id": "C₁₂H₁₇N₄OS⁺." },
  { "en": "Riboflavin (Vitamin B2)?", "id": "C₁₇H₂₀N₄O₆." },
  { "en": "Niasin (Vitamin B3)?", "id": "C₆H₅NO₂." },
  { "en": "Asam Folat (Vitamin B9)?", "id": "C₁₉H₁₉N₇O₆." },
  { "en": "Sianokobalamin (Vitamin B12)?", "id": "C₆₃H₈₈CoN₁₄O₁₄P." },
  { "en": "Kalsiferol (Vitamin D)?", "id": "C₂₈H₄₄O." },
  { "en": "Tokoferol (Vitamin E)?", "id": "C₂₉H₅₀O₂." },
  { "en": "Filokuinon (Vitamin K)?", "id": "C₃₁H₄₆O₂." },
  { "en": "Kolesterol?", "id": "C₂₇H₄₆O." },
  { "en": "Testosteron?", "id": "C₁₉H₂₈O₂." },
  { "en": "Estradiol (Hormon Estrogen)?", "id": "C₁₈H₂₄O₂." },
  { "en": "Adenosin Trifosfat (ATP)?", "id": "C₁₀H₁₆N₅O₁₃P₃." },
  { "en": "Kalium Heksasianoferat(II) (Prusi Kuning)?", "id": "K₄[Fe(CN)₆]." },
  { "en": "Kalium Heksasianoferat(III) (Prusi Merah)?", "id": "K₃[Fe(CN)₆]." },
  { "en": "Tetraamin Tembaga(II) Sulfat (Warna biru pekat)?", "id": "[Cu(NH₃)₄]SO₄." },
  { "en": "Biru Prusia (Pigmen)?", "id": "Fe₄[Fe(CN)₆]₃." },
  { "en": "Fenolftalein (Indikator pH)?", "id": "C₂₀H₁₄O₄." },
  { "en": "Metil Oranye (Indikator pH)?", "id": "C₁₄H₁₄N₃NaO₃S." },
  { "en": "Indigo (Pewarna biru jeans)?", "id": "C₁₆H₁₀N₂O₂." },
  { "en": "Dimetil Sulfoksida (DMSO)?", "id": "(CH₃)₂SO." },
  { "en": "Asetonitril?", "id": "CH₃CN." },
  { "en": "Etilendiamin?", "id": "C₂H₈N₂." },
  { "en": "Boron Trifluorida?", "id": "BF₃." },
  { "en": "Barium Klorida?", "id": "BaCl₂." },
  { "en": "Stronsium Nitrat (Pewarna merah kembang api)?", "id": "Sr(NO₃)₂." },
  { "en": "Natrium Silikat (Waterglass)?", "id": "Na₂SiO₃." },
  { "en": "Asam Borat (Antiseptik)?", "id": "H₃BO₃." },
  { "en": "Litium Aluminium Hidrida (Pereduksi kuat)?", "id": "LiAlH₄." },
  { "en": "Natrium Borohidrida?", "id": "NaBH₄." },
  { "en": "Asetil Klorida?", "id": "CH₃COCl." },
  { "en": "Anhidrida Asetat?", "id": "(CH₃CO)₂O." },
  { "en": "Piridin?", "id": "C₅H₅N." },
  { "en": "Pirol?", "id": "C₄H₅N." },
  { "en": "Furan?", "id": "C₄H₄O." },
  { "en": "Tiofena?", "id": "C₄H₄S." },
  { "en": "Purin?", "id": "C₅H₄N₄." },
  { "en": "Adenin (Basa Nitrogen)?", "id": "C₅H₅N₅." },
  { "en": "Guanin (Basa Nitrogen)?", "id": "C₅H₅N₅O." },
  { "en": "Sitosin (Basa Nitrogen)?", "id": "C₄H₅N₃O." },
  { "en": "Timin (Basa Nitrogen)?", "id": "C₅H₆N₂O₂." },
  { "en": "Urasil (Basa Nitrogen pada RNA)?", "id": "C₄H₄N₂O₂." },
  { "en": "Kaprilil Glikol?", "id": "C₈H₁₈O₂." },
  { "en": "Asam Undesilenat?", "id": "C₁₁H₂₀O₂." },
  { "en": "Asam Miristat?", "id": "C₁₄H₂₈O₂." },
  { "en": "Tereftalat (Monomer PET)?", "id": "C₈H₆O₄." },
  { "en": "Bisphenol A (BPA)?", "id": "C₁₅H₁₆O₂." },
  { "en": "Butadiena?", "id": "C₄H₆." },
  { "en": "Isoprena (Monomer Karet Alam)?", "id": "C₅H₈." },
  { "en": "Limonena (Aroma Jeruk)?", "id": "C₁₀H₁₆." },
  { "en": "Mentol (Aroma Mint)?", "id": "C₁₀H₂₀O." },
  { "en": "Kamper (Kapuk Barus)?", "id": "C₁₀H₁₆O." },
  { "en": "Geraniol (Aroma Mawar)?", "id": "C₁₀H₁₈O." },
  { "en": "Etilamina?", "id": "C₂H₅NH₂." },
  { "en": "Trimetilamina (Bau Amis Ikan)?", "id": "N(CH₃)₃." },
  { "en": "Kuinina (Obat Malaria)?", "id": "C₂₀H₂₄N₂O₂." },
  { "en": "Morfin?", "id": "C₁₇H₁₉NO₃." },
  { "en": "Nikotin?", "id": "C₁₀H₁₄N₂." },
  { "en": "Putresin (Bau Busuk)?", "id": "C₄H₁₂N₂." },
  { "en": "Kadaverin (Bau Busuk)?", "id": "C₅H₁₄N₂." },
  { "en": "Asetilkolin (Neurotransmiter)?", "id": "C₇H₁₆NO₂⁺." },
  { "en": "Asam Gamma-Aminobutirat (GABA)?", "id": "C₄H₉NO₂." },
  { "en": "Melatonin (Hormon tidur)?", "id": "C₁₃H₁₆N₂O₂." },
  { "en": "Kortisol (Hormon stres)?", "id": "C₂₁H₃₀O₅." },
  { "en": "Progesteron?", "id": "C₂₁H₃₀O₂." },
  { "en": "Laktosa (Gula susu)?", "id": "C₁₂H₂₂O₁₁." },
  { "en": "Maltosa (Gula gandum)?", "id": "C₁₂H₂₂O₁₁." },
  { "en": "Galaktosa?", "id": "C₆H₁₂O₆." },
  { "en": "Ribosa (Gula pada RNA)?", "id": "C₅H₁₀O₅." },
  { "en": "Deoksiribosa (Gula pada DNA)?", "id": "C₅H₁₀O₄." },
  { "en": "Asam Linoleat (Asam lemak Omega-6)?", "id": "C₁₈H₃₂O₂." },
  { "en": "EDTA (Asam Etilendiamintetraasetat)?", "id": "C₁₀H₁₆N₂O₈." },
  { "en": "Heksana (Pelarut)?", "id": "C₆H₁₄." },
  { "en": "Tetrahidrofuran (THF)?", "id": "C₄H₈O." },
  { "en": "Ferosen?", "id": "Fe(C₅H₅)₂." },
  { "en": "Tetraetil Timbal (Zat aditif bensin lama)?", "id": "Pb(C₂H₅)₄." },
  { "en": "Glifosat (Bahan aktif herbisida)?", "id": "C₃H₈NO₅P." },
  { "en": "DDT (Insektisida lama)?", "id": "C₁₄H₉Cl₅." },
  { "en": "Kapsaisin (Pemberi rasa pedas cabai)?", "id": "C₁₈H₂₇NO₃." },
  { "en": "Piperin (Pemberi rasa pedas lada)?", "id": "C₁₇H₁₉NO₃." },
  { "en": "Strychnine (Racun)?", "id": "C₂₁H₂₂N₂O₂." },
  { "en": "Atropin?", "id": "C₁₇H₂₃NO₃." },
  { "en": "Kokain?", "id": "C₁₇H₂₁NO₄." },
  { "en": "Asam Pikrat (Bahan Peledak)?", "id": "C₆H₃N₃O₇." },
  { "en": "Tetrafluoroetilena (Monomer Teflon)?", "id": "C₂F₄." },
  { "en": "Metil Metakrilat (Monomer kaca akrilik)?", "id": "C₅H₈O₂." },
  { "en": "Akrilamida?", "id": "C₃H₅NO." },
  { "en": "Kalium Tiosianat?", "id": "KSCN." },
  { "en": "Natrium Peroksida?", "id": "Na₂O₂." },
  { "en": "Barium Peroksida?", "id": "BaO₂." },
  { "en": "Kalsium Fosfida?", "id": "Ca₃P₂." },
  { "en": "Aluminium Nitrida?", "id": "AlN." },
  { "en": "Silikon Karbida (Batu gerinda)?", "id": "SiC." },
  { "en": "Boron Nitrida?", "id": "BN." },
  { "en": "Asam Sianurat?", "id": "C₃H₃N₃O₃." },
  { "en": "Serin (Asam Amino)?", "id": "C₃H₇NO₃." },
  { "en": "Valin (Asam Amino)?", "id": "C₅H₁₁NO₂." },
  { "en": "Prostaglandin E2?", "id": "C₂₀H₃₂O₅." },
  { "en": "Leukotriena B4?", "id": "C₂₀H₃₂O₄." },
  { "en": "Sildenafil (Viagra)?", "id": "C₂₂H₃₀N₆O₄S." },
  { "en": "Metformin (Obat Diabetes)?", "id": "C₄H₁₁N₅." },
  { "en": "Kuinon?", "id": "C₆H₄O₂." },
  { "en": "Hidrokuinon?", "id": "C₆H₆O₂." },
  { "en": "Resorsinol?", "id": "C₆H₆O₂." },
  { "en": "Katekol?", "id": "C₆H₆O₂." },
  { "en": "Piperidin?", "id": "C₅H₁₁N." },
  { "en": "Kuinolin?", "id": "C₉H₇N." },
  { "en": "Karbazol?", "id": "C₁₂H₉N." },
  { "en": "Adenosin?", "id": "C₁₀H₁₃N₅O₄." },
  { "en": "Guanosin?", "id": "C₁₀H₁₃N₅O₅." },
  { "en": "Asam Hialuronat?", "id": "(C₁₄H₂₁NO₁₁)n." },
  { "en": "Asam Salisilat (Bahan obat jerawat)?", "id": "C₇H₆O₃." },
  { "en": "Beta-Karoten (Pigmen oranye pada wortel)?", "id": "C₄₀H₅₆." },
  { "en": "Likopen (Pigmen merah pada tomat)?", "id": "C₄₀H₅₆." },
  { "en": "Quercetin (Flavonoid)?", "id": "C₁₅H₁₀O₇." },
  { "en": "Katekin (Antioksidan dalam teh)?", "id": "C₁₅H₁₄O₆." },
  { "en": "Sarin (Gas saraf)?", "id": "C₄H₁₀FO₂P." },
  { "en": "Gas Mustard?", "id": "C₄H₈Cl₂S." },
  { "en": "Zirkon (Batu mineral)?", "id": "ZrSiO₄." },
  { "en": "Fluorit?", "id": "CaF₂." },
  { "en": "Spinel?", "id": "MgAl₂O₄." },
  { "en": "Neodymium Oksida (Untuk magnet super kuat)?", "id": "Nd₂O₃." },
  { "en": "Serium(IV) Oksida (Untuk poles kaca)?", "id": "CeO₂." },
  { "en": "Yttrium Aluminium Garnet (YAG, untuk laser)?", "id": "Y₃Al₅O₁₂." },
  { "en": "Uranium Heksafluorida?", "id": "UF₆." },
  { "en": "Plutonium Dioksida?", "id": "PuO₂." },
  { "en": "Alfa-Pinena (Aroma pohon pinus)?", "id": "C₁₀H₁₆." },
  { "en": "Lutein (Pigmen kuning pada mata)?", "id": "C₄₀H₅₆O₂." },
  { "en": "Asam Piruvat (Hasil glikolisis)?", "id": "C₃H₄O₃." },
  { "en": "Trietilamina (Basa organik)?", "id": "N(C₂H₅)₃." },
  { "en": "N-Bromosuccinimide (NBS)?", "id": "C₄H₄BrNO₂." },
  { "en": "Allantoin (Bahan pelembap kulit)?", "id": "C₄H₆N₄O₃." },
  { "en": "Squalane?", "id": "C₃₀H₆₂." },
  { "en": "Rodamin B (Pewarna tekstil)?", "id": "C₂₈H₃₁ClN₂O₃." },
  { "en": "Eosin?", "id": "C₂₀H₆Br₄Na₂O₅." },
  { "en": "Perak Bromida (Fotografi film)?", "id": "AgBr." },
  { "en": "Natrium Dodesil Sulfat (SDS, deterjen)?", "id": "C₁₂H₂₅NaO₄S." },
  { "en": "Taurin?", "id": "C₂H₇NO₃S." },
  { "en": "Kreatin?", "id": "C₄H₉N₃O₂." },
  { "en": "Spermidin?", "id": "C₇H₁₉N₃." },
  { "en": "Karnitin?", "id": "C₇H₁₅NO₃." },
  { "en": "Vanadium Pentoksida (Katalis)?", "id": "V₂O₅." },
  { "en": "Molibdenum Disulfida (Pelumas padat)?", "id": "MoS₂." },
  { "en": "Tungsten Karbida (Mata bor)?", "id": "WC." },
  { "en": "Indium Timah Oksida (ITO, layar sentuh)?", "id": "In₂O₃·(SnO₂)ₓ." },
  { "en": "Barium Titanat (Kapasitor)?", "id": "BaTiO₃." },
  { "en": "Bismut Telurida (Termoelektrik)?", "id": "Bi₂Te₃." },
  { "en": "Gallium Arsenida (Semikonduktor)?", "id": "GaAs." },
  { "en": "Gallium Nitrida (LED biru)?", "id": "GaN." },
  { "en": "Asam Klorida Dekahidrat (Clathrate hydrate)?", "id": "Cl₂(H₂O)₁₀." },
  { "en": "Asam Lisergat Dietilamida (LSD)?", "id": "C₂₀H₂₅N₃O." },
  { "en": "Psilocybin (dari jamur ajaib)?", "id": "C₁₂H₁₇N₂O₄P." },
  { "en": "Dimetiltriptamin (DMT)?", "id": "C₁₂H₁₆N₂." },
  { "en": "Fentanil?", "id": "C₂₂H₂₈N₂O." },
  { "en": "Capsaicinoid?", "id": "C₁₈H₂₇NO₃." },
  { "en": "Astaxanthin (Pigmen merah muda pada salmon)?", "id": "C₄₀H₅₂O₄." },
  { "en": "Zeaxanthin?", "id": "C₄₀H₅₆O₂." },
  { "en": "Giberelin (Hormon tumbuhan)?", "id": "C₁₉H₂₂O₆." },
  { "en": "Asam Absisat (Hormon tumbuhan)?", "id": "C₁₅H₂₀O₄." },
  { "en": "Sitokinin (Hormon tumbuhan)?", "id": "C₁₀H₉N₅O." },
  { "en": "Skualena?", "id": "C₃₀H₅₀." },
  { "en": "Lanosterol?", "id": "C₃₀H₅₀O." },
  { "en": "Heme (Bagian dari hemoglobin)?", "id": "C₃₄H₃₂FeN₄O₄." },
  { "en": "Klorofil a?", "id": "C₅₅H₇₂MgN₄O₅." },
  { "en": "Buckminsterfullerene (Buckyball)?", "id": "C₆₀." },
  { "en": "Propilena Glikol (Bahan dasar liquid vape)?", "id": "C₃H₈O₂." },
  { "en": "Asam Lipoat?", "id": "C₈H₁₄O₂S₂." },
  { "en": "Asam Hialuronat (unit berulang)?", "id": "(C₁₄H₂₁NO₁₁)n." },
  { "en": "Amoksisilin (Antibiotik)?", "id": "C₁₆H₁₉N₃O₅S." },
  { "en": "Tetrasiklin (Antibiotik)?", "id": "C₂₂H₂₄N₂O₈." },
  { "en": "Atorvastatin (Obat kolesterol Lipitor)?", "id": "C₃₃H₃₅FN₂O₅." },
  { "en": "Warfarin (Pengencer darah)?", "id": "C₁₉H₁₆O₄." },
  { "en": "Digoksin (Obat jantung)?", "id": "C₄₁H₆₄O₁₄." },
  { "en": "Diazepam (Valium)?", "id": "C₁₆H₁₃ClN₂O." },
  { "en": "Imidazol (Cincin heterosiklik)?", "id": "C₃H₄N₂." },
  { "en": "Okzasol?", "id": "C₃H₃NO." },
  { "en": "Tiazol?", "id": "C₃H₃NS." },
  { "en": "Pirimidin (Basa Nitrogen Induk)?", "id": "C₄H₄N₂." },
  { "en": "Siklopentadiena?", "id": "C₅H₆." },
  { "en": "o-Xilena (Orto-Xilena)?", "id": "C₈H₁₀." },
  { "en": "m-Xilena (Meta-Xilena)?", "id": "C₈H₁₀." },
  { "en": "p-Xilena (Para-Xilena)?", "id": "C₈H₁₀." },
  { "en": "Asam Maleat?", "id": "C₄H₄O₄." },
  { "en": "Asam Fumarat?", "id": "C₄H₄O₄." },
  { "en": "Asam Tereftalat (Monomer PET)?", "id": "C₈H₆O₄." },
  { "en": "Dimercaprol (Agen kelasi logam berat)?", "id": "C₃H₈OS₂." },
  { "en": "Glutation?", "id": "C₁₀H₁₇N₃O₆S." },
  { "en": "Spermin?", "id": "C₁₀H₂₆N₄." },
  { "en": "α-D-Glukosa?", "id": "C₆H₁₂O₆." },
  { "en": "β-D-Glukosa?", "id": "C₆H₁₂O₆." },
  { "en": "Selobiosa?", "id": "C₁₂H₂₂O₁₁." },
  { "en": "Manosa?", "id": "C₆H₁₂O₆." },
  { "en": "Kitin (unit berulang)?", "id": "(C₈H₁₃O₅N)n." },
  { "en": "Asam Alginat?", "id": "(C₆H₈O₆)n." },
  { "en": "Cesium Klorida?", "id": "CsCl." },
  { "en": "Rubidium Oksida?", "id": "Rb₂O." },
  { "en": "Stronsium Oksida?", "id": "SrO." },
  { "en": "Asam Selenat?", "id": "H₂SeO₄." },
  { "en": "Asam Telurat?", "id": "H₆TeO₆." },
  { "en": "Natrium Tungstat?", "id": "Na₂WO₄." },
  { "en": "Kalium Superoksida?", "id": "KO₂." },
  { "en": "Natrium Peroksodisulfat?", "id": "Na₂S₂O₈." },
  { "en": "Asam Tiosulfat?", "id": "H₂S₂O₃." },
  { "en": "Hidroksilamina?", "id": "NH₂OH." },
  { "en": "Tetrametilsilan (TMS, standar NMR)?", "id": "Si(CH₃)₄." },
  { "en": "Asetamida?", "id": "CH₃CONH₂." },
  { "en": "Benzamida?", "id": "C₇H₇NO." },
  { "en": "Akrilonitril Butadiena Stirena (ABS)?", "id": "(C₈H₈·C₄H₆·C₃H₃N)n." },
  { "en": "Polikarbonat (unit berulang)?", "id": "(C₁₅H₁₆O₂)n." },
  { "en": "Kevlar (unit berulang)?", "id": "(-CO-C₆H₄-CO-NH-C₆H₄-NH-)n." },
  { "en": "Nomex (unit berulang)?", "id": "(-CO-C₆H₄-NH-)n." },
  { "en": "Ftalosianin?", "id": "C₃₂H₁₈N₈." },
  { "en": "Porfirin?", "id": "C₂₀H₁₄N₄." },
  { "en": "Siklamat (Pemanis buatan)?", "id": "C₆H₁₂NNaO₃S." },


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
