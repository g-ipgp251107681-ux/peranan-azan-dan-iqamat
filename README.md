<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Kembara Masjid: Jalan Menuju Solat - Tahun 4</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;500;600;700&family=Nunito:wght@600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Nunito', sans-serif;
            background: linear-gradient(180deg, #bae6fd 0%, #e0f2fe 52%, #dcfce7 100%);
            min-height: 100vh;
            overflow-x: hidden;
            user-select: none;
            -webkit-user-select: none;
        }
        h1, h2, h3, h4, .font-display {
            font-family: 'Fredoka', sans-serif;
        }

        /* Kesan Butang 3D Mesra Kanak-kanak */
        .btn-game {
            transition: all 0.15s cubic-bezier(0.4, 0, 0.2, 1);
            box-shadow: 0 5px 0 rgba(0, 0, 0, 0.22);
        }
        .btn-game:active:not(:disabled) {
            transform: translateY(3px);
            box-shadow: 0 2px 0 rgba(0, 0, 0, 0.22);
        }

        @keyframes floatSlow {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-6px); }
        }
        .animate-float {
            animation: floatSlow 3.2s ease-in-out infinite;
        }

        @keyframes hopToken {
            0%, 100% { transform: translateY(0) scale(1); }
            50% { transform: translateY(-9px) scale(1.1); }
        }
        .animate-hop {
            animation: hopToken 0.65s ease-in-out infinite;
        }

        @keyframes diceSpin {
            0% { transform: rotate(0deg) scale(1); }
            25% { transform: rotate(-16deg) scale(1.14); }
            50% { transform: rotate(16deg) scale(1.16); }
            75% { transform: rotate(-10deg) scale(1.1); }
            100% { transform: rotate(0deg) scale(1); }
        }
        .animate-dice {
            animation: diceSpin 0.28s ease-in-out infinite;
        }

        @keyframes popIn {
            0% { transform: scale(0.75); opacity: 0; }
            80% { transform: scale(1.03); opacity: 1; }
            100% { transform: scale(1); opacity: 1; }
        }
        .animate-pop {
            animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-7px); }
            40%, 80% { transform: translateX(7px); }
        }
        .animate-shake {
            animation: shake 0.36s ease-in-out;
        }

        /* Gaya Petak Papan Permainan */
        .board-tile {
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }
        .board-tile-active {
            transform: scale(1.06);
            box-shadow: 0 0 0 4px #facc15, 0 8px 16px rgba(0, 0, 0, 0.2);
            z-index: 10;
        }
        .masjid-center-active {
            transform: scale(1.04);
            box-shadow: 0 0 0 6px #fde047, 0 14px 28px rgba(16, 185, 129, 0.45);
        }

        /* Bar Skrol Kemas */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #94a3b8;
            border-radius: 10px;
        }
    </style>
