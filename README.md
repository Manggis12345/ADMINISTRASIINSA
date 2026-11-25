[ADMINISTRASI INSA.html](https://github.com/user-attachments/files/35410362/ADMINISTRASI.INSA.html)
# ADMINISTRASIINSA
WKWKWK
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Administrasi Persuratan Terpadu INSA - Institut Sabilillah Lamongan</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        insa: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                            accent: '#0284c7'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>

    <style>
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: #fff; color: #000; font-size: 12pt; }
        }
        .print-only { display: none; }
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="bg-slate-50 font-sans text-slate-800 antialiased min-h-screen flex flex-col">

    <header class="bg-insa-900 text-white shadow-md sticky top-0 z-30 no-print">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 rounded-full bg-white flex items-center justify-center text-insa-900 font-bold text-xl shadow">
                        <i class="fas fa-graduation-cap"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-base sm:text-lg leading-tight tracking-wide">E-Surat Terpadu INSA</h1>
                        <p class="text-xs text-emerald-200">Institut Sabilillah Lamongan</p>
                    </div>
                </div>

                <div class="flex items-center space-x-3">
                    <span id="gasStatusBadge" class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-amber-100 text-amber-800 border border-amber-200">
                        <span class="w-2 h-2 mr-1.5 bg-amber-500 rounded-full animate-pulse"></span> Mode Penyiapan Lokal
                    </span>
                    <button onclick="openGasModal()" class="bg-insa-700 hover:bg-insa-800 text-white text-xs px-3 py-2 rounded-lg font-medium transition flex items-center gap-1.5 shadow-sm border border-emerald-600">
                        <i class="fas fa-plug"></i> <span class="hidden sm:inline">Integrasi Google Sheets</span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <div class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 flex flex-col lg:flex-row gap-6">

        <!-- Sidebar Navigation -->
        <aside class="w-full lg:w-72 flex-shrink-0 no-print">
            <div class="bg-white rounded-xl shadow-sm border border-slate-200 p-4 sticky top-20">
                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider mb-2 px-2">Menu Utama</p>
                <nav class="space-y-1">
                    <button onclick="switchTab('dashboard')" id="nav-dashboard" class="nav-btn active w-full flex items-center space-x-3 px-3 py-2.5 rounded-lg text-xs font-semibold transition bg-insa-50 text-insa-800">
                        <i class="fas fa-chart-pie w-5 text-center text-insa-700 text-sm"></i>
                        <span>Dashboard Utama</span>
                    </button>
                    <button onclick="switchTab('surat-masuk')" id="nav-surat-masuk" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-lg text-xs font-medium transition text-slate-600 hover:bg-slate-100">
                        <i class="fas fa-inbox w-5 text-center text-blue-600 text-sm"></i>
                        <span>Surat Masuk & Disposisi</span>
                        <span id="badge-surat-masuk" class="ml-auto bg-blue-100 text-blue-800 text-[10px] px-2 py-0.5 rounded-full font-bold">0</span>
                    </button>
                    <button onclick="switchTab('surat-keluar')" id="nav-surat-keluar" class="nav-btn w-full flex items-center space-x-3 px-3 py-2.5 rounded-lg text-xs font-medium transition text-slate-600 hover:bg-slate-100">
                        <i class="fas fa-paper-plane w-5 text-center text-emerald-600 text-sm"></i>
                        <span>Register Semua Surat Keluar</span>
                        <span id="badge-surat-keluar" class="ml-auto bg-emerald-100 text-emerald-800 text-[10px] px-2 py-0.5 rounded-full font-bold">0</span>
                    </button>
                </nav>

                <hr class="my-4 border-slate-200">

                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider mb-2 px-2">Kategori Khusus Surat Keluar</p>
                <div class="space-y-1 max-h-64 overflow-y-auto pr-1">
                    <button onclick="filterByJenis('SK-YYS')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-landmark text-amber-600 mr-2 w-4"></i> SK Yayasan</span>
                        <span id="cat-count-SK-YYS" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('SK-REK')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-file-contract text-blue-600 mr-2 w-4"></i> SK Rektor</span>
                        <span id="cat-count-SK-REK" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('UND')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-envelope-open-text text-indigo-600 mr-2 w-4"></i> Surat Undangan</span>
                        <span id="cat-count-UND" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('STG')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-user-check text-emerald-600 mr-2 w-4"></i> Surat Tugas</span>
                        <span id="cat-count-STG" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('SPD')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-plane-departure text-purple-600 mr-2 w-4"></i> Perjalanan Dinas (SPPD)</span>
                        <span id="cat-count-SPD" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('BAC')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-clipboard-list text-rose-600 mr-2 w-4"></i> Berita Acara (BAP)</span>
                        <span id="cat-count-BAC" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                    <button onclick="filterByJenis('PJJ')" class="w-full text-left px-3 py-1.5 rounded text-xs text-slate-600 hover:bg-slate-100 flex items-center justify-between">
                        <span><i class="fas fa-handshake text-teal-600 mr-2 w-4"></i> Perjanjian / MoU</span>
                        <span id="cat-count-PJJ" class="text-[10px] font-bold text-slate-400">0</span>
                    </button>
                </div>

                <hr class="my-4 border-slate-200">

                <p class="text-[11px] font-bold text-slate-400 uppercase tracking-wider mb-2 px-2">Alat & Utilitas</p>
                <nav class="space-y-1">
                    <button onclick="openGasModal()" class="w-full flex items-center space-x-3 px-3 py-2 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-100 transition">
                        <i class="fas fa-code w-5 text-center text-slate-500"></i>
                        <span>Script Google Sheets</span>
                    </button>
                    <button onclick="exportDataToCSV()" class="w-full flex items-center space-x-3 px-3 py-2 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-100 transition">
                        <i class="fas fa-file-excel w-5 text-center text-emerald-600"></i>
                        <span>Ekspor Excel/CSV</span>
                    </button>
                    <button onclick="resetToDemoData()" class="w-full flex items-center space-x-3 px-3 py-2 rounded-lg text-xs font-medium text-amber-700 hover:bg-amber-50 transition">
                        <i class="fas fa-rotate w-5 text-center text-amber-600"></i>
                        <span>Reset Data Sampel</span>
                    </button>
                </nav>
            </div>
        </aside>

        <main class="flex-1 no-print min-w-0">

            <!-- TAB 1: DASHBOARD -->
            <section id="tab-dashboard" class="tab-content space-y-6">
                <div class="bg-gradient-to-r from-insa-900 via-insa-800 to-emerald-800 text-white p-6 rounded-2xl shadow-sm relative overflow-hidden">
                    <div class="relative z-10">
                        <span class="inline-block px-3 py-1 bg-emerald-500/30 text-emerald-100 rounded-full text-xs font-semibold mb-2 backdrop-blur-sm">Sistem Sekretariat & Tata Usaha</span>
                        <h2 class="text-2xl font-bold mb-1">Administrasi Persuratan Lengkap INSA</h2>
                        <p class="text-emerald-100 text-xs sm:text-sm max-w-2xl leading-relaxed">
                            Pencatatan 16+ jenis naskah dinas, penomoran otomatis instansi, lembar disposisi rektorat, cetak surat tugas/SPPD, dan penyimpanan arsip cloud Google Spreadsheet.
                        </p>
                    </div>
                    <i class="fas fa-graduation-cap absolute -right-4 -bottom-8 text-9xl text-white opacity-10 pointer-events-none"></i>
                </div>

                <!-- Stats Overview -->
                <div class="grid grid-cols-2 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                        <p class="text-[11px] font-bold uppercase tracking-wider text-slate-400">Total Surat Masuk</p>
                        <h3 id="stat-total-masuk" class="text-2xl font-bold text-slate-800 mt-1">0</h3>
                        <p class="text-[11px] text-blue-600 mt-1 font-medium"><i class="fas fa-inbox"></i> Ter-agenda di TU</p>
                    </div>
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                        <p class="text-[11px] font-bold uppercase tracking-wider text-slate-400">Total Surat Keluar</p>
                        <h3 id="stat-total-keluar" class="text-2xl font-bold text-slate-800 mt-1">0</h3>
                        <p class="text-[11px] text-emerald-600 mt-1 font-medium"><i class="fas fa-paper-plane"></i> Ter-terbitkan</p>
                    </div>
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                        <p class="text-[11px] font-bold uppercase tracking-wider text-slate-400">Butuh Disposisi</p>
                        <h3 id="stat-pending-disposisi" class="text-2xl font-bold text-amber-600 mt-1">0</h3>
                        <p class="text-[11px] text-amber-600 mt-1 font-medium"><i class="fas fa-clock"></i> Menunggu Rektor</p>
                    </div>
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm">
                        <p class="text-[11px] font-bold uppercase tracking-wider text-slate-400">SK & Surat Tugas</p>
                        <h3 id="stat-sk-tugas" class="text-2xl font-bold text-indigo-600 mt-1">0</h3>
                        <p class="text-[11px] text-indigo-600 mt-1 font-medium"><i class="fas fa-award"></i> Dokumen Resmi</p>
                    </div>
                </div>

                <!-- Quick Action Buttons -->
                <div class="bg-white p-5 rounded-xl border border-slate-200 shadow-sm">
                    <h3 class="text-xs font-bold text-slate-700 uppercase tracking-wider mb-3">Aksi Cepat Penomoran Surat Baru</h3>
                    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-2">
                        <button onclick="openFormKeluarModal('SK-REK')" class="p-2.5 bg-blue-50 hover:bg-blue-100 text-blue-700 rounded-lg text-xs font-medium text-left border border-blue-200 transition">
                            <i class="fas fa-file-contract block text-base mb-1"></i> SK Rektor
                        </button>
                        <button onclick="openFormKeluarModal('STG')" class="p-2.5 bg-emerald-50 hover:bg-emerald-100 text-emerald-700 rounded-lg text-xs font-medium text-left border border-emerald-200 transition">
                            <i class="fas fa-user-check block text-base mb-1"></i> Surat Tugas
                        </button>
                        <button onclick="openFormKeluarModal('UND')" class="p-2.5 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 rounded-lg text-xs font-medium text-left border border-indigo-200 transition">
                            <i class="fas fa-envelope-open-text block text-base mb-1"></i> Undangan
                        </button>
                        <button onclick="openFormKeluarModal('SPD')" class="p-2.5 bg-purple-50 hover:bg-purple-100 text-purple-700 rounded-lg text-xs font-medium text-left border border-purple-200 transition">
                            <i class="fas fa-plane-departure block text-base mb-1"></i> SPPD / Dinas
                        </button>
                        <button onclick="openFormKeluarModal('SKT')" class="p-2.5 bg-teal-50 hover:bg-teal-100 text-teal-700 rounded-lg text-xs font-medium text-left border border-teal-200 transition">
                            <i class="fas fa-id-card block text-base mb-1"></i> Keterangan
                        </button>
                        <button onclick="openFormKeluarModal('BAC')" class="p-2.5 bg-rose-50 hover:bg-rose-100 text-rose-700 rounded-lg text-xs font-medium text-left border border-rose-200 transition">
                            <i class="fas fa-clipboard-list block text-base mb-1"></i> Berita Acara
                        </button>
                    </div>
                </div>

                <!-- Recent Activities Table -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm p-4">
                        <div class="flex items-center justify-between mb-3">
                            <h3 class="font-bold text-slate-800 text-xs flex items-center gap-2">
                                <i class="fas fa-inbox text-blue-600"></i> Agenda Surat Masuk Terkini
                            </h3>
                            <button onclick="switchTab('surat-masuk')" class="text-[11px] text-insa-700 font-semibold hover:underline">Lihat Semua</button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-[11px] text-left">
                                <thead class="bg-slate-50 text-slate-500 uppercase font-semibold">
                                    <tr>
                                        <th class="p-2">No. Agenda</th>
                                        <th class="p-2">Pengirim</th>
                                        <th class="p-2">Perihal</th>
                                        <th class="p-2">Status</th>
                                    </tr>
                                </thead>
                                <tbody id="recent-masuk-table" class="divide-y divide-slate-100"></tbody>
                            </table>
                        </div>
                    </div>

                    <div class="bg-white rounded-xl border border-slate-200 shadow-sm p-4">
                        <div class="flex items-center justify-between mb-3">
                            <h3 class="font-bold text-slate-800 text-xs flex items-center gap-2">
                                <i class="fas fa-paper-plane text-emerald-600"></i> Register Surat Keluar Terkini
                            </h3>
                            <button onclick="switchTab('surat-keluar')" class="text-[11px] text-insa-700 font-semibold hover:underline">Lihat Semua</button>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-[11px] text-left">
                                <thead class="bg-slate-50 text-slate-500 uppercase font-semibold">
                                    <tr>
                                        <th class="p-2">No. Surat</th>
                                        <th class="p-2">Jenis</th>
                                        <th class="p-2">Tujuan</th>
                                        <th class="p-2">Tanggal</th>
                                    </tr>
                                </thead>
                                <tbody id="recent-keluar-table" class="divide-y divide-slate-100"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </section>

            <!-- TAB 2: SURAT MASUK -->
            <section id="tab-surat-masuk" class="tab-content hidden space-y-4">
                <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                    <div>
                        <h2 class="text-base font-bold text-slate-800 flex items-center gap-2">
                            <i class="fas fa-inbox text-blue-600"></i> Agenda & Disposisi Surat Masuk
                        </h2>
                        <p class="text-xs text-slate-500">Registrasi surat masuk dari luar instansi dan cetak lembar disposisi pimpinan</p>
                    </div>
                    <button onclick="openFormMasukModal()" class="w-full sm:w-auto bg-insa-700 hover:bg-insa-800 text-white px-4 py-2 rounded-lg text-xs font-semibold transition flex items-center justify-center gap-2 shadow">
                        <i class="fas fa-plus"></i> Register Surat Masuk
                    </button>
                </div>

                <div class="bg-white p-3 rounded-xl border border-slate-200 shadow-sm flex flex-col md:flex-row gap-3 items-center justify-between">
                    <div class="relative w-full md:w-80">
                        <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                        <input type="text" id="search-masuk" oninput="filterSuratMasuk()" placeholder="Cari no agenda, pengirim, perihal..." class="w-full pl-9 pr-3 py-1.5 border border-slate-200 rounded-lg text-xs focus:ring-2 focus:ring-insa-600 focus:outline-none">
                    </div>
                    <div class="flex flex-wrap gap-2 w-full md:w-auto">
                        <select id="filter-urgensi-masuk" onchange="filterSuratMasuk()" class="border border-slate-200 rounded-lg text-xs px-3 py-1.5 bg-white focus:ring-2 focus:ring-insa-600 focus:outline-none">
                            <option value="">-- Semua Urgensi --</option>
                            <option value="Sangat Penting">Sangat Penting</option>
                            <option value="Penting">Penting</option>
                            <option value="Biasa">Biasa</option>
                        </select>
                        <select id="filter-status-masuk" onchange="filterSuratMasuk()" class="border border-slate-200 rounded-lg text-xs px-3 py-1.5 bg-white focus:ring-2 focus:ring-insa-600 focus:outline-none">
                            <option value="">-- Semua Status --</option>
                            <option value="Belum Disposisi">Belum Disposisi</option>
                            <option value="Proses Disposisi">Proses Disposisi</option>
                            <option value="Selesai">Selesai</option>
                        </select>
                    </div>
                </div>

                <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs text-slate-700">
                            <thead class="bg-slate-100 text-slate-600 uppercase font-bold tracking-wider border-b border-slate-200">
                                <tr>
                                    <th class="p-3">No. Agenda / Terima</th>
                                    <th class="p-3">Pengirim & No. Surat</th>
                                    <th class="p-3">Perihal</th>
                                    <th class="p-3">Urgensi</th>
                                    <th class="p-3">Status Disposisi</th>
                                    <th class="p-3 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="table-surat-masuk-body" class="divide-y divide-slate-200"></tbody>
                        </table>
                    </div>
                    <div id="empty-masuk" class="p-8 text-center text-slate-400 hidden">
                        <i class="fas fa-folder-open text-4xl mb-2 text-slate-300"></i>
                        <p class="text-sm">Tidak ditemukan data surat masuk.</p>
                    </div>
                </div>
            </section>

            <!-- TAB 3: SURAT KELUAR (16+ CATEGORIES) -->
            <section id="tab-surat-keluar" class="tab-content hidden space-y-4">
                <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                    <div>
                        <h2 class="text-base font-bold text-slate-800 flex items-center gap-2">
                            <i class="fas fa-paper-plane text-emerald-600"></i> Register Persuratan Keluar INSA
                        </h2>
                        <p class="text-xs text-slate-500">Penomoran otomatis untuk SK, Undangan, Surat Tugas, SPPD, MoU, dan Surat Resmi Lainnya</p>
                    </div>
                    <button onclick="openFormKeluarModal()" class="w-full sm:w-auto bg-emerald-700 hover:bg-emerald-800 text-white px-4 py-2 rounded-lg text-xs font-semibold transition flex items-center justify-center gap-2 shadow">
                        <i class="fas fa-plus"></i> Buat Surat Keluar Baru
                    </button>
                </div>

                <div class="bg-white p-3 rounded-xl border border-slate-200 shadow-sm flex flex-col md:flex-row gap-3 items-center justify-between">
                    <div class="relative w-full md:w-80">
                        <i class="fas fa-search absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                        <input type="text" id="search-keluar" oninput="filterSuratKeluar()" placeholder="Cari nomor surat, penerima, perihal..." class="w-full pl-9 pr-3 py-1.5 border border-slate-200 rounded-lg text-xs focus:ring-2 focus:ring-emerald-600 focus:outline-none">
                    </div>
                    <div>
                        <select id="filter-jenis-keluar" onchange="filterSuratKeluar()" class="border border-slate-200 rounded-lg text-xs px-3 py-1.5 bg-white focus:ring-2 focus:ring-emerald-600 focus:outline-none">
                            <option value="">-- Semua Jenis Surat --</option>
                            <option value="SK-YYS">Surat Keputusan Yayasan</option>
                            <option value="SK-REK">Surat Keputusan Rektor</option>
                            <option value="UND">Surat Undangan</option>
                            <option value="PRM">Surat Permohonan</option>
                            <option value="PBT">Surat Pemberitahuan</option>
                            <option value="PJM">Surat Peminjaman Sarpras</option>
                            <option value="PYT">Surat Pernyataan</option>
                            <option value="MND">Surat Mandat</option>
                            <option value="STG">Surat Tugas</option>
                            <option value="SKT">Surat Keterangan</option>
                            <option value="RKM">Surat Rekomendasi</option>
                            <option value="BLS">Surat Balasan</option>
                            <option value="SPD">Surat Perjalanan Dinas (SPPD)</option>
                            <option value="PJJ">Surat Perjanjian (MoU/MoA)</option>
                            <option value="PNG">Surat Pengantar</option>
                            <option value="BAC">Surat Berita Acara</option>
                        </select>
                    </div>
                </div>

                <div class="bg-white rounded-xl border border-slate-200 shadow-sm overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs text-slate-700">
                            <thead class="bg-slate-100 text-slate-600 uppercase font-bold tracking-wider border-b border-slate-200">
                                <tr>
                                    <th class="p-3">No. Surat & Tgl</th>
                                    <th class="p-3">Jenis Surat</th>
                                    <th class="p-3">Tujuan / Penerima</th>
                                    <th class="p-3">Perihal</th>
                                    <th class="p-3">Penandatangan</th>
                                    <th class="p-3 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="table-surat-keluar-body" class="divide-y divide-slate-200"></tbody>
                        </table>
                    </div>
                    <div id="empty-keluar" class="p-8 text-center text-slate-400 hidden">
                        <i class="fas fa-folder-open text-4xl mb-2 text-slate-300"></i>
                        <p class="text-sm">Tidak ditemukan data surat keluar.</p>
                    </div>
                </div>
            </section>
        </main>
    </div>

    <!-- PRINT ONLY SECTION 1: LEMBAR DISPOSISI -->
    <div id="print-disposisi-area" class="print-only p-8 max-w-3xl mx-auto border-2 border-black bg-white text-black text-xs">
        <div class="flex items-center border-b-4 border-double border-black pb-3 mb-4">
            <div class="w-16 text-center font-bold text-3xl">
                <i class="fas fa-university"></i>
            </div>
            <div class="flex-1 text-center">
                <h2 class="text-base font-bold uppercase tracking-wider">INSTITUT SABILILLAH LAMONGAN (INSA)</h2>
                <p class="text-[11px]">Jl. Raya Sabilillah No.01 Lamongan, Jawa Timur</p>
                <p class="text-[9px]">Email: sekretariat@insa.ac.id | Website: https://insa.ac.id</p>
            </div>
        </div>

        <div class="text-center font-bold text-sm underline mb-4 uppercase tracking-widest">
            LEMBAR DISPOSISI REKTOR / PIMPINAN
        </div>

        <table class="w-full border-collapse border border-black text-xs mb-4">
            <tr>
                <td class="border border-black p-1.5 font-semibold bg-gray-100 w-28">Surat Dari</td>
                <td id="disp-pengirim" class="border border-black p-1.5">...</td>
                <td class="border border-black p-1.5 font-semibold bg-gray-100 w-28">No. Agenda</td>
                <td id="disp-no-agenda" class="border border-black p-1.5 font-bold">...</td>
            </tr>
            <tr>
                <td class="border border-black p-1.5 font-semibold bg-gray-100">No. Surat</td>
                <td id="disp-no-surat" class="border border-black p-1.5">...</td>
                <td class="border border-black p-1.5 font-semibold bg-gray-100">Tgl. Diterima</td>
                <td id="disp-tgl-terima" class="border border-black p-1.5">...</td>
            </tr>
            <tr>
                <td class="border border-black p-1.5 font-semibold bg-gray-100">Tgl. Surat</td>
                <td id="disp-tgl-surat" class="border border-black p-1.5">...</td>
                <td class="border border-black p-1.5 font-semibold bg-gray-100">Urgensi</td>
                <td id="disp-urgensi" class="border border-black p-1.5 font-bold">...</td>
            </tr>
            <tr>
                <td class="border border-black p-1.5 font-semibold bg-gray-100">Perihal</td>
                <td id="disp-perihal" colspan="3" class="border border-black p-1.5">...</td>
            </tr>
        </table>

        <div class="grid grid-cols-2 gap-3 border border-black p-3 mb-4 text-[11px]">
            <div>
                <p class="font-bold border-b border-black pb-1 mb-2">DITERUSKAN KEPADA YTH:</p>
                <div class="space-y-1">
                    <label class="block"><input type="checkbox"> Wakil Rektor I (Akademik)</label>
                    <label class="block"><input type="checkbox"> Wakil Rektor II (Keuangan/SDM)</label>
                    <label class="block"><input type="checkbox"> Wakil Rektor III (Kemahasiswaan)</label>
                    <label class="block"><input type="checkbox"> Dekan Fakultas</label>
                    <label class="block"><input type="checkbox"> LPPM / LPM</label>
                    <label class="block"><input type="checkbox"> Kepala Bagian TU</label>
                </div>
            </div>
            <div>
                <p class="font-bold border-b border-black pb-1 mb-2">PETUNJUK / DISPOSISI:</p>
                <div class="space-y-1">
                    <label class="block"><input type="checkbox"> Setuju / Selesaikan</label>
                    <label class="block"><input type="checkbox"> Proses / Tindak Lanjuti</label>
                    <label class="block"><input type="checkbox"> Pelajari / Buatkan Konsep</label>
                    <label class="block"><input type="checkbox"> Koordinasikan / Bicarakan</label>
                    <label class="block"><input type="checkbox"> Hadiri / Mewakili</label>
                    <label class="block"><input type="checkbox"> Arsip / File</label>
                </div>
            </div>
        </div>

        <div class="border border-black p-3 mb-6 min-h-[100px]">
            <p class="font-bold text-xs mb-1">CATATAN / INSTRUKSI REKTOR:</p>
            <p id="disp-catatan" class="italic text-gray-800">...</p>
        </div>

        <div class="flex justify-end text-center">
            <div class="w-56">
                <p>Lamongan, <span id="disp-tgl-ttd">...</span></p>
                <p class="font-bold mt-1">Rektor INSA</p>
                <div class="h-16"></div>
                <p class="font-bold underline">( Dr. H. Administrator, M.Pd.I )</p>
            </div>
        </div>
    </div>

    <!-- MODAL 1: FORM SURAT MASUK -->
    <div id="modal-surat-masuk" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4 no-print">
        <div class="bg-white rounded-2xl shadow-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto border border-slate-200">
            <div class="p-4 border-b border-slate-100 flex items-center justify-between bg-slate-50 rounded-t-2xl">
                <h3 id="modal-masuk-title" class="font-bold text-slate-800 text-sm flex items-center gap-2">
                    <i class="fas fa-inbox text-blue-600"></i> Register Surat Masuk
                </h3>
                <button onclick="closeFormMasukModal()" class="text-slate-400 hover:text-slate-600"><i class="fas fa-times"></i></button>
            </div>
            <form id="form-masuk" onsubmit="saveSuratMasuk(event)" class="p-5 space-y-3">
                <input type="hidden" id="masuk-id">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">No. Agenda Masuk *</label>
                        <input type="text" id="masuk-no-agenda" required placeholder="Contoh: SM-2026-001" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Tanggal Diterima *</label>
                        <input type="date" id="masuk-tgl-terima" required class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Pengirim (Instansi/Orang) *</label>
                        <input type="text" id="masuk-pengirim" required placeholder="Kemdikbud / LLDIKTI Region VII" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Nomor Surat Asli *</label>
                        <input type="text" id="masuk-no-surat" required placeholder="Contoh: 102/LL7/TU/2026" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Tanggal Surat Asli *</label>
                        <input type="date" id="masuk-tgl-surat" required class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Tingkat Urgensi *</label>
                        <select id="masuk-urgensi" required class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                            <option value="Biasa">Biasa</option>
                            <option value="Penting">Penting</option>
                            <option value="Sangat Penting">Sangat Penting</option>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Perihal / Ringkasan Isi Surat *</label>
                    <textarea id="masuk-perihal" rows="2" required placeholder="Tuliskan perihal ringkas..." class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none"></textarea>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Status Disposisi</label>
                        <select id="masuk-status" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                            <option value="Belum Disposisi">Belum Disposisi</option>
                            <option value="Proses Disposisi">Proses Disposisi</option>
                            <option value="Selesai">Selesai / Diarsipkan</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Link File Digital (Drive)</label>
                        <input type="url" id="masuk-file-link" placeholder="https://drive.google.com/..." class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Catatan Instruksi Rektor</label>
                    <textarea id="masuk-catatan-disposisi" rows="2" placeholder="Instruksi pimpinan..." class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-blue-500 focus:outline-none"></textarea>
                </div>

                <div class="pt-3 border-t border-slate-100 flex justify-end gap-2">
                    <button type="button" onclick="closeFormMasukModal()" class="px-3 py-1.5 border border-slate-300 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-50">Batal</button>
                    <button type="submit" class="px-4 py-1.5 bg-blue-600 hover:bg-blue-700 text-white rounded-lg text-xs font-semibold shadow">Simpan Data Surat</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL 2: FORM SURAT KELUAR (16 CATEGORIES) -->
    <div id="modal-surat-keluar" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4 no-print">
        <div class="bg-white rounded-2xl shadow-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto border border-slate-200">
            <div class="p-4 border-b border-slate-100 flex items-center justify-between bg-slate-50 rounded-t-2xl">
                <h3 id="modal-keluar-title" class="font-bold text-slate-800 text-sm flex items-center gap-2">
                    <i class="fas fa-paper-plane text-emerald-600"></i> Penomoran & Register Surat Keluar
                </h3>
                <button onclick="closeFormKeluarModal()" class="text-slate-400 hover:text-slate-600"><i class="fas fa-times"></i></button>
            </div>
            <form id="form-keluar" onsubmit="saveSuratKeluar(event)" class="p-5 space-y-3">
                <input type="hidden" id="keluar-id">
                
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Jenis / Kategori Surat Dinas *</label>
                    <select id="keluar-jenis" onchange="onJenisSuratChange()" required class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs font-medium focus:ring-2 focus:ring-emerald-500 focus:outline-none bg-emerald-50/50">
                        <option value="SK-YYS">1. Surat Keputusan Yayasan (SK-YYS)</option>
                        <option value="SK-REK">2. Surat Keputusan Rektor (SK-REK)</option>
                        <option value="UND">3. Surat Undangan (UND)</option>
                        <option value="PRM">4. Surat Permohonan (PRM)</option>
                        <option value="PBT">5. Surat Pemberitahuan (PBT)</option>
                        <option value="PJM">6. Surat Peminjaman Sarpras (PJM)</option>
                        <option value="PYT">7. Surat Pernyataan (PYT)</option>
                        <option value="MND">8. Surat Mandat (MND)</option>
                        <option value="STG">9. Surat Tugas (STG)</option>
                        <option value="SKT">10. Surat Keterangan (SKT)</option>
                        <option value="RKM">11. Surat Rekomendasi (RKM)</option>
                        <option value="BLS">12. Surat Balasan (BLS)</option>
                        <option value="SPD">13. Surat Perjalanan Dinas / SPPD (SPD)</option>
                        <option value="PJJ">14. Surat Perjanjian / MoU / MoA (PJJ)</option>
                        <option value="PNG">15. Surat Pengantar (PNG)</option>
                        <option value="BAC">16. Surat Berita Acara / BAP (BAC)</option>
                    </select>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Nomor Surat Otomatis *</label>
                        <input type="text" id="keluar-no-surat" required placeholder="Contoh: 012/INSA/STG/X/2026" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none font-mono font-bold text-emerald-800">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Tanggal Terbit Surat *</label>
                        <input type="date" id="keluar-tgl-surat" required class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Tujuan / Penerima *</label>
                        <input type="text" id="keluar-penerima" required placeholder="Contoh: Dekan Fakultas Tarbiyah / Instansi Luar" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1">Penandatangan Surat *</label>
                        <select id="keluar-penandatangan" class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                            <option value="Rektor INSA">Rektor INSA</option>
                            <option value="Ketua Yayasan">Ketua Yayasan</option>
                            <option value="Wakil Rektor I">Wakil Rektor I (Akademik)</option>
                            <option value="Wakil Rektor II">Wakil Rektor II (Keuangan/SDM)</option>
                            <option value="Wakil Rektor III">Wakil Rektor III (Kemahasiswaan)</option>
                            <option value="Dekan Fakultas">Dekan Fakultas</option>
                            <option value="Kepala Bagian TU">Kepala Bagian TU</option>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Perihal / Isian Ringkas Surat *</label>
                    <textarea id="keluar-perihal" rows="2" required placeholder="Tuliskan perihal atau judul keputusan..." class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none"></textarea>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1">Link File Berkas (Google Drive / PDF)</label>
                    <input type="url" id="keluar-file-link" placeholder="https://drive.google.com/..." class="w-full px-3 py-1.5 border border-slate-300 rounded-lg text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                </div>

                <div class="pt-3 border-t border-slate-100 flex justify-end gap-2">
                    <button type="button" onclick="closeFormKeluarModal()" class="px-3 py-1.5 border border-slate-300 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-50">Batal</button>
                    <button type="submit" class="px-4 py-1.5 bg-emerald-600 hover:bg-emerald-700 text-white rounded-lg text-xs font-semibold shadow">Simpan & Terbit Nomor</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL 3: INTEGRASI GOOGLE APPS SCRIPT -->
    <div id="modal-gas" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center hidden p-4 no-print">
        <div class="bg-white rounded-2xl shadow-xl max-w-3xl w-full max-h-[90vh] overflow-y-auto border border-slate-200">
            <div class="p-4 border-b border-slate-100 flex items-center justify-between bg-slate-900 text-white rounded-t-2xl">
                <div class="flex items-center gap-2">
                    <i class="fas fa-plug text-emerald-400"></i>
                    <h3 class="font-bold text-sm">Integrasi Google Apps Script & Spreadsheet Backend</h3>
                </div>
                <button onclick="closeGasModal()" class="text-slate-400 hover:text-white"><i class="fas fa-times"></i></button>
            </div>
            
            <div class="p-5 space-y-4 text-xs text-slate-600">
                <div class="bg-emerald-50 border border-emerald-200 p-3 rounded-xl text-emerald-900">
                    <p class="font-semibold text-xs mb-1"><i class="fas fa-check-circle"></i> Koneksi Database Google Sheets</p>
                    <p class="leading-relaxed">Aplikasi ini dapat disambungkan langsung ke Google Sheet kampus Anda secara gratis tanpa server tambahan. Setiap jenis surat akan dicatat otomatis pada sheet masing-masing.</p>
                </div>

                <div>
                    <label class="block font-bold text-slate-800 mb-1">Web App Deployment URL:</label>
                    <div class="flex gap-2">
                        <input type="url" id="gas-endpoint-url" placeholder="https://script.google.com/macros/s/AKfycbx.../exec" class="flex-1 px-3 py-1.5 border border-slate-300 rounded-lg font-mono text-xs focus:ring-2 focus:ring-emerald-500 focus:outline-none">
                        <button onclick="saveGasEndpoint()" class="bg-insa-700 hover:bg-insa-800 text-white px-4 py-1.5 rounded-lg font-semibold shadow">Simpan URL</button>
                    </div>
                </div>

                <hr class="border-slate-200">

                <div>
                    <div class="flex items-center justify-between mb-2">
                        <h4 class="font-bold text-slate-800 text-xs">Kode Backend Google Apps Script (Copy-Paste)</h4>
                        <button onclick="copyGasCode()" class="text-xs bg-slate-800 hover:bg-slate-700 text-white px-3 py-1 rounded font-medium flex items-center gap-1">
                            <i class="fas fa-copy"></i> Copy Script Code
                        </button>
                    </div>

                    <pre class="bg-slate-900 text-emerald-400 p-4 rounded-xl overflow-x-auto text-[11px] font-mono leading-relaxed h-52 border border-slate-800" id="gas-script-container">