</head>
<body class="text-slate-800 flex flex-col items-center justify-between p-2 sm:p-4">

    <!-- Latar Belakang Siluet Masjid & Taman Hijau -->
    <div class="fixed inset-0 pointer-events-none z-0 overflow-hidden opacity-40">
        <svg viewBox="0 0 1440 320" class="absolute bottom-0 w-full h-auto" preserveAspectRatio="none">
            <path fill="#4ade80" fill-opacity="0.65" d="M0,224L60,213.3C120,203,240,181,360,186.7C480,192,600,224,720,218.7C840,213,960,171,1080,165.3C1200,160,1320,192,1380,208L1440,224L1440,320L1380,320C1320,320,1200,320,1080,320C960,320,840,320,720,320C600,320,480,320,360,320C240,320,120,320,60,320L0,320Z"></path>
            <path fill="#16a34a" fill-opacity="0.45" d="M100,240 L100,135 L112,102 L124,135 L124,240 Z M135,240 Q180,145 225,240 Z"></path>
            <path fill="#15803d" fill-opacity="0.45" d="M1270,240 L1270,118 L1285,85 L1300,118 L1300,240 Z M1175,240 Q1222,138 1270,240 Z"></path>
        </svg>
    </div>

    <!-- BAR STATUS ATAS (HUD) -->
    <header id="game-hud" class="hidden relative z-20 w-full max-w-5xl bg-white/95 backdrop-blur-md rounded-2xl shadow-lg border-4 border-sky-300 px-3 py-2 sm:px-5 sm:py-2.5 mb-2 flex flex-wrap items-center justify-between gap-2">
        <div class="flex items-center gap-2">
            <button onclick="returnToMenu()" class="bg-amber-100 hover:bg-amber-200 text-amber-900 font-bold px-3 py-1.5 rounded-xl border-2 border-amber-300 text-xs sm:text-sm flex items-center gap-1 transition">
                <span>🏠</span> <span class="hidden sm:inline">Menu</span>
            </button>
            <div class="bg-indigo-100 border-2 border-indigo-300 px-3 py-1 rounded-xl">
                <span class="text-[10px] sm:text-xs font-bold text-indigo-600 uppercase block leading-none">Soalan Selesai</span>
                <span id="hud-progress" class="font-display font-bold text-sm sm:text-base text-indigo-950">0 / 12</span>
            </div>
            <div class="hidden md:block bg-emerald-50 border-2 border-emerald-300 px-3 py-1 rounded-xl">
                <span class="text-[10px] font-bold text-emerald-700 uppercase block leading-none">Kedudukan Laluan</span>
                <span id="hud-position" class="font-display font-bold text-sm text-emerald-950">Pusingan 1 • Petak 1</span>
            </div>
            <button onclick="openNotes()" class="bg-sky-100 hover:bg-sky-200 text-sky-900 font-bold px-2.5 py-1.5 rounded-xl border-2 border-sky-300 text-xs sm:text-sm flex items-center gap-1 transition" title="Buka Nota Ringkas">
                <span>📖</span> <span class="hidden sm:inline">Nota Ringkas</span>
            </button>
        </div>

        <!-- Baki Soalan, Markah, Pause & Bunyi -->
        <div class="flex items-center gap-2 sm:gap-3">
            <div class="bg-rose-50 px-2.5 py-1 rounded-xl border-2 border-rose-200 text-right">
                <span class="text-[10px] sm:text-xs font-bold text-rose-700 uppercase block leading-none">Baki Soalan</span>
                <span id="hud-remaining" class="font-display font-bold text-xs sm:text-sm text-rose-950">12 Soalan</span>
            </div>
            <div class="bg-amber-100 border-2 border-amber-400 px-3 py-1 rounded-xl text-right">
                <span class="text-[10px] sm:text-xs font-bold text-amber-800 uppercase block leading-none">Markah</span>
                <span id="hud-score" class="font-display font-bold text-sm sm:text-lg text-amber-950">⭐ 0</span>
            </div>
            <button onclick="togglePause()" id="pause-btn-hud" class="bg-amber-300 hover:bg-amber-400 text-amber-950 font-bold px-3 py-1.5 rounded-xl border-2 border-amber-500 text-xs sm:text-sm flex items-center gap-1 transition" title="Jeda / Rehat (Esc atau P)">
                <span>⏸️</span> <span class="hidden sm:inline">Pause</span>
            </button>
            <button onclick="toggleSound()" id="sound-btn-hud" class="bg-sky-100 hover:bg-sky-200 p-1.5 sm:p-2 rounded-xl border-2 border-sky-300 text-base sm:text-lg" title="Tukar Bunyi">
                🔊
            </button>
        </div>
    </header>

    <!-- ==================== MODAL: PAUSE / JEDA PERMAINAN ==================== -->
    <section id="modal-pause" class="hidden fixed inset-0 z-50 bg-slate-900/75 backdrop-blur-sm flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl border-4 border-amber-400 max-w-md w-full p-6 sm:p-8 text-center shadow-2xl animate-pop">
            <div class="text-6xl mb-3 animate-bounce">⏸️</div>
            <h2 class="font-display font-extrabold text-2xl sm:text-3xl text-slate-800 mb-2">
                Permainan Dijeda
            </h2>
            <p class="text-slate-600 font-semibold text-sm sm:text-base mb-6">
                Ambil masa untuk berehat atau rujuk Nota Ringkas sebelum meneruskan kembara ke masjid!
            </p>
            <div class="flex flex-col gap-3">
                <button onclick="togglePause()" class="btn-game w-full bg-emerald-500 hover:bg-emerald-400 text-white font-display font-bold text-lg py-3.5 rounded-2xl border-2 border-emerald-700 flex items-center justify-center gap-2">
                    <span>▶️</span> Sambung Main
                </button>
                <button onclick="openNotes()" class="btn-game w-full bg-sky-100 hover:bg-sky-200 text-sky-900 font-display font-bold text-base py-3 rounded-2xl border-2 border-sky-400 flex items-center justify-center gap-2">
                    <span>📖</span> Nota Ringkas
                </button>
                <button onclick="restartFromPause()" class="btn-game w-full bg-amber-400 hover:bg-amber-300 text-amber-950 font-display font-bold text-base py-3 rounded-2xl border-2 border-amber-600 flex items-center justify-center gap-2">
                    <span>🔄</span> Mula Semula
                </button>
                <button onclick="menuFromPause()" class="btn-game w-full bg-rose-100 hover:bg-rose-200 text-rose-800 font-display font-bold text-base py-3 rounded-2xl border-2 border-rose-300 flex items-center justify-center gap-2">
                    <span>🏠</span> Menu Utama
                </button>
            </div>
        </div>
    </section>

    <!-- KANDUNGAN UTAMA -->
    <main class="relative z-10 w-full max-w-5xl flex-1 flex flex-col items-center justify-center my-auto">

        <!-- ==================== SKRIN 1: MENU UTAMA & PILIHAN WATAK ==================== -->
        <section id="screen-menu" class="w-full max-w-2xl bg-white/95 backdrop-blur-md rounded-3xl shadow-2xl border-4 border-sky-400 p-5 sm:p-8 text-center animate-pop">
            <div class="flex justify-between items-center mb-3 gap-2">
                <span class="bg-amber-100 text-amber-900 border-2 border-amber-300 font-bold text-xs sm:text-sm px-3 py-1 rounded-full">
                    🎲 Permainan Papan • Pendidikan Islam Tahun 4
                </span>
                <button onclick="toggleSound()" id="sound-btn-menu" class="bg-sky-100 hover:bg-sky-200 text-sky-900 font-bold px-3 py-1 rounded-full border-2 border-sky-300 text-xs sm:text-sm flex items-center gap-1">
                    <span id="sound-icon-menu">🔊</span> <span id="sound-text-menu">Bunyi: Hidup</span>
                </button>
            </div>

            <!-- Tajuk Utama -->
            <div class="bg-gradient-to-b from-sky-50 to-emerald-50 border-4 border-sky-200 rounded-3xl py-4 px-4 mb-4 shadow-inner">
                <span class="inline-block bg-indigo-800 text-white font-display font-bold text-xs px-3 py-0.5 rounded-full mb-1">
                    ✨ Tertib Itu Indah Dalam Islam ✨
                </span>
                <h1 class="text-2xl sm:text-4xl font-extrabold tracking-tight leading-tight">
                    <span class="text-slate-800">Kembara Masjid:</span><br>
                    <span class="text-emerald-600">Jalan Menuju</span>
                    <span class="text-pink-600">Solat</span>
                </h1>
                <p class="text-xs sm:text-sm text-amber-800 font-bold mt-1.5">
                    💖 "Ikuti Tertibnya: Azan ➔ Pergi ke Masjid ➔ Iqamat ➔ Masuk Saf untuk Solat"
                </p>
            </div>

            <!-- Pilihan Watak Pengembara -->
            <div class="mb-5">
                <p class="text-xs sm:text-sm font-extrabold text-slate-700 uppercase tracking-wide mb-2">
                    1. Pilih Watak Pengembara Kamu:
                </p>
                <div class="grid grid-cols-2 gap-3 max-w-md mx-auto">
                    <button id="char-boy" onclick="selectAvatar('boy')" class="btn-game bg-emerald-100 border-3 border-emerald-600 rounded-2xl p-3 flex flex-col items-center ring-4 ring-emerald-300 transition">
                        <span class="text-4xl sm:text-5xl mb-1">👦🏻</span>
                        <span class="font-display font-bold text-base text-emerald-950">Ahmad</span>
                        <span class="text-[11px] font-bold text-emerald-700">Murid Berkopiah</span>
                    </button>
                    <button id="char-girl" onclick="selectAvatar('girl')" class="btn-game bg-white border-3 border-slate-300 rounded-2xl p-3 flex flex-col items-center opacity-80 hover:opacity-100 transition">
                        <span class="text-4xl sm:text-5xl mb-1">🧕🏻</span>
                        <span class="font-display font-bold text-base text-pink-950">Aisyah</span>
                        <span class="text-[11px] font-bold text-pink-700">Murid Bertudung</span>
                    </button>
                </div>
            </div>

            <!-- Cara Bermain Ringkas -->
            <div class="bg-amber-50 border-2 border-amber-300 rounded-2xl p-3 mb-5 text-xs sm:text-sm text-amber-950 font-semibold">
                🎲 <strong>Cara Bermain:</strong> Baling dadu rawak (<strong>1–6</strong>) mengelilingi 20 petak laluan. Selesaikan kesemua <strong>12 soalan tanpa ulangan</strong> untuk masuk ke <strong>Masjid Al-Falah</strong>!
            </div>

            <!-- Butang Mula & Nota -->
            <div class="flex flex-col sm:flex-row gap-3 justify-center">
                <button onclick="openNotes()" class="btn-game bg-amber-400 hover:bg-amber-300 text-amber-950 font-display font-bold text-lg sm:text-xl px-6 py-4 rounded-2xl border-2 border-amber-600 flex items-center justify-center gap-2">
                    <span>📖</span> Nota Ringkas
                </button>
                <button onclick="startGame()" class="btn-game bg-emerald-500 hover:bg-emerald-400 text-white font-display font-bold text-lg sm:text-xl px-8 py-4 rounded-2xl border-2 border-emerald-700 flex items-center justify-center gap-2">
                    <span>🎲</span> Mula Kembara!
                </button>
            </div>
        </section>

        <!-- ==================== SKRIN 2: PAPAN PERMAINAN PENGEMBARAAN (20 PETAK PUSINGAN + MASJID TENGAH) ==================== -->
        <section id="screen-board" class="hidden w-full bg-white/95 backdrop-blur-md rounded-3xl shadow-2xl border-4 border-emerald-400 p-3 sm:p-5 animate-pop">
            
            <!-- Petunjuk Jenis Petak & Status Giliran -->
            <div class="flex flex-wrap items-center justify-between gap-2 mb-3 pb-2 border-b-2 border-slate-100">
                <div class="flex flex-wrap items-center gap-1.5 sm:gap-2 text-[11px] sm:text-xs font-bold">
                    <span class="bg-sky-100 text-sky-900 border border-sky-300 px-2.5 py-1 rounded-full">📘 Petak Ilmu (Soalan)</span>
                    <span class="bg-amber-100 text-amber-900 border border-amber-400 px-2.5 py-1 rounded-full">⭐ Bonus (+20 Markah & Soalan)</span>
                    <span class="bg-pink-100 text-pink-900 border border-pink-300 px-2.5 py-1 rounded-full">🌸 Peringatan Tertib</span>
                </div>
                <span id="board-status-banner" class="text-xs sm:text-sm font-display font-bold text-indigo-800 bg-indigo-50 px-3 py-1 rounded-xl border border-indigo-200">
                    Baling dadu (1–6) untuk mengelilingi laluan & jawab semua 12 soalan!
                </span>
            </div>

            <!-- Grid Papan Laluan Pusingan Tertutup (6 Kolom x 6 Baris = 20 Petak Luar + Masjid Al-Falah di Tengah) -->
            <div id="board-grid" class="grid grid-cols-6 gap-1 sm:gap-2 mb-3">
                <!-- Dijana secara dinamik oleh JavaScript -->
            </div>

            <!-- Penjejak Visual 12 Soalan Tanpa Ulangan -->
            <div class="bg-slate-50 border-2 border-slate-200 rounded-2xl px-3 py-2 mb-3 flex flex-wrap items-center justify-between gap-2">
                <span class="text-xs font-extrabold text-slate-700">📋 Jejak 12 Soalan Pengukuhan (Tanpa Ulangan):</span>
                <div id="question-tracker-dots" class="flex flex-wrap items-center gap-1.5">
                    <!-- 12 lencana nombor soalan dijana oleh JS -->
                </div>
            </div>

            <!-- Panel Kawalan Bawah: Dadu Rawak 1-6 & Info Pengembara -->
            <div class="bg-gradient-to-r from-sky-50 via-amber-50 to-emerald-50 border-3 border-sky-200 rounded-2xl p-3 sm:p-4 flex flex-col sm:flex-row items-center justify-between gap-3">
                <div class="flex items-center gap-3 text-center sm:text-left">
                    <div id="player-badge-icon" class="text-4xl sm:text-5xl bg-white w-14 h-14 sm:w-16 sm:h-16 rounded-2xl border-3 border-emerald-400 flex items-center justify-center shadow-sm shrink-0">
                        👦🏻
                    </div>
                    <div>
                        <h3 id="player-badge-name" class="font-display font-bold text-base sm:text-lg text-slate-800">
                            Pengembara: Ahmad
                        </h3>
                        <p id="turn-instruction" class="text-xs sm:text-sm text-slate-600 font-semibold">
                            Tekan "Baling Dadu!" untuk mendapat mata dadu rawak (1–6) dan jawab soalan pengukuhan.
                        </p>
                    </div>
                </div>

                <!-- Kotak Dadu & Butang Baling -->
                <div class="flex items-center gap-3 w-full sm:w-auto justify-center">
                    <div id="dice-box" class="w-14 h-14 sm:w-16 sm:h-16 bg-white rounded-2xl border-4 border-indigo-500 shadow-md flex items-center justify-center font-display font-extrabold text-3xl text-indigo-900">
                        🎲
                    </div>
                    <button id="btn-roll-dice" onclick="rollDice()" class="btn-game flex-1 sm:flex-none bg-emerald-500 hover:bg-emerald-400 disabled:opacity-50 disabled:cursor-not-allowed text-white font-display font-bold text-base sm:text-xl px-6 py-3.5 rounded-2xl border-2 border-emerald-700 flex items-center justify-center gap-2">
                        <span>🎲</span> Baling Dadu!
                    </button>
                </div>
            </div>
        </section>

        <!-- ==================== MODAL: KAD SOALAN / CABARAN PETAK ==================== -->
        <section id="modal-challenge" class="hidden fixed inset-0 z-40 bg-slate-900/70 backdrop-blur-sm flex items-center justify-center p-3 sm:p-5">
            <div class="bg-white rounded-3xl border-4 border-sky-400 max-w-2xl w-full max-h-[92vh] flex flex-col shadow-2xl animate-pop overflow-hidden">
                <!-- Kepala Kad Soalan -->
                <div id="chal-header" class="bg-sky-500 text-white px-5 py-3 flex items-center justify-between">
                    <div class="flex items-center gap-2">
                        <span id="chal-type-icon" class="text-2xl">📘</span>
                        <div>
                            <span id="chal-type-badge" class="text-[11px] font-extrabold uppercase tracking-wider bg-white/25 px-2.5 py-0.5 rounded-full">
                                Soalan Pengukuhan
                            </span>
                            <h3 id="chal-title" class="font-display font-bold text-lg sm:text-xl leading-tight mt-0.5">
                                Cabaran Ilmu
                            </h3>
                        </div>
                    </div>
                    <span id="chal-tile-num" class="bg-white/25 font-display font-bold text-xs sm:text-sm px-3 py-1 rounded-xl">
                        Soalan 1 / 12
                    </span>
                </div>

                <!-- Isi Soalan Interaktif -->
                <div class="p-4 sm:p-6 overflow-y-auto space-y-4">
                    <p id="chal-question" class="font-display font-bold text-base sm:text-xl text-slate-800 leading-snug bg-sky-50 border-2 border-sky-200 rounded-2xl p-4 whitespace-pre-line">
                        Soalan akan dipaparkan di sini...
                    </p>

                    <!-- Kawasan Interaktif Dinamik (MCQ / Susun Urutan 1-2-3-4 / Isi Tempat Kosong) -->
                    <div id="chal-interactive-area" class="space-y-2.5"></div>

                    <!-- Kotak Maklum Balas Segera -->
                    <div id="chal-feedback" class="hidden rounded-2xl p-4 border-2 animate-pop">
                        <div class="flex items-start gap-3">
                            <span id="chal-fb-icon" class="text-2xl">🌟</span>
                            <div class="flex-1">
                                <h4 id="chal-fb-title" class="font-display font-bold text-base sm:text-lg">Jawapan Tepat!</h4>
                                <p id="chal-fb-desc" class="text-xs sm:text-sm font-semibold mt-0.5"></p>
                            </div>
                        </div>
                        <div class="mt-3 flex justify-end">
                            <button id="chal-continue-btn" onclick="closeChallengeModal()" class="btn-game bg-indigo-600 hover:bg-indigo-500 text-white font-display font-bold px-5 py-2.5 rounded-xl border-2 border-indigo-800 text-sm sm:text-base">
                                Teruskan Perjalanan ➡️
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ==================== MODAL: NOTA RINGKAS LENGKAP ==================== -->
        <section id="modal-notes" class="hidden fixed inset-0 z-50 bg-slate-900/70 backdrop-blur-sm flex items-center justify-center p-2.5 sm:p-5">
            <div class="bg-white rounded-3xl border-4 border-amber-400 max-w-3xl w-full max-h-[92vh] flex flex-col shadow-2xl animate-pop overflow-hidden">
                <div class="bg-amber-400 px-5 py-3 flex items-center justify-between border-b-4 border-amber-500">
                    <h2 class="font-display font-bold text-lg sm:text-2xl text-amber-950 flex items-center gap-2">
                        <span>📖</span> Nota Ringkas: Peranan Azan & Iqamat
                    </h2>
                    <button onclick="closeNotes()" class="bg-white hover:bg-rose-100 text-rose-600 font-bold w-9 h-9 rounded-full border-2 border-amber-600 flex items-center justify-center text-lg shrink-0">
                        ✖
                    </button>
                </div>

                <div class="p-4 sm:p-6 overflow-y-auto space-y-4 text-sm sm:text-base">
                    <!-- Peranan Azan & Peranan Iqamat -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="bg-emerald-50 border-3 border-emerald-500 rounded-2xl p-4">
                            <div class="inline-block bg-emerald-600 text-white font-display font-bold px-4 py-1 rounded-full text-base mb-2">
                                🕌 Peranan Azan
                            </div>
                            <p class="text-xs font-bold text-emerald-800 bg-white border border-emerald-200 rounded-xl p-2 mb-2 text-center">
                                Lafaz Permulaan: <em>"Allahu Akbar Allahu Akbar..."</em>
                            </p>
                            <ul class="space-y-1 text-slate-700 font-semibold text-xs sm:text-sm list-disc pl-5">
                                <li>Memberitahu umat Islam bahawa <strong>waktu solat fardu telah masuk</strong>.</li>
                                <li>Mengajak mereka bersiap sedia dan menunaikan solat secara berjemaah.</li>
                            </ul>
                        </div>

                        <div class="bg-pink-50 border-3 border-pink-500 rounded-2xl p-4">
                            <div class="inline-block bg-pink-600 text-white font-display font-bold px-4 py-1 rounded-full text-base mb-2">
                                🧎 Peranan Iqamat
                            </div>
                            <p class="text-xs font-bold text-pink-800 bg-white border border-pink-200 rounded-xl p-2 mb-2 text-center">
                                Lafaz Khusus: <em>"Qad Qamat as-Solah..."</em>
                            </p>
                            <ul class="space-y-1 text-slate-700 font-semibold text-xs sm:text-sm list-disc pl-5">
                                <li>Seruan yang menandakan <strong>solat berjemaah akan dimulakan tidak lama lagi</strong>.</li>
                            </ul>
                        </div>
                    </div>

                    <!-- 4 Langkah Susunan Ibadah (Urutan yang Diperbaiki) -->
                    <div class="bg-indigo-50 border-2 border-indigo-300 rounded-2xl p-4">
                        <div class="inline-block bg-indigo-800 text-white font-display font-bold px-4 py-1 rounded-full text-sm mb-3">
                            🔢 4 Langkah Susunan Ibadah
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-2.5 text-xs">
                            <div class="bg-white border-2 border-emerald-400 rounded-xl p-3">
                                <span class="bg-emerald-600 text-white font-bold w-6 h-6 rounded-full inline-flex items-center justify-center mb-1">1</span>
                                <p class="font-bold text-emerald-800">Azan</p>
                                <p class="font-semibold text-slate-700 mt-0.5">Apabila masuk waktu solat, <strong>azan</strong> dilaungkan.</p>
                            </div>
                            <div class="bg-white border-2 border-sky-400 rounded-xl p-3">
                                <span class="bg-sky-600 text-white font-bold w-6 h-6 rounded-full inline-flex items-center justify-center mb-1">2</span>
                                <p class="font-bold text-sky-800">Pergi ke masjid</p>
                                <p class="font-semibold text-slate-700 mt-0.5">Umat Islam bersiap sedia, mengambil wuduk dan <strong>pergi ke masjid</strong>.</p>
                            </div>
                            <div class="bg-white border-2 border-pink-400 rounded-xl p-3">
                                <span class="bg-pink-600 text-white font-bold w-6 h-6 rounded-full inline-flex items-center justify-center mb-1">3</span>
                                <p class="font-bold text-pink-800">Iqamat</p>
                                <p class="font-semibold text-slate-700 mt-0.5"><strong>Iqamat</strong> dilaungkan sejurus sebelum solat dimulakan.</p>
                            </div>
                            <div class="bg-white border-2 border-indigo-400 rounded-xl p-3">
                                <span class="bg-indigo-600 text-white font-bold w-6 h-6 rounded-full inline-flex items-center justify-center mb-1">4</span>
                                <p class="font-bold text-indigo-800">Masuk saf untuk solat</p>
                                <p class="font-semibold text-slate-700 mt-0.5">Jemaah <strong>masuk saf untuk solat</strong> dengan lurus dan rapat bersama imam.</p>
                            </div>
                        </div>
                    </div>

                    <!-- Perbezaan Utama, Persamaan & Cara Mudah Mengingatinya -->
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-xs sm:text-sm">
                        <div class="bg-sky-50 border-2 border-sky-300 rounded-2xl p-4 space-y-2">
                            <div class="inline-block bg-indigo-800 text-white font-display font-bold px-3.5 py-0.5 rounded-full text-sm">
                                ⚖️ Perbezaan Utama & Persamaan
                            </div>
                            <p class="text-slate-700 font-semibold">
                                • <strong class="text-emerald-700">Persamaan:</strong> Kedua-duanya ialah seruan penting berkaitan solat yang membantu umat Islam bersedia menunaikan solat.<br>
                                • <strong class="text-rose-600">Tujuan:</strong> Azan menandakan masuknya waktu solat; Iqamat menandakan solat berjemaah akan bermula.<br>
                                • <strong class="text-indigo-700">Waktu:</strong> Azan dilaungkan lebih awal (apabila masuk waktu solat); Iqamat sejurus sebelum solat dimulakan.
                            </p>
                        </div>

                        <div class="bg-amber-50 border-2 border-amber-400 rounded-2xl p-4 flex flex-col justify-between">
                            <div>
                                <div class="inline-block bg-indigo-800 text-white font-display font-bold px-3.5 py-0.5 rounded-full text-sm mb-2">
                                    🧠 Cara Mudah Mengingatinya
                                </div>
                                <p class="font-bold text-slate-800 mb-2">
                                    🟢 <span class="text-emerald-700">Azan</span> = panggilan untuk datang menunaikan solat.<br>
                                    🌸 <span class="text-pink-700">Iqamat</span> = tanda bahawa solat akan bermula sekarang.
                                </p>
                            </div>
                            <div class="bg-amber-300/90 border border-amber-500 rounded-xl p-2.5 text-center font-extrabold text-amber-950 text-xs">
                                Tertibnya: Azan ➔ Pergi ke masjid ➔ Iqamat ➔ Masuk saf untuk solat
                            </div>
                        </div>
                    </div>
                </div>

                <div class="bg-slate-50 px-5 py-3 border-t-2 border-slate-200 flex justify-end">
                    <button onclick="closeNotes()" class="btn-game bg-emerald-500 hover:bg-emerald-400 text-white font-display font-bold px-6 py-2.5 rounded-xl border-2 border-emerald-700">
                        Faham! Teruskan 🚀
                    </button>
                </div>
            </div>
        </section>

        <!-- ==================== SKRIN 3: TAMAT KEMBARA / KEMENANGAN ==================== -->
        <section id="screen-end" class="hidden w-full max-w-xl bg-white/95 rounded-3xl shadow-2xl border-4 border-amber-400 p-6 sm:p-8 text-center animate-pop relative overflow-hidden">
            <div id="end-stars" class="text-3xl sm:text-4xl mb-1 tracking-widest">⭐⭐⭐</div>
            <div id="end-badge-icon" class="text-6xl sm:text-7xl my-2 animate-bounce">🕌</div>
            <div id="end-badge-title" class="inline-block bg-amber-400 text-amber-950 font-display font-bold px-4 py-1 rounded-full text-sm sm:text-base border-2 border-amber-600 mb-2">
                Lencana: Duta Tertib Ibadah 🌟
            </div>
            <h2 id="end-heading" class="font-display font-extrabold text-2xl sm:text-4xl text-emerald-700 mb-2">
                Tahniah! Semua 12 Soalan Selesai!
            </h2>
            <p id="end-message" class="text-slate-600 font-semibold text-sm sm:text-base mb-5">
                Alhamdulillah! Kamu telah menyelesaikan kesemua 12 soalan pengukuhan dan selamat masuk ke Masjid Al-Falah!
            </p>

            <!-- Rumusan Prestasi -->
            <div class="grid grid-cols-3 gap-2.5 mb-5">
                <div class="bg-amber-50 border-2 border-amber-300 rounded-2xl p-3">
                    <span class="text-[11px] font-bold text-amber-800 uppercase block">Jumlah Markah</span>
                    <div id="end-score" class="font-display font-extrabold text-xl sm:text-2xl text-amber-950">⭐ 0</div>
                </div>
                <div class="bg-emerald-50 border-2 border-emerald-300 rounded-2xl p-3">
                    <span class="text-[11px] font-bold text-emerald-700 uppercase block">Ketepatan Jawapan</span>
                    <div id="end-accuracy" class="font-display font-extrabold text-xl sm:text-2xl text-emerald-950">100%</div>
                </div>
                <div class="bg-sky-50 border-2 border-sky-300 rounded-2xl p-3">
                    <span class="text-[11px] font-bold text-sky-700 uppercase block">Pusingan Laluan</span>
                    <div id="end-laps" class="font-display font-extrabold text-xl sm:text-2xl text-sky-950">1</div>
                </div>
            </div>

            <div class="bg-pink-50 border-2 border-pink-300 rounded-2xl p-3 mb-6 text-xs sm:text-sm text-pink-950 font-bold">
                💖 "Tertibnya ialah azan, pergi ke masjid, iqamat, dan masuk saf untuk solat.<br>Solat Teratur, Hidup Lebih Bermakna!"
            </div>

            <div class="flex flex-col sm:flex-row gap-3 justify-center">
                <button onclick="openNotes()" class="btn-game bg-amber-400 hover:bg-amber-300 text-amber-950 font-display font-bold px-5 py-3.5 rounded-2xl border-2 border-amber-600">
                    📖 Semak Nota
                </button>
                <button onclick="startGame()" class="btn-game bg-emerald-500 hover:bg-emerald-400 text-white font-display font-bold px-7 py-3.5 rounded-2xl border-2 border-emerald-700">
                    🔄 Main Semula
                </button>
            </div>
        </section>

    </main>

    <!-- Kanvas Konfeti Kemenangan -->
    <canvas id="confetti-canvas" class="fixed inset-0 pointer-events-none z-50 hidden"></canvas>

    <!-- Pengaki Halaman -->
    <footer class="relative z-10 text-center text-xs text-slate-700 font-bold mt-2">
        Jom Cintai Solat 💖 | Latihan Pengukuhan Pendidikan Islam Tahun 4
    </footer>

    <script>
        /* =========================================================================
           1. ENJIN BUNYI SINTETIK (WEB AUDIO API)
           ========================================================================= */
        let soundEnabled = true;
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                const AudioContext = window.AudioContext || window.webkitAudioContext;
                audioCtx = new AudioContext();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
        }

        function toggleSound() {
            soundEnabled = !soundEnabled;
            const icon = soundEnabled ? '🔊' : '🔇';
            const text = soundEnabled ? 'Bunyi: Hidup' : 'Bunyi: Senyap';
            document.getElementById('sound-icon-menu').textContent = icon;
            document.getElementById('sound-text-menu').textContent = text;
            document.getElementById('sound-btn-hud').textContent = icon;
            if (soundEnabled) playSound('click');
        }

        function playSound(type) {
            if (!soundEnabled) return;
            try {
                initAudio();
                const now = audioCtx.currentTime;
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.connect(gain);
                gain.connect(audioCtx.destination);

                if (type === 'click') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(520, now);
                    osc.frequency.exponentialRampToValueAtTime(760, now + 0.08);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
                    osc.start(now);
                    osc.stop(now + 0.08);
                } else if (type === 'dice') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(280 + Math.random() * 240, now);
                    gain.gain.setValueAtTime(0.11, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.05);
                    osc.start(now);
                    osc.stop(now + 0.05);
                } else if (type === 'step') {
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(420, now);
                    osc.frequency.exponentialRampToValueAtTime(640, now + 0.09);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.09);
                    osc.start(now);
                    osc.stop(now + 0.09);
                } else if (type === 'bonus') {
                    const notes = [587.33, 783.99, 1046.50];
                    notes.forEach((f, i) => {
                        const o = audioCtx.createOscillator();
                        const g = audioCtx.createGain();
                        o.type = 'sine';
                        o.connect(g);
                        g.connect(audioCtx.destination);
                        o.frequency.setValueAtTime(f, now + i * 0.08);
                        g.gain.setValueAtTime(0.16, now + i * 0.08);
                        g.gain.exponentialRampToValueAtTime(0.01, now + i * 0.08 + 0.22);
                        o.start(now + i * 0.08);
                        o.stop(now + i * 0.08 + 0.22);
                    });
                } else if (type === 'correct') {
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(523.25, now);
                    osc.frequency.setValueAtTime(659.25, now + 0.09);
                    osc.frequency.setValueAtTime(783.99, now + 0.18);
                    gain.gain.setValueAtTime(0.18, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.32);
                    osc.start(now);
                    osc.stop(now + 0.32);
                } else if (type === 'wrong') {
                    osc.type = 'sawtooth';
                    osc.frequency.setValueAtTime(220, now);
                    osc.frequency.linearRampToValueAtTime(145, now + 0.24);
                    gain.gain.setValueAtTime(0.14, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.24);
                    osc.start(now);
                    osc.stop(now + 0.24);
                } else if (type === 'win') {
                    const notes = [523.25, 659.25, 783.99, 1046.50, 1318.51];
                    notes.forEach((freq, i) => {
                        const o = audioCtx.createOscillator();
                        const g = audioCtx.createGain();
                        o.type = 'triangle';
                        o.connect(g);
                        g.connect(audioCtx.destination);
                        o.frequency.setValueAtTime(freq, now + i * 0.11);
                        g.gain.setValueAtTime(0.2, now + i * 0.11);
                        g.gain.exponentialRampToValueAtTime(0.01, now + i * 0.11 + 0.36);
                        o.start(now + i * 0.11);
                        o.stop(now + i * 0.11 + 0.36);
                    });
                }
            } catch (e) {
                console.warn('Ralat audio:', e);
            }
        }

        /* =========================================================================
           2. DATA PAPAN LALUAN PUSINGAN (20 PETAK) & KOLAM 12 SOALAN TANPA ULANGAN
           ========================================================================= */
        const TOTAL_TILES = 20;
        const TOTAL_QUESTIONS = 12;

        // 20 Petak Laluan Pusingan Tertutup (Mengelilingi Masjid Al-Falah di tengah)
        const boardTilesConfig = [
            { num: 1, type: 'START', label: 'MULA', sub: 'Rumah', icon: '🏡' },
            { num: 2, type: 'INFO', label: 'Peranan', sub: 'Azan', icon: '📢' },
            { num: 3, type: 'INFO', label: 'Lafaz', sub: 'Seruan', icon: '🗣️' },
            { num: 4, type: 'BONUS', label: 'Bonus', sub: '+20 Markah', icon: '⭐' },
            { num: 5, type: 'INFO', label: 'Pergi ke', sub: 'Masjid', icon: '🚶' },
            { num: 6, type: 'REMINDER', label: 'Peringatan', sub: 'Berwuduk', icon: '💧' },
            { num: 7, type: 'INFO', label: 'Peranan', sub: 'Iqamat', icon: '🧎' },
            { num: 8, type: 'INFO', label: 'Masuk Saf', sub: 'Solat', icon: '🕌' },
            { num: 9, type: 'BONUS', label: 'Bonus', sub: '+20 Markah', icon: '⭐' },
            { num: 10, type: 'INFO', label: 'Susunan', sub: 'Ibadah', icon: '🔢' },
            { num: 11, type: 'REMINDER', label: 'Peringatan', sub: 'Lurus Saf', icon: '🌸' },
            { num: 12, type: 'INFO', label: 'Tujuan', sub: 'Berbeza', icon: '🎯' },
            { num: 13, type: 'INFO', label: 'Waktu', sub: 'Laungan', icon: '🕒' },
            { num: 14, type: 'BONUS', label: 'Bonus', sub: '+20 Markah', icon: '⭐' },
            { num: 15, type: 'INFO', label: 'Cara Ingat', sub: 'Azan', icon: '🧠' },
            { num: 16, type: 'INFO', label: 'Cara Ingat', sub: 'Iqamat', icon: '⚡' },
            { num: 17, type: 'REMINDER', label: 'Peringatan', sub: 'Sedia Solat', icon: '🌸' },
            { num: 18, type: 'INFO', label: 'Kesimpulan', sub: 'Seruan', icon: '📣' },
            { num: 19, type: 'INFO', label: 'Tertib', sub: '4 Langkah', icon: '📋' },
            { num: 20, type: 'INFO', label: 'Takbiratul', sub: 'Ihram', icon: '🤲' }
        ];

        // 12 Soalan Pengukuhan Unik (Setiap soalan ditanda selesai selepas dijawab & tidak berulang)
        const challengeBank = [
            {
                id: 1,
                mode: 'MCQ',
                tag: 'Peranan Azan',
                title: 'Seruan Masuk Waktu Solat',
                question: "Apabila masuk waktu solat fardu, bilal melaungkan 'Allahu Akbar Allahu Akbar...'. Apakah peranan utama Azan?",
                options: [
                    "Memberitahu waktu solat fardu telah masuk dan mengajak umat Islam menunaikan solat berjemaah",
                    "Menandakan solat berjemaah sudah selesai dan jemaah boleh pulang",
                    "Meminta jemaah duduk diam mendengar khutbah Jumaat sahaja",
                    "Menandakan masuk waktu berbuka puasa di bulan Ramadan"
                ],
                correct: 0,
                explanation: "Tepat! Azan ialah seruan untuk memberitahu umat Islam bahawa waktu solat fardu telah masuk serta mengajak bersiap sedia menunaikan solat berjemaah."
            },
            {
                id: 2,
                mode: 'MCQ',
                tag: 'Peranan Iqamat',
                title: 'Seruan Sebelum Solat Bermula',
                question: "Bilal melaungkan 'Qad Qamat as-Solah, Qad Qamat as-Solah...'. Apakah peranan Iqamat?",
                options: [
                    "Menandakan waktu solat fardu baru sahaja tamat",
                    "Menandakan solat berjemaah akan dimulakan tidak lama lagi",
                    "Memanggil penduduk kampung datang dari rumah ke masjid",
                    "Memberitahu jemaah supaya mengambil wuduk terlebih dahulu"
                ],
                correct: 1,
                explanation: "Betul! Iqamat ialah seruan yang menandakan solat berjemaah akan dimulakan tidak lama lagi."
            },
            {
                id: 3,
                mode: 'ORDER',
                tag: 'Susunan Ibadah (1–4)',
                title: 'Susun 4 Langkah Tertib Ibadah!',
                question: "Klik langkah-langkah di bawah mengikut urutan tertib ibadah yang betul dari Langkah 1 hingga Langkah 4:",
                steps: [
                    { id: 1, icon: "📢", text: "Azan" },
                    { id: 2, icon: "🕌", text: "Pergi ke masjid" },
                    { id: 3, icon: "🗣️", text: "Iqamat" },
                    { id: 4, icon: "🧎", text: "Masuk saf untuk solat" }
                ],
                explanation: "Sempurna! Susunan tertib ibadah yang betul ialah: 1. Azan ➔ 2. Pergi ke masjid ➔ 3. Iqamat ➔ 4. Masuk saf untuk solat."
            },
            {
                id: 4,
                mode: 'MCQ',
                tag: 'Perbezaan Utama: Tujuan',
                title: 'Perbezaan Tujuan Azan & Iqamat',
                question: "Apakah perbezaan TUJUAN antara Azan dan Iqamat?",
                options: [
                    "Azan menandakan masuknya waktu solat; Iqamat menandakan solat berjemaah akan bermula",
                    "Azan untuk solat sunat; Iqamat untuk solat fardu sahaja",
                    "Azan dilaungkan selepas solat; Iqamat sebelum masuk waktu",
                    "Azan dibaca oleh imam; Iqamat dibaca oleh makmum"
                ],
                correct: 0,
                explanation: "Syabas! Azan menandakan masuknya waktu solat dan menyeru umat Islam menunaikannya, manakala iqamat menandakan solat berjemaah akan bermula."
            },
            {
                id: 5,
                mode: 'MCQ',
                tag: 'Perbezaan Utama: Waktu',
                title: 'Bilakah Azan & Iqamat Dilaungkan?',
                question: "Bagaimanakah perbezaan WAKTU antara laungan Azan dan Iqamat?",
                options: [
                    "Iqamat dilaungkan lebih awal daripada Azan",
                    "Azan dilaungkan lebih awal apabila masuk waktu solat; Iqamat sejurus sebelum solat dimulakan",
                    "Kedua-duanya dilaungkan serentak sebelum jemaah pergi ke masjid",
                    "Azan dilaungkan selepas imam mengangkat takbiratul ihram"
                ],
                correct: 1,
                explanation: "Betul! Azan dilaungkan lebih awal (apabila masuk waktu solat), manakala iqamat dilaungkan sejurus sebelum solat dimulakan."
            },
            {
                id: 6,
                mode: 'FILL',
                tag: 'Cara Mudah Mengingatinya',
                title: 'Lengkapkan Formula Ingatan Azan!',
                question: "Lengkapkan ayat di bawah:\n\"Azan = _______________ untuk datang menunaikan solat.\"",
                blanks: ["panggilan", "tanda", "peraturan", "ucapan"],
                correctWord: "panggilan",
                explanation: "Tepat sekali! Azan = panggilan untuk datang menunaikan solat."
            },
            {
                id: 7,
                mode: 'FILL',
                tag: 'Cara Mudah Mengingatinya',
                title: 'Lengkapkan Formula Ingatan Iqamat!',
                question: "Lengkapkan ayat di bawah:\n\"Iqamat = _______________ bahawa solat akan bermula sekarang.\"",
                blanks: ["tanda", "panggilan", "ceramah", "salam"],
                correctWord: "tanda",
                explanation: "Bagus! Iqamat = tanda bahawa solat akan bermula sekarang."
            },
            {
                id: 8,
                mode: 'MCQ',
                tag: 'Langkah Selepas Azan',
                title: 'Tindakan Selepas Mendengar Azan',
                question: "Ahmad mendengar laungan azan Zuhur dari masjid berhampiran rumahnya. Apakah langkah tertib seterusnya yang perlu Ahmad lakukan?",
                options: [
                    "Bersiap sedia, mengambil wuduk dan pergi ke masjid",
                    "Terus mengangkat takbiratul ihram di ruang tamu",
                    "Menunggu sehingga solat berjemaah selesai",
                    "Tidur semula kerana waktu solat masih panjang"
                ],
                correct: 0,
                explanation: "Cemerlang! Selepas azan dilaungkan, langkah seterusnya ialah bersiap sedia, berwuduk dan pergi ke masjid."
            },
            {
                id: 9,
                mode: 'MCQ',
                tag: 'Langkah Selepas Iqamat',
                title: 'Tindakan Apabila Iqamat Dilaungkan',
                question: "Apabila jemaah telah tiba di masjid dan bersedia, bilal pun melaungkan iqamat. Apakah langkah tertib selepas iqamat?",
                options: [
                    "Masuk saf untuk solat dengan lurus dan rapat bersama imam",
                    "Keluar semula dari masjid untuk berbual-bual",
                    "Melaungkan azan sekali lagi",
                    "Duduk bersandar di dinding masjid"
                ],
                correct: 0,
                explanation: "Tepat! Selepas iqamat dilaungkan, jemaah masuk saf untuk solat dengan lurus dan rapat sebelum imam mengangkat takbiratul ihram."
            },
            {
                id: 10,
                mode: 'ORDER',
                tag: 'Huraian Susunan Ibadah',
                title: 'Susun 4 Huraian Tertib Ibadah!',
                question: "Susun 4 huraian langkah ibadah ini dari awal hingga akhir (Langkah 1 hingga Langkah 4):",
                steps: [
                    { id: 1, icon: "📢", text: "Azan dilaungkan apabila masuk waktu solat fardu." },
                    { id: 2, icon: "🚶", text: "Umat Islam berwuduk dan pergi ke masjid." },
                    { id: 3, icon: "🗣️", text: "Iqamat dilaungkan sejurus sebelum solat dimulakan." },
                    { id: 4, icon: "🤲", text: "Jemaah masuk saf untuk solat bersama imam." }
                ],
                explanation: "Hebat! Urutannya ialah: Azan ➔ Pergi ke masjid ➔ Iqamat ➔ Masuk saf untuk solat."
            },
            {
                id: 11,
                mode: 'MCQ',
                tag: 'Persamaan & Kesimpulan',
                title: 'Persamaan & Kesimpulan Azan dan Iqamat',
                question: "Apakah KESIMPULAN yang tepat tentang Azan dan Iqamat?",
                options: [
                    "Kedua-duanya ialah seruan penting berkaitan solat, tetapi azan lebih awal manakala iqamat sejurus sebelum solat",
                    "Hanya azan sahaja yang penting dalam ibadah solat berjemaah",
                    "Iqamat dilaungkan lebih awal daripada azan pada setiap waktu solat",
                    "Azan dan iqamat tidak mempunyai sebarang kaitan dengan persediaan solat"
                ],
                correct: 0,
                explanation: "Betul! Kedua-duanya ialah seruan penting berkaitan solat, tetapi azan lebih awal manakala iqamat sejurus sebelum solat."
            },
            {
                id: 12,
                mode: 'MCQ',
                tag: 'Lafaz Khusus Seruan',
                title: 'Mengenal Pasti Lafaz Iqamat',
                question: "Manakah antara berikut merupakan lafaz khusus yang menandakan seruan Iqamat?",
                options: [
                    "Qad Qamat as-Solah, Qad Qamat as-Solah",
                    "Allahu Akbar, Allahu Akbar",
                    "Assalamualaikum Warahmatullah",
                    "Subhanallah Walhamdulillah"
                ],
                correct: 0,
                explanation: "Syabas! Lafaz 'Qad Qamat as-Solah' menandakan solat berjemaah akan didirikan (dimulakan) tidak lama lagi."
            }
        ];

        const reminderEvents = [
            {
                title: "Peringatan Tertib: Berwuduk & Pergi ke Masjid!",
                text: "Selepas azan dilaungkan, ambil wuduk dengan sempurna dan segera pergi ke masjid. Jawab soalan pengukuhan ini!"
            },
            {
                title: "Peringatan Tertib: Lurus & Rapatkan Saf!",
                text: "Apabila iqamat dilaungkan, segera masuk saf untuk solat dengan lurus dan rapat di belakang imam. Mari jawab soalan ini!"
            },
            {
                title: "Peringatan Tertib: Jaga Adab di Masjid!",
                text: "Ingat urutan tertib: Azan ➔ Pergi ke masjid ➔ Iqamat ➔ Masuk saf untuk solat. Jawab soalan seterusnya!"
            }
        ];

        /* =========================================================================
           3. KEADAAN PERMAINAN & KAWALAN ASAS
           ========================================================================= */
        const gameState = {
            avatar: 'boy',
            position: 1,
            laps: 1,
            score: 0,
            correctCount: 0,
            answeredIds: [],
            enteredMasjid: false,
            isMoving: false,
            isPaused: false,
            challengeOpen: false
        };

        const screens = ['screen-menu', 'screen-board', 'screen-end'];

        function showScreen(id) {
            screens.forEach(s => {
                const el = document.getElementById(s);
                if (el) el.classList.add('hidden');
            });
            const target = document.getElementById(id);
            if (target) target.classList.remove('hidden');

            const hud = document.getElementById('game-hud');
            if (id === 'screen-board') {
                hud.classList.remove('hidden');
            } else {
                hud.classList.add('hidden');
            }
        }

        function selectAvatar(type) {
            playSound('click');
            gameState.avatar = type;
            const boyBtn = document.getElementById('char-boy');
            const girlBtn = document.getElementById('char-girl');

            if (type === 'boy') {
                boyBtn.className = 'btn-game bg-emerald-100 border-3 border-emerald-600 rounded-2xl p-3 flex flex-col items-center ring-4 ring-emerald-300 transition';
                girlBtn.className = 'btn-game bg-white border-3 border-slate-300 rounded-2xl p-3 flex flex-col items-center opacity-80 hover:opacity-100 transition';
            } else {
                girlBtn.className = 'btn-game bg-pink-100 border-3 border-pink-600 rounded-2xl p-3 flex flex-col items-center ring-4 ring-pink-300 transition';
                boyBtn.className = 'btn-game bg-white border-3 border-slate-300 rounded-2xl p-3 flex flex-col items-center opacity-80 hover:opacity-100 transition';
            }
        }

        function updateHUD() {
            const answeredTotal = gameState.answeredIds.length;
            const remainingTotal = TOTAL_QUESTIONS - answeredTotal;

            document.getElementById('hud-progress').textContent = `${answeredTotal} / ${TOTAL_QUESTIONS}`;
            document.getElementById('hud-remaining').textContent = `${remainingTotal} Soalan`;
            document.getElementById('hud-position').textContent = gameState.enteredMasjid
                ? 'Di Dalam Masjid 🕌'
                : `Pusingan ${gameState.laps} • Petak ${gameState.position}`;
            document.getElementById('hud-score').textContent = `⭐ ${gameState.score}`;

            renderQuestionTrackerDots();
        }

        function renderQuestionTrackerDots() {
            const container = document.getElementById('question-tracker-dots');
            if (!container) return;
            container.innerHTML = '';

            for (let qNum = 1; qNum <= TOTAL_QUESTIONS; qNum++) {
                const isDone = gameState.answeredIds.includes(qNum);
                const badge = document.createElement('span');
                badge.className = isDone
                    ? 'w-6 h-6 rounded-full bg-emerald-500 text-white font-display font-bold text-[11px] flex items-center justify-center shadow-sm'
                    : 'w-6 h-6 rounded-full bg-white border-2 border-slate-300 text-slate-500 font-display font-bold text-[11px] flex items-center justify-center';
                badge.textContent = isDone ? '✓' : qNum;
                badge.title = isDone ? `Soalan ${qNum}: Selesai` : `Soalan ${qNum}: Belum dijawab`;
                container.appendChild(badge);
            }
        }

        function openNotes() {
            playSound('click');
            document.getElementById('modal-notes').classList.remove('hidden');
            const hudActive = !document.getElementById('game-hud').classList.contains('hidden');
            if (hudActive && !gameState.isPaused) {
                gameState.isPaused = true;
            }
        }

        function closeNotes() {
            playSound('click');
            document.getElementById('modal-notes').classList.add('hidden');
            const pauseHidden = document.getElementById('modal-pause').classList.contains('hidden');
            const hudActive = !document.getElementById('game-hud').classList.contains('hidden');
            if (hudActive && pauseHidden && gameState.isPaused) {
                gameState.isPaused = false;
            }
        }

        function togglePause() {
            const hudHidden = document.getElementById('game-hud').classList.contains('hidden');
            if (hudHidden) return;

            playSound('click');
            gameState.isPaused = !gameState.isPaused;
            const pauseModal = document.getElementById('modal-pause');

            if (gameState.isPaused) {
                pauseModal.classList.remove('hidden');
            } else {
                pauseModal.classList.add('hidden');
            }
        }

        function restartFromPause() {
            gameState.isPaused = false;
            document.getElementById('modal-pause').classList.add('hidden');
            document.getElementById('modal-challenge').classList.add('hidden');
            startGame();
        }

        function menuFromPause() {
            gameState.isPaused = false;
            document.getElementById('modal-pause').classList.add('hidden');
            document.getElementById('modal-challenge').classList.add('hidden');
            returnToMenu();
        }

        function returnToMenu() {
            playSound('click');
            gameState.isPaused = false;
            gameState.isMoving = false;
            gameState.challengeOpen = false;
            document.getElementById('modal-pause').classList.add('hidden');
            document.getElementById('modal-challenge').classList.add('hidden');
            stopConfetti();
            showScreen('screen-menu');
        }

        window.addEventListener('keydown', (e) => {
            if (e.key === 'p' || e.key === 'P' || e.key === 'Escape') {
                const notesHidden = document.getElementById('modal-notes').classList.contains('hidden');
                if (notesHidden) {
                    e.preventDefault();
                    togglePause();
                }
            }
        });

        /* =========================================================================
           4. PENJANAAN PAPAN LALUAN PUSINGAN (20 PETAK + MASJID TENGAH) & DADU RAWAK 1-6
           ========================================================================= */
        function getTrueRandomDice() {
            // Menggunakan crypto.getRandomValues jika disokong untuk kerawakan sebenar 1..6
            if (window.crypto && window.crypto.getRandomValues) {
                const arr = new Uint32Array(1);
                window.crypto.getRandomValues(arr);
                return (arr[0] % 6) + 1;
            }
            return Math.floor(Math.random() * 6) + 1;
        }

        function startGame() {
            playSound('click');
            stopConfetti();

            gameState.position = 1;
            gameState.laps = 1;
            gameState.score = 0;
            gameState.correctCount = 0;
            gameState.answeredIds = [];
            gameState.enteredMasjid = false;
            gameState.isMoving = false;
            gameState.isPaused = false;
            gameState.challengeOpen = false;

            const isBoy = gameState.avatar === 'boy';
            document.getElementById('player-badge-icon').textContent = isBoy ? '👦🏻' : '🧕🏻';
            document.getElementById('player-badge-name').textContent = isBoy ? 'Pengembara: Ahmad' : 'Pengembara: Aisyah';
            document.getElementById('turn-instruction').textContent = 'Tekan "Baling Dadu!" untuk mendapat mata dadu rawak (1–6) dan jawab soalan pengukuhan.';
            document.getElementById('board-status-banner').textContent = 'Giliran Anda: Baling Dadu (1–6)!';
            document.getElementById('dice-box').textContent = '🎲';
            document.getElementById('btn-roll-dice').disabled = false;

            updateHUD();
            renderBoard();
            showScreen('screen-board');
        }

        function renderBoard() {
            const grid = document.getElementById('board-grid');
            grid.innerHTML = '';
            grid.className = 'grid grid-cols-6 gap-1 sm:gap-2 mb-3';

            const avatarEmoji = gameState.avatar === 'boy' ? '👦🏻' : '🧕🏻';

            // Peta koordinat (row 0..5, col 0..5) kepada nombor petak 1..20 (Laluan Pusingan Tertutup)
            const coordToTile = {
                '0,0': 1,  '0,1': 2,  '0,2': 3,  '0,3': 4,  '0,4': 5,  '0,5': 6,
                '1,5': 7,  '2,5': 8,  '3,5': 9,  '4,5': 10,
                '5,5': 11, '5,4': 12, '5,3': 13, '5,2': 14, '5,1': 15, '5,0': 16,
                '4,0': 17, '3,0': 18, '2,0': 19, '1,0': 20
            };

            const arrowDirection = {
                1: '➡️', 2: '➡️', 3: '➡️', 4: '➡️', 5: '➡️', 6: '⬇️',
                7: '⬇️', 8: '⬇️', 9: '⬇️', 10: '⬇️',
                11: '⬅️', 12: '⬅️', 13: '⬅️', 14: '⬅️', 15: '⬅️', 16: '⬆️',
                17: '⬆️', 18: '⬆️', 19: '⬆️', 20: '➡️'
            };

            for (let r = 0; r < 6; r++) {
                for (let c = 0; c < 6; c++) {
                    // Kawasan tengah 4x4 (r=1..4, c=1..4) menempatkan Masjid Al-Falah
                    if (r === 1 && c === 1) {
                        const masjidCard = document.createElement('div');
                        masjidCard.className = `col-span-4 row-span-4 rounded-3xl border-4 border-emerald-500 bg-gradient-to-b from-sky-100 via-emerald-50 to-emerald-100 p-3 sm:p-4 flex flex-col items-center justify-center text-center shadow-inner transition-all ${
                            gameState.enteredMasjid ? 'masjid-center-active' : ''
                        }`;
                        const answeredCount = gameState.answeredIds.length;
                        masjidCard.innerHTML = `
                            <div class="bg-amber-300 text-amber-950 font-display font-bold text-[10px] sm:text-xs px-3 py-0.5 rounded-full border border-amber-500 mb-1">
                                🕌 Destinasi Akhir (12/12 Soalan)
                            </div>
                            <div class="text-4xl sm:text-6xl my-1 animate-float">🕌</div>
                            <h4 class="font-display font-extrabold text-sm sm:text-xl text-emerald-900 leading-tight">
                                Masjid Al-Falah
                            </h4>
                            <p class="text-[10px] sm:text-xs font-bold text-slate-700 mt-1 max-w-xs">
                                ${
                                    gameState.enteredMasjid
                                        ? `${avatarEmoji} Alhamdulillah! Kamu telah selamat masuk ke Masjid Al-Falah!`
                                        : `Selesaikan <strong>${TOTAL_QUESTIONS - answeredCount} lagi soalan</strong> di laluan pusingan untuk masuk ke Masjid!`
                                }
                            </p>
                            <div class="mt-2 bg-white/90 border border-emerald-300 rounded-xl px-3 py-1 text-[10px] sm:text-xs font-extrabold text-emerald-800">
                                Azan ➔ Pergi ke masjid ➔ Iqamat ➔ Masuk saf untuk solat
                            </div>
                            ${
                                gameState.enteredMasjid
                                    ? `<div class="mt-2 text-3xl sm:text-4xl animate-bounce">${avatarEmoji}</div>`
                                    : ''
                            }
                        `;
                        grid.appendChild(masjidCard);
                        continue;
                    }

                    if (r >= 1 && r <= 4 && c >= 1 && c <= 4) {
                        continue;
                    }

                    const tileNum = coordToTile[`${r},${c}`];
                    const tile = boardTilesConfig[tileNum - 1];
                    const isCurrent = !gameState.enteredMasjid && gameState.position === tile.num;

                    let bgClass = 'bg-sky-50 border-sky-300 text-sky-950';
                    if (tile.type === 'START') bgClass = 'bg-emerald-100 border-emerald-500 text-emerald-950';
                    if (tile.type === 'BONUS') bgClass = 'bg-amber-100 border-amber-400 text-amber-950';
                    if (tile.type === 'REMINDER') bgClass = 'bg-pink-100 border-pink-400 text-pink-950';

                    const cell = document.createElement('div');
                    cell.id = `tile-${tile.num}`;
                    cell.className = `board-tile relative rounded-xl sm:rounded-2xl border-2 p-1 sm:p-2 flex flex-col items-center justify-between min-h-[58px] sm:min-h-[70px] text-center shadow-sm ${bgClass} ${
                        isCurrent ? 'board-tile-active' : ''
                    }`;

                    cell.innerHTML = `
                        <div class="w-full flex items-center justify-between text-[9px] sm:text-[11px] font-extrabold opacity-80">
                            <span>#${tile.num}</span>
                            <span>${arrowDirection[tile.num]}</span>
                        </div>
                        <div class="text-base sm:text-xl">${tile.icon}</div>
                        <div class="leading-tight">
                            <div class="font-display font-bold text-[9px] sm:text-[11px]">${tile.label}</div>
                            <div class="text-[8px] sm:text-[10px] font-semibold opacity-85 hidden sm:block">${tile.sub}</div>
                        </div>
                        ${
                            isCurrent
                                ? `<div id="player-token" class="animate-hop absolute -top-2.5 sm:-top-3.5 bg-white border-3 border-indigo-600 rounded-full w-8 h-8 sm:w-10 sm:h-10 flex items-center justify-center text-lg sm:text-2xl shadow-lg z-20">${avatarEmoji}</div>`
                                : ''
                        }
                    `;
                    grid.appendChild(cell);
                }
            }
        }

        function rollDice() {
            if (gameState.isMoving || gameState.isPaused || gameState.challengeOpen || gameState.enteredMasjid) return;

            gameState.isMoving = true;
            const rollBtn = document.getElementById('btn-roll-dice');
            const diceBox = document.getElementById('dice-box');
            rollBtn.disabled = true;
            diceBox.classList.add('animate-dice');

            document.getElementById('board-status-banner').textContent = '🎲 Dadu sedang bergolek secara rawak...';

            let rollTicks = 0;
            const diceFaces = ['1️⃣', '2️⃣', '3️⃣', '4️⃣', '5️⃣', '6️⃣'];
            const rollAnim = setInterval(() => {
                if (gameState.isPaused) return;
                playSound('dice');
                const preview = Math.floor(Math.random() * 6);
                diceBox.textContent = diceFaces[preview];
                rollTicks++;

                if (rollTicks >= 10) {
                    clearInterval(rollAnim);
                    diceBox.classList.remove('animate-dice');

                    // Hasil balingan dadu rawak sebenar 1 hingga 6
                    const steps = getTrueRandomDice();
                    diceBox.textContent = steps;
                    document.getElementById('board-status-banner').textContent = `Dadu keluar nombor ${steps}! Melangkah ${steps} petak...`;
                    movePlayerSteps(steps);
                }
            }, 80);
        }

        function movePlayerSteps(steps) {
            let remaining = steps;

            const stepInterval = setInterval(() => {
                if (gameState.isPaused) return;

                if (remaining > 0) {
                    gameState.position++;
                    if (gameState.position > TOTAL_TILES) {
                        gameState.position = 1;
                        gameState.laps++;
                    }
                    remaining--;
                    playSound('step');
                    updateHUD();
                    renderBoard();
                } else {
                    clearInterval(stepInterval);
                    handleTileArrival();
                }
            }, 310);
        }

        function handleTileArrival() {
            if (gameState.answeredIds.length >= TOTAL_QUESTIONS) {
                enterMasjidAndFinish();
                return;
            }

            const tile = boardTilesConfig[gameState.position - 1];

            // Petak Bonus: tambah +20 Markah dan terus paparkan 1 soalan tanpa ulangan
            if (tile.type === 'BONUS') {
                playSound('bonus');
                gameState.score += 20;
                updateHUD();
                openChallengeModal('⭐ Bonus +20 Markah! • Soalan Pengukuhan');
                return;
            }

            // Petak Peringatan Tertib: paparkan peringatan & soalan tanpa ulangan
            if (tile.type === 'REMINDER') {
                const rem = reminderEvents[Math.floor(Math.random() * reminderEvents.length)];
                openChallengeModal(`🌸 ${rem.title}`, rem.text);
                return;
            }

            // Petak Ilmu / Mula: paparkan soalan tanpa ulangan
            openChallengeModal();
        }

        function enterMasjidAndFinish() {
            gameState.enteredMasjid = true;
            gameState.isMoving = false;
            gameState.challengeOpen = false;
            document.getElementById('btn-roll-dice').disabled = true;
            document.getElementById('board-status-banner').textContent = '🎉 Semua 12 Soalan Selesai! Selamat Masuk ke Masjid Al-Falah!';
            updateHUD();
            renderBoard();

            setTimeout(() => {
                endGame();
            }, 1300);
        }

        function finishTurn() {
            gameState.isMoving = false;
            gameState.challengeOpen = false;
            document.getElementById('btn-roll-dice').disabled = false;
            const remaining = TOTAL_QUESTIONS - gameState.answeredIds.length;
            document.getElementById('board-status-banner').textContent = `Baki ${remaining} soalan lagi! Baling dadu untuk teruskan.`;
            document.getElementById('turn-instruction').textContent = `Kamu berada di Petak ${gameState.position} (Pusingan ${gameState.laps}). Baling dadu (1–6) untuk jawab soalan seterusnya!`;
        }

        /* =========================================================================
           5. SISTEM KOLAM 12 SOALAN TANPA ULANGAN & INTERAKSI JAWAPAN
           ========================================================================= */
        let currentChallenge = null;
        let challengeSolved = false;
        let orderSelected = [];

        function getNextUnansweredChallenge() {
            const unansweredPool = challengeBank.filter(q => !gameState.answeredIds.includes(q.id));
            if (unansweredPool.length === 0) return null;
            const randomIndex = Math.floor(Math.random() * unansweredPool.length);
            return unansweredPool[randomIndex];
        }

        function openChallengeModal(customBadgeTitle = null, reminderNote = null) {
            const nextQ = getNextUnansweredChallenge();
            if (!nextQ) {
                enterMasjidAndFinish();
                return;
            }

            gameState.challengeOpen = true;
            challengeSolved = false;
            currentChallenge = nextQ;

            const modal = document.getElementById('modal-challenge');
            const header = document.getElementById('chal-header');
            header.className = customBadgeTitle && customBadgeTitle.includes('Bonus')
                ? 'bg-amber-500 text-amber-950 px-5 py-3 flex items-center justify-between'
                : 'bg-sky-600 text-white px-5 py-3 flex items-center justify-between';

            document.getElementById('chal-type-icon').textContent = currentChallenge.mode === 'ORDER' ? '🔢' : currentChallenge.mode === 'FILL' ? '✍️' : '📘';
            document.getElementById('chal-type-badge').textContent = customBadgeTitle || currentChallenge.tag;
            document.getElementById('chal-title').textContent = currentChallenge.title;
            document.getElementById('chal-tile-num').textContent = `Soalan ${gameState.answeredIds.length + 1} / ${TOTAL_QUESTIONS}`;

            const questionText = reminderNote
                ? `💡 ${reminderNote}\n\n${currentChallenge.question}`
                : currentChallenge.question;
            document.getElementById('chal-question').innerText = questionText;

            const fb = document.getElementById('chal-feedback');
            fb.classList.add('hidden');

            const area = document.getElementById('chal-interactive-area');
            area.innerHTML = '';

            if (currentChallenge.mode === 'MCQ') {
                renderMCQChallenge(area, currentChallenge);
            } else if (currentChallenge.mode === 'ORDER') {
                renderOrderChallenge(area, currentChallenge);
            } else if (currentChallenge.mode === 'FILL') {
                renderFillChallenge(area, currentChallenge);
            }

            modal.classList.remove('hidden');
        }

        /* --- MOD A: SOALAN ANEKA PILIHAN (MCQ) --- */
        function renderMCQChallenge(container, chal) {
            const indexed = chal.options.map((text, idx) => ({ text, idx }));
            indexed.sort(() => Math.random() - 0.5);
            const letters = ['A', 'B', 'C', 'D'];

            indexed.forEach((opt, i) => {
                const btn = document.createElement('button');
                btn.className = 'btn-game w-full text-left bg-white hover:bg-sky-50 border-3 border-sky-300 rounded-2xl p-3.5 font-bold text-slate-800 text-xs sm:text-sm flex items-start gap-3 transition';
                btn.innerHTML = `
                    <span class="bg-sky-500 text-white font-display font-bold w-7 h-7 rounded-lg flex items-center justify-center shrink-0 mt-0.5">${letters[i]}</span>
                    <span>${opt.text}</span>
                `;
                btn.onclick = () => {
                    if (challengeSolved || gameState.isPaused) return;
                    challengeSolved = true;
                    const isCorrect = opt.idx === chal.correct;

                    const allBtns = container.querySelectorAll('button');
                    allBtns.forEach((b, bIdx) => {
                        b.disabled = true;
                        if (indexed[bIdx].idx === chal.correct) {
                            b.className = 'w-full text-left bg-emerald-100 border-3 border-emerald-600 rounded-2xl p-3.5 font-bold text-emerald-950 text-xs sm:text-sm flex items-start gap-3 ring-4 ring-emerald-200';
                        } else if (b === btn && !isCorrect) {
                            b.className = 'animate-shake w-full text-left bg-rose-100 border-3 border-rose-500 rounded-2xl p-3.5 font-bold text-rose-950 text-xs sm:text-sm flex items-start gap-3';
                        } else {
                            b.classList.add('opacity-50');
                        }
                    });

                    processChallengeResult(isCorrect, chal.explanation);
                };
                container.appendChild(btn);
            });
        }

        /* --- MOD B: CABARAN SUSUN URUTAN (1 - 2 - 3 - 4) --- */
        function renderOrderChallenge(container, chal) {
            orderSelected = [];
            const shuffled = [...chal.steps].sort(() => Math.random() - 0.5);

            const wrapper = document.createElement('div');
            wrapper.className = 'space-y-3';
            wrapper.innerHTML = `
                <div class="flex items-center justify-between">
                    <span class="text-xs font-extrabold uppercase text-slate-500">Urutan Pilihan Kamu (1 ➔ 4):</span>
                    <button id="btn-order-reset" class="text-xs font-bold bg-amber-100 hover:bg-amber-200 text-amber-900 px-3 py-1 rounded-lg border border-amber-400">
                        🔄 Set Semula
                    </button>
                </div>
                <div id="order-slots-box" class="space-y-2"></div>
                <div class="bg-slate-50 border-2 border-slate-200 rounded-2xl p-3">
                    <p class="text-xs font-bold text-slate-600 mb-2">Klik langkah mengikut urutan yang betul:</p>
                    <div id="order-pool-box" class="grid grid-cols-1 gap-2"></div>
                </div>
                <div class="flex justify-end">
                    <button id="btn-order-check" disabled class="btn-game disabled:opacity-40 bg-emerald-500 hover:bg-emerald-400 text-white font-display font-bold px-5 py-2.5 rounded-xl border-2 border-emerald-700 text-sm sm:text-base">
                        ✅ Semak Urutan!
                    </button>
                </div>
            `;
            container.appendChild(wrapper);

            const slotsBox = wrapper.querySelector('#order-slots-box');
            const poolBox = wrapper.querySelector('#order-pool-box');
            const checkBtn = wrapper.querySelector('#btn-order-check');
            const resetBtn = wrapper.querySelector('#btn-order-reset');

            function refreshOrderUI() {
                slotsBox.innerHTML = '';
                for (let i = 0; i < 4; i++) {
                    const item = orderSelected[i];
                    const row = document.createElement('div');
                    if (item) {
                        row.className = 'bg-emerald-50 border-2 border-emerald-400 rounded-xl px-3 py-2 flex items-center justify-between gap-2 text-xs sm:text-sm font-bold text-emerald-950';
                        row.innerHTML = `
                            <div class="flex items-center gap-2">
                                <span class="bg-emerald-600 text-white font-display w-6 h-6 rounded-full flex items-center justify-center shrink-0">${i + 1}</span>
                                <span>${item.icon} ${item.text}</span>
                            </div>
                            ${!challengeSolved ? `<button data-remove="${i}" class="text-xs bg-white text-rose-600 px-2 py-0.5 rounded border border-rose-300">Batal</button>` : ''}
                        `;
                    } else {
                        row.className = 'bg-slate-50 border-2 border-dashed border-slate-300 rounded-xl px-3 py-2 flex items-center gap-2 text-xs font-bold text-slate-400';
                        row.innerHTML = `
                            <span class="bg-slate-200 text-slate-500 font-display w-6 h-6 rounded-full flex items-center justify-center shrink-0">${i + 1}</span>
                            <span>Pilih langkah ke-${i + 1}...</span>
                        `;
                    }
                    slotsBox.appendChild(row);
                }

                slotsBox.querySelectorAll('[data-remove]').forEach(btn => {
                    btn.onclick = () => {
                        if (challengeSolved || gameState.isPaused) return;
                        playSound('click');
                        const idx = parseInt(btn.getAttribute('data-remove'), 10);
                        orderSelected.splice(idx, 1);
                        refreshOrderUI();
                    };
                });

                poolBox.innerHTML = '';
                shuffled.forEach(step => {
                    const used = orderSelected.some(s => s.id === step.id);
                    if (!used) {
                        const b = document.createElement('button');
                        b.className = 'btn-game text-left bg-white hover:bg-amber-50 border-2 border-sky-300 rounded-xl px-3.5 py-2 font-bold text-xs sm:text-sm text-slate-800 flex items-center gap-2';
                        b.innerHTML = `<span>${step.icon}</span> <span>${step.text}</span>`;
                        b.onclick = () => {
                            if (challengeSolved || gameState.isPaused || orderSelected.length >= 4) return;
                            playSound('step');
                            orderSelected.push(step);
                            refreshOrderUI();
                        };
                        poolBox.appendChild(b);
                    }
                });

                checkBtn.disabled = orderSelected.length !== 4 || challengeSolved;
            }

            resetBtn.onclick = () => {
                if (challengeSolved || gameState.isPaused) return;
                playSound('click');
                orderSelected = [];
                refreshOrderUI();
            };

            checkBtn.onclick = () => {
                if (challengeSolved || gameState.isPaused || orderSelected.length !== 4) return;
                challengeSolved = true;
                resetBtn.disabled = true;
                checkBtn.disabled = true;
                const isCorrect = orderSelected.every((s, idx) => s.id === idx + 1);
                refreshOrderUI();
                processChallengeResult(isCorrect, chal.explanation);
            };

            refreshOrderUI();
        }

        /* --- MOD C: CABARAN ISI TEMPAT KOSONG --- */
        function renderFillChallenge(container, chal) {
            const shuffledWords = [...chal.blanks].sort(() => Math.random() - 0.5);
            const grid = document.createElement('div');
            grid.className = 'grid grid-cols-2 gap-3';

            shuffledWords.forEach(word => {
                const btn = document.createElement('button');
                btn.className = 'btn-game bg-amber-50 hover:bg-amber-100 border-3 border-amber-400 rounded-2xl p-3.5 font-display font-bold text-base sm:text-lg text-amber-950 capitalize';
                btn.textContent = word;
                btn.onclick = () => {
                    if (challengeSolved || gameState.isPaused) return;
                    challengeSolved = true;
                    const isCorrect = word === chal.correctWord;

                    grid.querySelectorAll('button').forEach(b => {
                        b.disabled = true;
                        if (b.textContent === chal.correctWord) {
                            b.className = 'bg-emerald-100 border-3 border-emerald-600 rounded-2xl p-3.5 font-display font-bold text-base sm:text-lg text-emerald-950 capitalize ring-4 ring-emerald-200';
                        } else if (b === btn && !isCorrect) {
                            b.className = 'animate-shake bg-rose-100 border-3 border-rose-500 rounded-2xl p-3.5 font-display font-bold text-base sm:text-lg text-rose-950 capitalize';
                        } else {
                            b.classList.add('opacity-50');
                        }
                    });

                    processChallengeResult(isCorrect, chal.explanation);
                };
                grid.appendChild(btn);
            });

            container.appendChild(grid);
        }

        function processChallengeResult(isCorrect, explanation) {
            // Tandakan soalan ini sebagai SELESAI & buang daripada kolam pemilihan aktif
            if (currentChallenge && !gameState.answeredIds.includes(currentChallenge.id)) {
                gameState.answeredIds.push(currentChallenge.id);
            }

            const fb = document.getElementById('chal-feedback');
            const fbIcon = document.getElementById('chal-fb-icon');
            const fbTitle = document.getElementById('chal-fb-title');
            const fbDesc = document.getElementById('chal-fb-desc');
            const contBtn = document.getElementById('chal-continue-btn');

            if (isCorrect) {
                playSound('correct');
                gameState.score += 25;
                gameState.correctCount++;
                fb.className = 'rounded-2xl p-4 border-2 animate-pop bg-emerald-50 border-emerald-400 text-emerald-950';
                fbIcon.textContent = '🌟';
                fbTitle.textContent = `Jawapan Tepat! (+25 Markah) • Selesai ${gameState.answeredIds.length}/${TOTAL_QUESTIONS}`;
                fbDesc.textContent = explanation;
            } else {
                playSound('wrong');
                fb.className = 'rounded-2xl p-4 border-2 animate-pop bg-rose-50 border-rose-400 text-rose-950';
                fbIcon.textContent = '💡';
                fbTitle.textContent = `Mari Semak Jawapan! • Selesai ${gameState.answeredIds.length}/${TOTAL_QUESTIONS}`;
                fbDesc.textContent = explanation;
            }

            if (gameState.answeredIds.length >= TOTAL_QUESTIONS) {
                contBtn.textContent = 'Masuk ke Masjid Al-Falah! 🕌';
            } else {
                contBtn.textContent = 'Teruskan Perjalanan ➡️';
            }

            updateHUD();
            fb.classList.remove('hidden');
        }

        function closeChallengeModal() {
            playSound('click');
            document.getElementById('modal-challenge').classList.add('hidden');

            // Tamat permainan sebaik sahaja semua 12 soalan telah dijawab
            if (gameState.answeredIds.length >= TOTAL_QUESTIONS) {
                enterMasjidAndFinish();
            } else {
                finishTurn();
            }
        }

        /* =========================================================================
           6. TAMAT PERMAINAN, LENCANA GANJARAN & KONFETI KEMENANGAN
           ========================================================================= */
        function endGame() {
            gameState.isMoving = false;
            gameState.challengeOpen = false;
            showScreen('screen-end');

            const starsEl = document.getElementById('end-stars');
            const badgeIcon = document.getElementById('end-badge-icon');
            const badgeTitle = document.getElementById('end-badge-title');
            const heading = document.getElementById('end-heading');
            const message = document.getElementById('end-message');

            const accuracy = Math.round((gameState.correctCount / TOTAL_QUESTIONS) * 100);
            document.getElementById('end-score').textContent = `⭐ ${gameState.score}`;
            document.getElementById('end-accuracy').textContent = `${accuracy}% (${gameState.correctCount}/12)`;
            document.getElementById('end-laps').textContent = `${gameState.laps} Pusingan`;

            playSound('win');
            startConfetti();

            if (accuracy >= 80) {
                starsEl.textContent = '⭐⭐⭐';
                badgeTitle.textContent = 'Lencana: Duta Tertib Ibadah 🌟';
            } else if (accuracy >= 50) {
                starsEl.textContent = '⭐⭐☆';
                badgeTitle.textContent = 'Lencana: Sahabat Masjid Gigih 🕌';
            } else {
                starsEl.textContent = '⭐☆☆';
                badgeTitle.textContent = 'Lencana: Pengembara Ilmu Solat 📖';
            }

            badgeIcon.textContent = '🕌';
            heading.textContent = 'Tahniah! Semua 12 Soalan Selesai!';
            heading.className = 'font-display font-extrabold text-2xl sm:text-4xl text-emerald-700 mb-2';
            message.textContent = `Alhamdulillah, ${gameState.avatar === 'boy' ? 'Ahmad' : 'Aisyah'} telah menyelesaikan kesemua 12 soalan pengukuhan dan selamat masuk ke Masjid Al-Falah!`;
        }

        let confettiAnim = null;
        function startConfetti() {
            const cCanvas = document.getElementById('confetti-canvas');
            cCanvas.classList.remove('hidden');
            cCanvas.width = window.innerWidth;
            cCanvas.height = window.innerHeight;
            const cCtx = cCanvas.getContext('2d');

            const colors = ['#22c55e', '#ec4899', '#eab308', '#3b82f6', '#f97316'];
            const pieces = Array.from({ length: 90 }, () => ({
                x: Math.random() * cCanvas.width,
                y: Math.random() * cCanvas.height - cCanvas.height,
                r: Math.random() * 7 + 4,
                color: colors[Math.floor(Math.random() * colors.length)],
                vy: Math.random() * 3 + 2,
                vx: Math.random() * 2 - 1,
                rot: Math.random() * 360
            }));

            function renderConfetti() {
                cCtx.clearRect(0, 0, cCanvas.width, cCanvas.height);
                pieces.forEach(p => {
                    p.y += p.vy;
                    p.x += p.vx;
                    p.rot += 4;
                    if (p.y > cCanvas.height) p.y = -10;

                    cCtx.save();
                    cCtx.translate(p.x, p.y);
                    cCtx.rotate((p.rot * Math.PI) / 180);
                    cCtx.fillStyle = p.color;
                    cCtx.fillRect(-p.r / 2, -p.r / 2, p.r, p.r * 0.6);
                    cCtx.restore();
                });
                confettiAnim = requestAnimationFrame(renderConfetti);
            }
            renderConfetti();
        }

        function stopConfetti() {
            if (confettiAnim) {
                cancelAnimationFrame(confettiAnim);
                confettiAnim = null;
            }
            const cCanvas = document.getElementById('confetti-canvas');
            if (cCanvas) cCanvas.classList.add('hidden');
        }
    </script>
</body>
</html>