// =========================================================================
// GOOGLE APPS SCRIPT BACKEND - SISTEM ADM PERSURATAN TERPADU INSA
// Buka Google Sheet > Ekstensi > Apps Script > Tempel Kode Ini
// =========================================================================

function doGet(e) {
  var action = e.parameter.action;
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  
  if (action === "readAll") {
    var sheetMasuk = ss.getSheetByName("SuratMasuk") || createSheetMasuk(ss);
    var sheetKeluar = ss.getSheetByName("SuratKeluar") || createSheetKeluar(ss);
    
    return ContentService.createTextOutput(JSON.stringify({
      status: "success",
      suratMasuk: sheetToObjects(sheetMasuk),
      suratKeluar: sheetToObjects(sheetKeluar)
    })).setMimeType(ContentService.MimeType.JSON);
  }
}

function doPost(e) {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var postData = JSON.parse(e.postData.contents);
  var action = postData.action;
  
  if (action === "saveMasuk") {
    var sheet = ss.getSheetByName("SuratMasuk") || createSheetMasuk(ss);
    var item = postData.data;
    sheet.appendRow([
      item.id, item.noAgenda, item.tglTerima, item.pengirim, 
      item.noSurat, item.tglSurat, item.urgensi, item.perihal, 
      item.status, item.fileLink, item.catatanDisposisi
    ]);
    return ContentService.createTextOutput(JSON.stringify({status: "success"})).setMimeType(ContentService.MimeType.JSON);
  }
  
  if (action === "saveKeluar") {
    var sheet = ss.getSheetByName("SuratKeluar") || createSheetKeluar(ss);
    var item = postData.data;
    sheet.appendRow([
      item.id, item.jenis, item.noSurat, item.tglSurat, 
      item.penerima, item.perihal, item.penandatangan, item.fileLink
    ]);

    // Opsi simpan ke tab terpisah per jenis surat
    var catSheetName = "LOG_" + item.jenis;
    var catSheet = ss.getSheetByName(catSheetName) || ss.insertSheet(catSheetName);
    if(catSheet.getLastRow() === 0) {
      catSheet.appendRow(["ID", "NoSurat", "Tanggal", "Penerima", "Perihal", "Ttd", "File"]);
    }
    catSheet.appendRow([item.id, item.noSurat, item.tglSurat, item.penerima, item.perihal, item.penandatangan, item.fileLink]);

    return ContentService.createTextOutput(JSON.stringify({status: "success"})).setMimeType(ContentService.MimeType.JSON);
  }
}

function sheetToObjects(sheet) {
  var data = sheet.getDataRange().getValues();
  if (data.length <= 1) return [];
  var headers = data[0];
  var result = [];
  for (var i = 1; i < data.length; i++) {
    var obj = {};
    for (var j = 0; j < headers.length; j++) {
      obj[headers[j]] = data[i][j];
    }
    result.push(obj);
  }
  return result;
}

function createSheetMasuk(ss) {
  var sh = ss.insertSheet("SuratMasuk");
  sh.appendRow(["id", "noAgenda", "tglTerima", "pengirim", "noSurat", "tglSurat", "urgensi", "perihal", "status", "fileLink", "catatanDisposisi"]);
  return sh;
}

function createSheetKeluar(ss) {
  var sh = ss.insertSheet("SuratKeluar");
  sh.appendRow(["id", "jenis", "noSurat", "tglSurat", "penerima", "perihal", "penandatangan", "fileLink"]);
  return sh;
}
                    </pre>
                </div>
            </div>

            <div class="p-4 border-t border-slate-100 flex justify-end">
                <button onclick="closeGasModal()" class="px-4 py-1.5 bg-slate-800 text-white rounded-lg text-xs font-semibold">Tutup</button>
            </div>
        </div>
    </div>

    <script>
        // Data State
        let state = {
            suratMasuk: [],
            suratKeluar: [],
            gasEndpoint: ''
        };

        // Kategori Kode Jenis Surat Resmi
        const jenisSuratConfig = {
            'SK-YYS': { name: 'SK Yayasan', code: 'YYS' },
            'SK-REK': { name: 'SK Rektor', code: 'REK' },
            'UND': { name: 'Surat Undangan', code: 'UND' },
            'PRM': { name: 'Surat Permohonan', code: 'PRM' },
            'PBT': { name: 'Surat Pemberitahuan', code: 'PBT' },
            'PJM': { name: 'Surat Peminjaman', code: 'PJM' },
            'PYT': { name: 'Surat Pernyataan', code: 'PYT' },
            'MND': { name: 'Surat Mandat', code: 'MND' },
            'STG': { name: 'Surat Tugas', code: 'STG' },
            'SKT': { name: 'Surat Keterangan', code: 'SKT' },
            'RKM': { name: 'Surat Rekomendasi', code: 'RKM' },
            'BLS': { name: 'Surat Balasan', code: 'BLS' },
            'SPD': { name: 'Surat Perjalanan Dinas', code: 'SPD' },
            'PJJ': { name: 'Surat Perjanjian / MoU', code: 'PJJ' },
            'PNG': { name: 'Surat Pengantar', code: 'PNG' },
            'BAC': { name: 'Surat Berita Acara', code: 'BAC' }
        };

        // Sample Demo Initial Data
        const demoSuratMasuk = [
            {
                id: 'sm-1',
                noAgenda: 'SM-2026-001',
                tglTerima: '2026-10-10',
                pengirim: 'LLDIKTI Region VII Jawa Timur',
                noSurat: '1420/LL7/TU/2026',
                tglSurat: '2026-10-08',
                urgensi: 'Sangat Penting',
                perihal: 'Rakor Penjaminan Mutu Perguruan Tinggi Swasta',
                status: 'Proses Disposisi',
                fileLink: 'https://drive.google.com',
                catatanDisposisi: 'Warek I dan LPM harap hadir mewakili Rektor.'
            },
            {
                id: 'sm-2',
                noAgenda: 'SM-2026-002',
                tglTerima: '2026-10-12',
                pengirim: 'Dinas Pendidikan Kab. Lamongan',
                noSurat: '421/889/413.101/2026',
                tglSurat: '2026-10-11',
                urgensi: 'Penting',
                perihal: 'Permohonan Kerjasama Program Pengabdian KKN Tematik',
                status: 'Belum Disposisi',
                fileLink: '',
                catatanDisposisi: ''
            }
        ];

        const demoSuratKeluar = [
            {
                id: 'sk-1',
                jenis: 'SK-REK',
                noSurat: '001/INSA/SK-REK/X/2026',
                tglSurat: '2026-10-02',
                penerima: 'Seluruh Civitas Akademika INSA',
                perihal: 'SK Rektor tentang Kalender Akademik TA 2026/2027',
                penandatangan: 'Rektor INSA',
                fileLink: 'https://drive.google.com'
            },
            {
                id: 'sk-2',
                jenis: 'STG',
                noSurat: '015/INSA/STG/X/2026',
                tglSurat: '2026-10-14',
                penerima: 'Dr. Ahmad Fauzi, M.Pd.',
                perihal: 'Surat Tugas Pendampingan Akreditasi Prodi Tarbiyah',
                penandatangan: 'Wakil Rektor I',
                fileLink: ''
            },
            {
                id: 'sk-3',
                jenis: 'SPD',
                noSurat: '008/INSA/SPD/X/2026',
                tglSurat: '2026-10-15',
                penerima: 'Tim LPPM INSA',
                perihal: 'Surat Perjalanan Dinas Monitoring KKN di Kecamatan Babat',
                penandatangan: 'Rektor INSA',
                fileLink: ''
            }
        ];

        window.addEventListener('DOMContentLoaded', () => {
            loadInitialData();
            updateDashboardStats();
            renderSuratMasukTable();
            renderSuratKeluarTable();
            checkGasStatus();
        });

        function loadInitialData() {
            const savedMasuk = localStorage.getItem('insa_surat_masuk');
            const savedKeluar = localStorage.getItem('insa_surat_keluar');
            const savedEndpoint = localStorage.getItem('insa_gas_endpoint');

            state.suratMasuk = savedMasuk ? JSON.parse(savedMasuk) : demoSuratMasuk;
            state.suratKeluar = savedKeluar ? JSON.parse(savedKeluar) : demoSuratKeluar;
            state.gasEndpoint = savedEndpoint || '';

            if (state.gasEndpoint) {
                document.getElementById('gas-endpoint-url').value = state.gasEndpoint;
                fetchDataFromGAS();
            }
        }

        function saveDataLocal() {
            localStorage.setItem('insa_surat_masuk', JSON.stringify(state.suratMasuk));
            localStorage.setItem('insa_surat_keluar', JSON.stringify(state.suratKeluar));
        }

        // NAVIGATION SWITCH
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active', 'bg-insa-50', 'text-insa-800');
                btn.classList.add('text-slate-600');
            });

            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`nav-${tabId}`);
            if (activeBtn) {
                activeBtn.classList.add('active', 'bg-insa-50', 'text-insa-800');
                activeBtn.classList.remove('text-slate-600');
            }
        }

        function filterByJenis(jenisCode) {
            switchTab('surat-keluar');
            document.getElementById('filter-jenis-keluar').value = jenisCode;
            filterSuratKeluar();
        }

        // STATS COMPUTATION
        function updateDashboardStats() {
            document.getElementById('stat-total-masuk').innerText = state.suratMasuk.length;
            document.getElementById('stat-total-keluar').innerText = state.suratKeluar.length;
            
            const pending = state.suratMasuk.filter(s => s.status === 'Belum Disposisi').length;
            document.getElementById('stat-pending-disposisi').innerText = pending;

            const skTugas = state.suratKeluar.filter(s => s.jenis === 'SK-REK' || s.jenis === 'SK-YYS' || s.jenis === 'STG' || s.jenis === 'SPD').length;
            document.getElementById('stat-sk-tugas').innerText = skTugas;

            document.getElementById('badge-surat-masuk').innerText = state.suratMasuk.length;
            document.getElementById('badge-surat-keluar').innerText = state.suratKeluar.length;

            // Category counts
            Object.keys(jenisSuratConfig).forEach(key => {
                const count = state.suratKeluar.filter(s => s.jenis === key).length;
                const badgeEl = document.getElementById(`cat-count-${key}`);
                if (badgeEl) badgeEl.innerText = count;
            });

            renderRecentTables();
        }

        function renderRecentTables() {
            const recentMasukBody = document.getElementById('recent-masuk-table');
            const recentKeluarBody = document.getElementById('recent-keluar-table');

            recentMasukBody.innerHTML = state.suratMasuk.slice(0, 4).map(item => `
                <tr class="hover:bg-slate-50">
                    <td class="p-2 font-mono font-bold text-blue-700">${escapeHtml(item.noAgenda)}</td>
                    <td class="p-2 truncate max-w-[110px]">${escapeHtml(item.pengirim)}</td>
                    <td class="p-2 truncate max-w-[140px]">${escapeHtml(item.perihal)}</td>
                    <td class="p-2">${getStatusBadgeHtml(item.status)}</td>
                </tr>
            `).join('') || '<tr><td colspan="4" class="p-3 text-center text-slate-400">Belum ada data</td></tr>';

            recentKeluarBody.innerHTML = state.suratKeluar.slice(0, 4).map(item => `
                <tr class="hover:bg-slate-50">
                    <td class="p-2 font-mono font-bold text-emerald-700">${escapeHtml(item.noSurat)}</td>
                    <td class="p-2 font-semibold text-slate-600">${item.jenis}</td>
                    <td class="p-2 truncate max-w-[110px]">${escapeHtml(item.penerima)}</td>
                    <td class="p-2">${escapeHtml(item.tglSurat)}</td>
                </tr>
            `).join('') || '<tr><td colspan="4" class="p-3 text-center text-slate-400">Belum ada data</td></tr>';
        }

        // RENDER SURAT MASUK TABLE
        function renderSuratMasukTable(data = state.suratMasuk) {
            const tbody = document.getElementById('table-surat-masuk-body');
            const emptyState = document.getElementById('empty-masuk');

            if (data.length === 0) {
                tbody.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            tbody.innerHTML = data.map(item => `
                <tr class="hover:bg-slate-50 border-b border-slate-100 transition">
                    <td class="p-3">
                        <div class="font-mono font-bold text-blue-700">${escapeHtml(item.noAgenda)}</div>
                        <div class="text-[10px] text-slate-400"><i class="fas fa-calendar-alt"></i> ${escapeHtml(item.tglTerima)}</div>
                    </td>
                    <td class="p-3">
                        <div class="font-semibold text-slate-800">${escapeHtml(item.pengirim)}</div>
                        <div class="text-[10px] text-slate-500 font-mono">No: ${escapeHtml(item.noSurat)}</div>
                    </td>
                    <td class="p-3 max-w-xs">
                        <div class="font-medium text-slate-800 line-clamp-2">${escapeHtml(item.perihal)}</div>
                        ${item.fileLink ? `<a href="${escapeHtml(item.fileLink)}" target="_blank" class="inline-flex items-center gap-1 text-[10px] text-blue-600 hover:underline mt-1"><i class="fas fa-paperclip"></i> Lihat Dokumen</a>` : ''}
                    </td>
                    <td class="p-3">${getUrgensiBadgeHtml(item.urgensi)}</td>
                    <td class="p-3">${getStatusBadgeHtml(item.status)}</td>
                    <td class="p-3 text-center">
                        <div class="flex items-center justify-center gap-1">
                            <button onclick="printDisposisiSheet('${item.id}')" title="Cetak Disposisi" class="p-1.5 bg-blue-50 text-blue-700 hover:bg-blue-100 rounded-lg transition">
                                <i class="fas fa-print"></i>
                            </button>
                            <button onclick="editSuratMasuk('${item.id}')" title="Edit" class="p-1.5 bg-amber-50 text-amber-700 hover:bg-amber-100 rounded-lg transition">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button onclick="deleteSuratMasuk('${item.id}')" title="Hapus" class="p-1.5 bg-rose-50 text-rose-700 hover:bg-rose-100 rounded-lg transition">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                    </td>
                </tr>
            `).join('');
        }

        // RENDER SURAT KELUAR TABLE
        function renderSuratKeluarTable(data = state.suratKeluar) {
            const tbody = document.getElementById('table-surat-keluar-body');
            const emptyState = document.getElementById('empty-keluar');

            if (data.length === 0) {
                tbody.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            tbody.innerHTML = data.map(item => `
                <tr class="hover:bg-slate-50 border-b border-slate-100 transition">
                    <td class="p-3">
                        <div class="font-mono font-bold text-emerald-700">${escapeHtml(item.noSurat)}</div>
                        <div class="text-[10px] text-slate-400"><i class="fas fa-calendar-alt"></i> ${escapeHtml(item.tglSurat)}</div>
                    </td>
                    <td class="p-3">
                        <span class="px-2 py-0.5 bg-slate-100 text-slate-700 text-[10px] font-bold rounded border border-slate-200 inline-block">${escapeHtml(item.jenis)}</span>
                    </td>
                    <td class="p-3 font-semibold text-slate-800">${escapeHtml(item.penerima)}</td>
                    <td class="p-3 max-w-xs">
                        <div class="font-medium text-slate-800 line-clamp-2">${escapeHtml(item.perihal)}</div>
                        ${item.fileLink ? `<a href="${escapeHtml(item.fileLink)}" target="_blank" class="inline-flex items-center gap-1 text-[10px] text-emerald-600 hover:underline mt-1"><i class="fas fa-file-pdf"></i> Draft PDF</a>` : ''}
                    </td>
                    <td class="p-3 font-medium text-slate-700">${escapeHtml(item.penandatangan)}</td>
                    <td class="p-3 text-center">
                        <div class="flex items-center justify-center gap-1">
                            <button onclick="editSuratKeluar('${item.id}')" title="Edit Data" class="p-1.5 bg-amber-50 text-amber-700 hover:bg-amber-100 rounded-lg transition">
                                <i class="fas fa-edit"></i>
                            </button>
                            <button onclick="deleteSuratKeluar('${item.id}')" title="Hapus Data" class="p-1.5 bg-rose-50 text-rose-700 hover:bg-rose-100 rounded-lg transition">
                                <i class="fas fa-trash"></i>
                            </button>
                        </div>
                    </td>
                </tr>
            `).join('');
        }

        // BADGE UTILITIES
        function getStatusBadgeHtml(status) {
            if (status === 'Belum Disposisi') return `<span class="px-2 py-0.5 bg-amber-100 text-amber-800 rounded-full text-[10px] font-semibold border border-amber-200">Belum Disposisi</span>`;
            if (status === 'Proses Disposisi') return `<span class="px-2 py-0.5 bg-blue-100 text-blue-800 rounded-full text-[10px] font-semibold border border-blue-200">Proses</span>`;
            return `<span class="px-2 py-0.5 bg-emerald-100 text-emerald-800 rounded-full text-[10px] font-semibold border border-emerald-200">Selesai</span>`;
        }

        function getUrgensiBadgeHtml(urgensi) {
            if (urgensi === 'Sangat Penting') return `<span class="px-2 py-0.5 bg-rose-100 text-rose-700 font-bold rounded text-[10px]">Sangat Penting</span>`;
            if (urgensi === 'Penting') return `<span class="px-2 py-0.5 bg-amber-100 text-amber-700 font-bold rounded text-[10px]">Penting</span>`;
            return `<span class="px-2 py-0.5 bg-slate-100 text-slate-600 font-medium rounded text-[10px]">Biasa</span>`;
        }

        // SEARCH & FILTERS
        function filterSuratMasuk() {
            const query = document.getElementById('search-masuk').value.toLowerCase();
            const urgensi = document.getElementById('filter-urgensi-masuk').value;
            const status = document.getElementById('filter-status-masuk').value;

            const filtered = state.suratMasuk.filter(item => {
                const matchesQuery = item.noAgenda.toLowerCase().includes(query) ||
                                     item.pengirim.toLowerCase().includes(query) ||
                                     item.perihal.toLowerCase().includes(query) ||
                                     item.noSurat.toLowerCase().includes(query);
                const matchesUrgensi = !urgensi || item.urgensi === urgensi;
                const matchesStatus = !status || item.status === status;
                return matchesQuery && matchesUrgensi && matchesStatus;
            });

            renderSuratMasukTable(filtered);
        }

        function filterSuratKeluar() {
            const query = document.getElementById('search-keluar').value.toLowerCase();
            const jenis = document.getElementById('filter-jenis-keluar').value;

            const filtered = state.suratKeluar.filter(item => {
                const matchesQuery = item.noSurat.toLowerCase().includes(query) ||
                                     item.penerima.toLowerCase().includes(query) ||
                                     item.perihal.toLowerCase().includes(query);
                const matchesJenis = !jenis || item.jenis === jenis;
                return matchesQuery && matchesJenis;
            });

            renderSuratKeluarTable(filtered);
        }

        // SURAT MASUK CRUD
        function openFormMasukModal(editId = null) {
            document.getElementById('form-masuk').reset();
            document.getElementById('masuk-id').value = '';
            document.getElementById('modal-masuk-title').innerHTML = `<i class="fas fa-inbox text-blue-600"></i> Register Surat Masuk`;

            if (editId) {
                const item = state.suratMasuk.find(s => s.id === editId);
                if (item) {
                    document.getElementById('masuk-id').value = item.id;
                    document.getElementById('masuk-no-agenda').value = item.noAgenda;
                    document.getElementById('masuk-tgl-terima').value = item.tglTerima;
                    document.getElementById('masuk-pengirim').value = item.pengirim;
                    document.getElementById('masuk-no-surat').value = item.noSurat;
                    document.getElementById('masuk-tgl-surat').value = item.tglSurat;
                    document.getElementById('masuk-urgensi').value = item.urgensi;
                    document.getElementById('masuk-perihal').value = item.perihal;
                    document.getElementById('masuk-status').value = item.status;
                    document.getElementById('masuk-file-link').value = item.fileLink || '';
                    document.getElementById('masuk-catatan-disposisi').value = item.catatanDisposisi || '';
                }
            } else {
                const nextNum = state.suratMasuk.length + 1;
                document.getElementById('masuk-no-agenda').value = `SM-2026-${String(nextNum).padStart(3, '0')}`;
                document.getElementById('masuk-tgl-terima').valueAsDate = new Date();
                document.getElementById('masuk-tgl-surat').valueAsDate = new Date();
            }

            document.getElementById('modal-surat-masuk').classList.remove('hidden');
        }

        function closeFormMasukModal() {
            document.getElementById('modal-surat-masuk').classList.add('hidden');
        }

        function saveSuratMasuk(e) {
            e.preventDefault();
            const id = document.getElementById('masuk-id').value || `sm-${Date.now()}`;
            
            const itemData = {
                id: id,
                noAgenda: document.getElementById('masuk-no-agenda').value,
                tglTerima: document.getElementById('masuk-tgl-terima').value,
                pengirim: document.getElementById('masuk-pengirim').value,
                noSurat: document.getElementById('masuk-no-surat').value,
                tglSurat: document.getElementById('masuk-tgl-surat').value,
                urgensi: document.getElementById('masuk-urgensi').value,
                perihal: document.getElementById('masuk-perihal').value,
                status: document.getElementById('masuk-status').value,
                fileLink: document.getElementById('masuk-file-link').value,
                catatanDisposisi: document.getElementById('masuk-catatan-disposisi').value
            };

            const index = state.suratMasuk.findIndex(s => s.id === id);
            if (index > -1) {
                state.suratMasuk[index] = itemData;
            } else {
                state.suratMasuk.unshift(itemData);
            }

            saveDataLocal();
            updateDashboardStats();
            renderSuratMasukTable();
            closeFormMasukModal();

            if (state.gasEndpoint) {
                sendToGAS('saveMasuk', itemData);
            }
        }

        function editSuratMasuk(id) {
            openFormMasukModal(id);
        }

        function deleteSuratMasuk(id) {
            if (confirm('Hapus registrasi surat masuk ini?')) {
                state.suratMasuk = state.suratMasuk.filter(s => s.id !== id);
                saveDataLocal();
                updateDashboardStats();
                renderSuratMasukTable();
            }
        }

        // SURAT KELUAR CRUD & AUTO NUMBER GENERATOR
        function openFormKeluarModal(defaultJenis = 'STG', editId = null) {
            document.getElementById('form-keluar').reset();
            document.getElementById('keluar-id').value = '';

            if (editId) {
                const item = state.suratKeluar.find(s => s.id === editId);
                if (item) {
                    document.getElementById('keluar-id').value = item.id;
                    document.getElementById('keluar-jenis').value = item.jenis;
                    document.getElementById('keluar-no-surat').value = item.noSurat;
                    document.getElementById('keluar-tgl-surat').value = item.tglSurat;
                    document.getElementById('keluar-penerima').value = item.penerima;
                    document.getElementById('keluar-perihal').value = item.perihal;
                    document.getElementById('keluar-penandatangan').value = item.penandatangan;
                    document.getElementById('keluar-file-link').value = item.fileLink || '';
                }
            } else {
                document.getElementById('keluar-jenis').value = defaultJenis;
                document.getElementById('keluar-tgl-surat').valueAsDate = new Date();
                generateAutoNoSurat();
            }

            document.getElementById('modal-surat-keluar').classList.remove('hidden');
        }

        function closeFormKeluarModal() {
            document.getElementById('modal-surat-keluar').classList.add('hidden');
        }

        function onJenisSuratChange() {
            const editId = document.getElementById('keluar-id').value;
            if (!editId) {
                generateAutoNoSurat();
            }
        }

        function generateAutoNoSurat() {
            const jenisKey = document.getElementById('keluar-jenis').value;
            const config = jenisSuratConfig[jenisKey] || { code: 'SURAT' };
            
            // Count existing for this category
            const countInCat = state.suratKeluar.filter(s => s.jenis === jenisKey).length + 1;
            const nextNum = String(countInCat).padStart(3, '0');
            
            const romanMonths = ["I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI", "XII"];
            const currentMonth = romanMonths[new Date().getMonth()];
            const currentYear = new Date().getFullYear();

            document.getElementById('keluar-no-surat').value = `${nextNum}/INSA/${config.code}/${currentMonth}/${currentYear}`;
        }

        function saveSuratKeluar(e) {
            e.preventDefault();
            const id = document.getElementById('keluar-id').value || `sk-${Date.now()}`;

            const itemData = {
                id: id,
                jenis: document.getElementById('keluar-jenis').value,
                noSurat: document.getElementById('keluar-no-surat').value,
                tglSurat: document.getElementById('keluar-tgl-surat').value,
                penerima: document.getElementById('keluar-penerima').value,
                perihal: document.getElementById('keluar-perihal').value,
                penandatangan: document.getElementById('keluar-penandatangan').value,
                fileLink: document.getElementById('keluar-file-link').value
            };

            const index = state.suratKeluar.findIndex(s => s.id === id);
            if (index > -1) {
                state.suratKeluar[index] = itemData;
            } else {
                state.suratKeluar.unshift(itemData);
            }

            saveDataLocal();
            updateDashboardStats();
            renderSuratKeluarTable();
            closeFormKeluarModal();

            if (state.gasEndpoint) {
                sendToGAS('saveKeluar', itemData);
            }
        }

        function editSuratKeluar(id) {
            openFormKeluarModal('STG', id);
        }

        function deleteSuratKeluar(id) {
            if (confirm('Hapus registrasi surat keluar ini?')) {
                state.suratKeluar = state.suratKeluar.filter(s => s.id !== id);
                saveDataLocal();
                updateDashboardStats();
                renderSuratKeluarTable();
            }
        }

        // PRINT LEMBAR DISPOSISI
        function printDisposisiSheet(id) {
            const item = state.suratMasuk.find(s => s.id === id);
            if (!item) return;

            document.getElementById('disp-pengirim').innerText = item.pengirim;
            document.getElementById('disp-no-agenda').innerText = item.noAgenda;
            document.getElementById('disp-no-surat').innerText = item.noSurat;
            document.getElementById('disp-tgl-terima').innerText = item.tglTerima;
            document.getElementById('disp-tgl-surat').innerText = item.tglSurat;
            document.getElementById('disp-urgensi').innerText = item.urgensi;
            document.getElementById('disp-perihal').innerText = item.perihal;
            document.getElementById('disp-catatan').innerText = item.catatanDisposisi || '( Belum ada catatan tertulis )';
            document.getElementById('disp-tgl-ttd').innerText = new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'long', year: 'numeric' });

            window.print();
        }

        // EXPORT DATA TO CSV
        function exportDataToCSV() {
            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "TIPE,ID,NO_AGENDA_SURAT,TANGGAL,PENGIRIM_PENERIMA,JENIS_SURAT,PERIHAL,URGENSI,STATUS\n";

            state.suratMasuk.forEach(m => {
                csvContent += `SURAT_MASUK,"${m.id}","${m.noAgenda}","${m.tglTerima}","${m.pengirim}","MASUK","${m.perihal.replace(/"/g, '""')}","${m.urgensi}","${m.status}"\n`;
            });

            state.suratKeluar.forEach(k => {
                csvContent += `SURAT_KELUAR,"${k.id}","${k.noSurat}","${k.tglSurat}","${k.penerima}","${k.jenis}","${k.perihal.replace(/"/g, '""')}","-","TERBIT"\n`;
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `arsip_persuratan_INSA_${new Date().toISOString().slice(0,10)}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }

        // GAS MODAL & ENDPOINT API
        function openGasModal() {
            document.getElementById('modal-gas').classList.remove('hidden');
        }

        function closeGasModal() {
            document.getElementById('modal-gas').classList.add('hidden');
        }

        function saveGasEndpoint() {
            const url = document.getElementById('gas-endpoint-url').value.trim();
            state.gasEndpoint = url;
            localStorage.setItem('insa_gas_endpoint', url);
            checkGasStatus();
            closeGasModal();

            if (url) {
                fetchDataFromGAS();
            }
        }

        function checkGasStatus() {
            const badge = document.getElementById('gasStatusBadge');
            if (state.gasEndpoint) {
                badge.className = 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-emerald-100 text-emerald-800 border border-emerald-200';
                badge.innerHTML = `<span class="w-2 h-2 mr-1.5 bg-emerald-500 rounded-full"></span> Terhubung Google Sheets`;
            } else {
                badge.className = 'inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-amber-100 text-amber-800 border border-amber-200';
                badge.innerHTML = `<span class="w-2 h-2 mr-1.5 bg-amber-500 rounded-full animate-pulse"></span> Mode Penyiapan Lokal`;
            }
        }

        function fetchDataFromGAS() {
            if (!state.gasEndpoint) return;
            fetch(`${state.gasEndpoint}?action=readAll`)
                .then(res => res.json())
                .then(data => {
                    if (data.status === 'success') {
                        if (data.suratMasuk && data.suratMasuk.length > 0) state.suratMasuk = data.suratMasuk;
                        if (data.suratKeluar && data.suratKeluar.length > 0) state.suratKeluar = data.suratKeluar;
                        saveDataLocal();
                        updateDashboardStats();
                        renderSuratMasukTable();
                        renderSuratKeluarTable();
                    }
                })
                .catch(err => console.log('Google Apps Script mode offline/backup lokal'));
        }

        function sendToGAS(action, data) {
            if (!state.gasEndpoint) return;
            fetch(state.gasEndpoint, {
                method: 'POST',
                mode: 'no-cors',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ action: action, data: data })
            }).catch(err => console.error('Gagal mengirim data ke Apps Script:', err));
        }

        function copyGasCode() {
            const codeText = document.getElementById('gas-script-container').innerText;
            const textArea = document.createElement("textarea");
            textArea.value = codeText;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand('copy');
            document.body.removeChild(textArea);
            alert('Kode Apps Script berhasil disalin ke clipboard!');
        }

        function resetToDemoData() {
            if (confirm('Kembalikan data sampel demo awal persuratan INSA?')) {
                state.suratMasuk = [...demoSuratMasuk];
                state.suratKeluar = [...demoSuratKeluar];
                saveDataLocal();
                updateDashboardStats();
                renderSuratMasukTable();
                renderSuratKeluarTable();
            }
        }

        function escapeHtml(str) {
            if (!str) return '';
            return String(str)
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
