<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Stok Opname - Caffe TokoKopiTuju</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0f6ff',
                            100: '#e0effe',
                            200: '#bae0fd',
                            300: '#7cc5fb',
                            400: '#38a5f8',
                            500: '#0e86eb',
                            600: '#026dc7',
                            700: '#0355a1',
                            800: '#074885',
                            900: '#0b3d6f',
                            950: '#072647',
                        }
                    }
                }
            }
        }
    </script>
    <!-- Font Poppins & Lucide Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    <style>
        body { font-family: 'Poppins', sans-serif; }
    </style>
</head>
<body class="h-full text-slate-800 flex flex-col bg-slate-50">

    <!-- Header / Navbar dengan Pemilihan 5 Cabang Outlet -->
    <header class="bg-brand-900 text-white shadow-md sticky top-0 z-30 border-b border-brand-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between">
            <div class="flex items-center space-x-3 sm:space-x-4">
                <div class="bg-white p-2.5 rounded-2xl text-brand-900 shadow-md border border-brand-200 flex items-center justify-center font-bold text-xl tracking-tighter">
                    <span class="text-brand-900 font-extrabold text-2xl px-1">7.</span>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-bold text-base sm:text-xl tracking-wide text-white">TokoKopiTuju</h1>
                    </div>
                    <div class="flex items-center gap-1.5 mt-0.5">
                        <span class="text-xs text-brand-300">Outlet:</span>
                        <select id="selectCabang" onchange="gantiCabang()" class="bg-brand-800 text-white text-xs font-semibold px-2.5 py-1 rounded-xl border border-brand-700 focus:outline-none focus:ring-2 focus:ring-brand-400 cursor-pointer">
                            <option value="bajigau">Cabang Baji Gau</option>
                            <option value="tamalate">Cabang Tamalate</option>
                            <option value="perintis">Cabang Perintis</option>
                            <option value="agussalim">Cabang Agus Salim Gowa</option>
                            <option value="abdesir">Cabang Abdesir</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="flex items-center space-x-2">
                <button onclick="resetDataPaksa()" title="Muat Ulang Data Awal" class="px-3 py-2 bg-amber-600 hover:bg-amber-700 rounded-xl text-white transition flex items-center gap-1.5 text-xs font-medium shadow-sm">
                    <i data-lucide="refresh-cw" class="w-4 h-4"></i> <span class="hidden sm:inline">Reset Data</span>
                </button>
                <button onclick="exportData()" title="Backup Data Cabang Ini" class="px-3 py-2 bg-brand-800 hover:bg-brand-700 rounded-xl text-brand-100 hover:text-white transition flex items-center gap-1.5 text-xs font-medium border border-brand-700 shadow-sm">
                    <i data-lucide="download" class="w-4 h-4 text-brand-300"></i> <span class="hidden sm:inline">Backup</span>
                </button>
                <label title="Restore Data Cabang Ini" class="px-3 py-2 bg-brand-800 hover:bg-brand-700 rounded-xl text-brand-100 hover:text-white transition flex items-center gap-1.5 text-xs font-medium border border-brand-700 cursor-pointer shadow-sm">
                    <i data-lucide="upload" class="w-4 h-4 text-brand-300"></i> <span class="hidden sm:inline">Restore</span>
                    <input type="file" id="importFile" accept=".json" onchange="importData(event)" class="hidden">
                </label>
            </div>
        </div>
    </header>

    <!-- Main Navigation Tabs -->
    <nav class="bg-brand-800 text-brand-100 shadow-inner">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex space-x-1 sm:space-x-3 overflow-x-auto py-1.5">
            <button onclick="switchTab('dashboard')" id="nav-dashboard" class="tab-btn px-4 py-2.5 rounded-xl font-medium text-sm flex items-center gap-2 transition whitespace-nowrap">
                <i data-lucide="layout-dashboard" class="w-4 h-4"></i> Dashboard
            </button>
            <button onclick="switchTab('stok')" id="nav-stok" class="tab-btn px-4 py-2.5 rounded-xl font-medium text-sm flex items-center gap-2 transition whitespace-nowrap">
                <i data-lucide="package" class="w-4 h-4"></i> Data Bahan Baku
            </button>
            <button onclick="switchTab('opname')" id="nav-opname" class="tab-btn px-4 py-2.5 rounded-xl font-medium text-sm flex items-center gap-2 transition whitespace-nowrap">
                <i data-lucide="clipboard-check" class="w-4 h-4"></i> Mulai Opname Harian
            </button>
            <button onclick="switchTab('riwayat')" id="nav-riwayat" class="tab-btn px-4 py-2.5 rounded-xl font-medium text-sm flex items-center gap-2 transition whitespace-nowrap">
                <i data-lucide="history" class="w-4 h-4"></i> Riwayat & Laporan
            </button>
        </div>
    </nav>

    <!-- Content Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 overflow-y-auto">

        <!-- TAB 1: DASHBOARD -->
        <section id="tab-dashboard" class="tab-content space-y-6">
            <div class="relative overflow-hidden bg-gradient-to-r from-brand-900 via-brand-800 to-brand-950 rounded-3xl p-8 text-white shadow-xl border border-brand-700">
                <div class="absolute -right-10 -bottom-10 opacity-10 text-white pointer-events-none">
                    <i data-lucide="coffee" class="w-64 h-64"></i>
                </div>
                <div class="relative z-10 flex flex-col md:flex-row justify-between items-start md:items-center gap-6">
                    <div class="space-y-2">
                        <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-white/10 border border-white/20 text-brand-200 text-xs font-semibold">
                            <span class="w-2 h-2 rounded-full bg-blue-400 animate-pulse"></span>
                            Outlet Aktif: <span id="labelNamaOutlet" class="text-white font-bold uppercase">Cabang Baji Gau</span>
                        </div>
                        <h2 class="text-3xl font-bold tracking-tight text-white">Form Barista Harian ☕</h2>
                        <p class="text-brand-200 text-sm max-w-xl">Pantau inventaris bahan gudang dan penunjang barista berdasarkan laporan fisik harian.</p>
                    </div>
                    <button onclick="switchTab('opname')" class="bg-white text-brand-900 hover:bg-brand-50 font-semibold px-6 py-3 rounded-2xl shadow-lg transition flex items-center gap-2 text-sm border border-brand-100">
                        <i data-lucide="clipboard-check" class="w-5 h-5 text-brand-700"></i> Mulai Opname Hari Ini
                    </button>
                </div>
            </div>

            <!-- Statistik Cards -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-200 flex items-center justify-between hover:shadow-md transition">
                    <div>
                        <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Total Jenis Item</p>
                        <h3 id="statTotalBahan" class="text-3xl font-bold text-slate-900 mt-1">0</h3>
                        <p class="text-[11px] text-slate-400 mt-1">Item aktif di sistem</p>
                    </div>
                    <div class="p-4 bg-brand-50 text-brand-600 rounded-2xl">
                        <i data-lucide="boxes" class="w-7 h-7"></i>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-200 flex items-center justify-between hover:shadow-md transition">
                    <div>
                        <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Stok Menipis</p>
                        <h3 id="statBahanMenipis" class="text-3xl font-bold text-amber-600 mt-1">0</h3>
                        <p class="text-[11px] text-amber-600/80 mt-1">Segera lakukan restock</p>
                    </div>
                    <div class="p-4 bg-amber-50 text-amber-600 rounded-2xl">
                        <i data-lucide="alert-triangle" class="w-7 h-7"></i>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-200 flex items-center justify-between hover:shadow-md transition">
                    <div>
                        <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Status Opname Hari Ini</p>
                        <h3 id="statStatusHariIni" class="text-base font-bold text-slate-900 mt-2">Belum Dilakukan</h3>
                        <p id="statStatusSub" class="text-[11px] text-slate-400 mt-1">Pengecekan fisik barista</p>
                    </div>
                    <div id="statStatusIcon" class="p-4 bg-rose-50 text-rose-600 rounded-2xl">
                        <i data-lucide="clock" class="w-7 h-7"></i>
                    </div>
                </div>
                <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-200 flex items-center justify-between hover:shadow-md transition">
                    <div>
                        <p class="text-xs font-semibold text-slate-500 uppercase tracking-wider">Total Riwayat Opname</p>
                        <h3 id="statTotalRiwayat" class="text-3xl font-bold text-slate-900 mt-1">0</h3>
                        <p class="text-[11px] text-slate-400 mt-1">Arsip laporan tersimpan</p>
                    </div>
                    <div class="p-4 bg-brand-50 text-brand-600 rounded-2xl">
                        <i data-lucide="file-spreadsheet" class="w-7 h-7"></i>
                    </div>
                </div>
            </div>

            <!-- Tabel Peringatan Stok Menipis -->
            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 p-6 sm:p-8">
                <div class="flex justify-between items-center mb-6">
                    <div>
                        <h3 class="font-bold text-lg text-slate-900 flex items-center gap-2">
                            <i data-lucide="alert-circle" class="w-5 h-5 text-amber-600"></i> Perhatian: Stok Item Menipis
                        </h3>
                        <p class="text-xs text-slate-500 mt-0.5">Daftar item yang berada di bawah batas minimum.</p>
                    </div>
                </div>
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="border-b border-slate-200 text-xs text-slate-500 uppercase bg-slate-50">
                                <th class="p-4 rounded-l-xl font-semibold">Nama Item</th>
                                <th class="p-4 font-semibold">Kategori</th>
                                <th class="p-4 font-semibold">Stok Sistem</th>
                                <th class="p-4 font-semibold">Batas Minimum</th>
                                <th class="p-4 rounded-r-xl font-semibold">Satuan</th>
                            </tr>
                        </thead>
                        <tbody id="tabelMenipisBody" class="text-sm divide-y divide-slate-100">
                            <!-- Diisi JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 2: DATA BAHAN BAKU -->
        <section id="tab-stok" class="tab-content hidden space-y-6">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-5 rounded-3xl shadow-sm border border-slate-200">
                <div class="flex flex-col sm:flex-row gap-3 w-full sm:w-auto">
                    <input type="text" id="searchBahan" oninput="renderBahan()" placeholder="Cari nama item..." class="px-4 py-2.5 border border-slate-200 rounded-2xl text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 w-full sm:w-64 bg-slate-50">
                    <select id="filterKategori" onchange="renderBahan()" class="px-4 py-2.5 border border-slate-200 rounded-2xl text-sm focus:outline-none focus:ring-2 focus:ring-brand-500 bg-slate-50">
                        <option value="">Semua Kategori</option>
                        <option value="Stok Gudang">Stok Gudang</option>
                        <option value="Stok Penunjang">Stok Penunjang</option>
                    </select>
                </div>
                <button onclick="openModalBahan()" class="bg-brand-900 hover:bg-brand-950 text-white font-medium px-5 py-2.5 rounded-2xl text-sm shadow transition flex items-center gap-2 w-full sm:w-auto justify-center">
                    <i data-lucide="plus" class="w-4 h-4"></i> Tambah Item Baru
                </button>
            </div>

            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 border-b border-slate-200 text-xs text-slate-500 uppercase">
                                <th class="p-4 font-semibold">Nama Item</th>
                                <th class="p-4 font-semibold">Kategori</th>
                                <th class="p-4 font-semibold">Stok Saat Ini</th>
                                <th class="p-4 font-semibold">Batas Min.</th>
                                <th class="p-4 font-semibold">Satuan</th>
                                <th class="p-4 font-semibold text-center">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="tabelBahanBody" class="text-sm divide-y divide-slate-100">
                            <!-- Diisi JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 3: MULAI OPNAME -->
        <section id="tab-opname" class="tab-content hidden space-y-6">
            <div class="bg-white p-6 rounded-3xl shadow-sm border border-slate-200 flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                <div>
                    <h2 class="text-xl font-bold text-slate-900">Form Stok Opname Harian</h2>
                    <p class="text-xs text-slate-500 mt-1">Masukkan hasil perhitungan fisik stok barista.</p>
                </div>
                <div class="flex items-center gap-3 w-full md:w-auto">
                    <div>
                        <label class="block text-xs font-semibold text-slate-600 mb-1">Tanggal Opname</label>
                        <input type="date" id="opnameTanggal" class="px-3.5 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500 font-medium">
                    </div>
                    <button onclick="simpanOpname()" class="mt-5 bg-brand-900 hover:bg-brand-950 text-white font-medium px-5 py-2.5 rounded-2xl text-sm shadow transition flex items-center gap-2">
                        <i data-lucide="save" class="w-4 h-4"></i> Simpan Opname
                    </button>
                </div>
            </div>

            <div class="bg-white rounded-3xl shadow-sm border border-slate-200 overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 border-b border-slate-200 text-xs text-slate-500 uppercase">
                                <th class="p-4 font-semibold">Nama Item</th>
                                <th class="p-4 font-semibold">Kategori</th>
                                <th class="p-4 font-semibold">Stok Sistem</th>
                                <th class="p-4 font-semibold">Stok Fisik (Riil)</th>
                                <th class="p-4 font-semibold">Selisih</th>
                                <th class="p-4 font-semibold">Catatan / Keterangan</th>
                            </tr>
                        </thead>
                        <tbody id="tabelOpnameBody" class="text-sm divide-y divide-slate-100">
                            <!-- Diisi JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 4: RIWAYAT & LAPORAN -->
        <section id="tab-riwayat" class="tab-content hidden space-y-6">
            <div class="bg-white p-6 sm:p-8 rounded-3xl shadow-sm border border-slate-200">
                <h2 class="text-xl font-bold text-slate-900 mb-1">Riwayat Laporan Stok Opname</h2>
                <p class="text-xs text-slate-500 mb-6">Arsip laporan pengecekan harian untuk outlet yang dipilih.</p>
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 border-b border-slate-200 text-xs text-slate-500 uppercase">
                                <th class="p-4 font-semibold rounded-l-xl">Tanggal Opname</th>
                                <th class="p-4 font-semibold">Total Jenis Item Dicek</th>
                                <th class="p-4 font-semibold">Jumlah Ada Selisih</th>
                                <th class="p-4 font-semibold text-center rounded-r-xl">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="tabelRiwayatBody" class="text-sm divide-y divide-slate-100">
                            <!-- Diisi JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <!-- Footer Cafe -->
    <footer class="bg-brand-950 text-brand-200 text-xs py-6 mt-12 border-t border-brand-900">
        <div class="max-w-7xl mx-auto px-4 text-center space-y-1">
            <p class="font-semibold text-white">TokoKopiTuju — Form Barista Harian</p>
            <p class="text-brand-300">Sistem Manajemen Inventaris & Stok Opname Bahan Baku.</p>
        </div>
    </footer>

    <!-- MODAL TAMBAH / EDIT ITEM -->
    <div id="modalBahan" class="fixed inset-0 bg-slate-950/60 backdrop-blur-sm hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-3xl max-w-md w-full p-6 sm:p-8 shadow-2xl border border-slate-200">
            <div class="flex justify-between items-center mb-5">
                <h3 id="modalBahanTitle" class="font-bold text-lg text-slate-900">Tambah Item Baru</h3>
                <button onclick="closeModalBahan()" class="text-slate-400 hover:text-slate-700 p-1">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <form id="formBahan" onsubmit="saveBahan(event)" class="space-y-4">
                <input type="hidden" id="bahanIndex">
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1.5">Nama Item</label>
                    <input type="text" id="inputNama" required class="w-full px-4 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500" placeholder="Contoh: Beans blend">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1.5">Kategori</label>
                    <select id="inputKategori" required class="w-full px-4 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500">
                        <option value="Stok Gudang">Stok Gudang</option>
                        <option value="Stok Penunjang">Stok Penunjang</option>
                    </select>
                </div>
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1.5">Stok Awal / Sistem</label>
                        <input type="number" step="any" id="inputStok" required class="w-full px-4 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500" placeholder="0">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-700 mb-1.5">Batas Minimum</label>
                        <input type="number" step="any" id="inputMin" required class="w-full px-4 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500" placeholder="0">
                    </div>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-700 mb-1.5">Satuan</label>
                    <input type="text" id="inputSatuan" required class="w-full px-4 py-2.5 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500" placeholder="Contoh: 1 Kg, pcs, Dos">
                </div>
                <div class="flex justify-end gap-3 pt-3">
                    <button type="button" onclick="closeModalBahan()" class="px-5 py-2.5 border border-slate-200 text-slate-700 rounded-2xl text-sm font-medium hover:bg-slate-50 transition">Batal</button>
                    <button type="submit" class="px-5 py-2.5 bg-brand-900 hover:bg-brand-950 text-white rounded-2xl text-sm font-medium shadow">Simpan</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL DETAIL RIWAYAT OPNAME -->
    <div id="modalDetail" class="fixed inset-0 bg-slate-950/60 backdrop-blur-sm hidden items-center justify-center z-50 p-4">
        <div class="bg-white rounded-3xl max-w-2xl w-full p-6 sm:p-8 shadow-2xl border border-slate-200 max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center mb-4">
                <h3 id="modalDetailTitle" class="font-bold text-lg text-slate-900">Detail Laporan Opname</h3>
                <button onclick="closeModalDetail()" class="text-slate-400 hover:text-slate-700 p-1">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div class="overflow-y-auto flex-1 pr-1">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-50 border-b border-slate-200 text-xs text-slate-500 uppercase">
                            <th class="p-3 font-semibold">Item</th>
                            <th class="p-3 font-semibold">Sistem</th>
                            <th class="p-3 font-semibold">Fisik</th>
                            <th class="p-3 font-semibold">Selisih</th>
                            <th class="p-3 font-semibold">Catatan</th>
                        </tr>
                    </thead>
                    <tbody id="tabelDetailBody" class="text-sm divide-y divide-slate-100">
                        <!-- Diisi JS -->
                    </tbody>
                </table>
            </div>
            <div class="mt-6 pt-3 border-t border-slate-200 flex justify-end">
                <button onclick="closeModalDetail()" class="px-5 py-2.5 bg-brand-900 hover:bg-brand-950 text-white rounded-2xl text-sm font-medium shadow">Tutup</button>
            </div>
        </div>
    </div>

    <!-- JavaScript Aplikasi -->
    <script>
        const defaultBahan = [
            { nama: "Beans blend", kategori: "Stok Gudang", stok: 6, min: 1, satuan: "1 Kg" },
            { nama: "Beans natural", kategori: "Stok Gudang", stok: 0, min: 1, satuan: "1 Kg" },
            { nama: "Susu UHT", kategori: "Stok Gudang", stok: 105, min: 10, satuan: "1 Dos" },
            { nama: "Cashew", kategori: "Stok Gudang", stok: 5, min: 1, satuan: "1 Liter" },
            { nama: "Oatside", kategori: "Stok Gudang", stok: 3, min: 1, satuan: "1 Liter" },
            { nama: "Gula Aren", kategori: "Stok Gudang", stok: 8, min: 2, satuan: "1 Kg" },
            { nama: "Krimer", kategori: "Stok Gudang", stok: 4, min: 1, satuan: "1 Kg" },
            { nama: "SKM Omela", kategori: "Stok Gudang", stok: 8, min: 2, satuan: "1 Kaleng" },
            { nama: "Whipcream", kategori: "Stok Gudang", stok: 12, min: 2, satuan: "1 Kg" },
            { nama: "Madu Honey", kategori: "Stok Gudang", stok: 1, min: 1, satuan: "1 Botol" },
            { nama: "Jungle Orange", kategori: "Stok Gudang", stok: 6, min: 1, satuan: "1 Kg" },
            { nama: "Jungle Apel", kategori: "Stok Gudang", stok: 3, min: 1, satuan: "1 Kg" },
            { nama: "Sirup Peach", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Kg" },
            { nama: "Sirup Vanila", kategori: "Stok Gudang", stok: 0, min: 1, satuan: "1 Kg" },
            { nama: "Markisa", kategori: "Stok Gudang", stok: 5, min: 1, satuan: "1 Botol" },
            { nama: "Straw Puree", kategori: "Stok Gudang", stok: 0, min: 1, satuan: "1 Kg" },
            { nama: "Straw Jam selai", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Kg" },
            { nama: "Sprite", kategori: "Stok Gudang", stok: 16, min: 3, satuan: "1 Botol" },
            { nama: "Gula Pasir", kategori: "Stok Gudang", stok: 0, min: 1, satuan: "1 Kg" },
            { nama: "Bubuk Lemontea", kategori: "Stok Gudang", stok: 0, min: 1, satuan: "1 Kg" },
            { nama: "Bubuk Lecitea", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Pcs" },
            { nama: "Bubuk Lemoonade", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Kg" },
            { nama: "Bubuk Coklat", kategori: "Stok Gudang", stok: 3, min: 1, satuan: "1 Kg" },
            { nama: "Bubuk Matcha Tofico", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Kg" },
            { nama: "Bubuk Matcha Arnav", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Kg" },
            { nama: "Buah Jeruk", kategori: "Stok Gudang", stok: 2, min: 1, satuan: "1 Buah" },
            { nama: "Galon Cleo", kategori: "Stok Gudang", stok: 1, min: 1, satuan: "1 Galon" },
            { nama: "Es Batu", kategori: "Stok Gudang", stok: 1, min: 1, satuan: "1 Sak" },
            { nama: "Air vit", kategori: "Stok Gudang", stok: 22, min: 5, satuan: "1 Dos" },
            { nama: "CUP Dingin", kategori: "Stok Gudang", stok: 1759, min: 100, satuan: "pcs" },
            { nama: "Tutup Cup Dingin", kategori: "Stok Gudang", stok: 1875, min: 100, satuan: "pcs" },
            { nama: "CUP Hot", kategori: "Stok Gudang", stok: 270, min: 50, satuan: "pcs" },
            { nama: "Tutup Cup Hot", kategori: "Stok Gudang", stok: 210, min: 50, satuan: "pcs" },
            { nama: "Botol Literan", kategori: "Stok Gudang", stok: 18, min: 5, satuan: "pcs" },
            { nama: "Plastik 1 Cup", kategori: "Stok Penunjang", stok: 7, min: 2, satuan: "pcs" },
            { nama: "Plastik 2 Cup", kategori: "Stok Penunjang", stok: 7, min: 2, satuan: "pcs" },
            { nama: "Plastik u. 20", kategori: "Stok Penunjang", stok: 4, min: 1, satuan: "pcs" },
            { nama: "Plastik u. 30", kategori: "Stok Penunjang", stok: 7, min: 2, satuan: "pcs" },
            { nama: "Clingwrap", kategori: "Stok Penunjang", stok: 0, min: 1, satuan: "pcs" },
            { nama: "Trashbag", kategori: "Stok Penunjang", stok: 4, min: 1, satuan: "pcs" }
        ];

        let currentCabang = document.getElementById('selectCabang').value;
        let dataBahan = [];
        let dataRiwayat = [];

        function loadCabangData() {
            currentCabang = document.getElementById('selectCabang').value;
            const selectEl = document.getElementById('selectCabang');
            const namaCabangText = selectEl.options[selectEl.selectedIndex].text;
            document.getElementById('labelNamaOutlet').innerText = namaCabangText;

            const savedBahan = localStorage.getItem(`kopi7_bahan_${currentCabang}`);
            if (savedBahan) {
                try {
                    dataBahan = JSON.parse(savedBahan);
                    if (!Array.isArray(dataBahan) || dataBahan.length === 0) {
                        dataBahan = JSON.parse(JSON.stringify(defaultBahan));
                    }
                } catch (e) {
                    dataBahan = JSON.parse(JSON.stringify(defaultBahan));
                }
            } else {
                dataBahan = JSON.parse(JSON.stringify(defaultBahan));
                saveData();
            }

            dataRiwayat = JSON.parse(localStorage.getItem(`kopi7_riwayat_${currentCabang}`)) || [];
        }

        function saveData() {
            localStorage.setItem(`kopi7_bahan_${currentCabang}`, JSON.stringify(dataBahan));
            localStorage.setItem(`kopi7_riwayat_${currentCabang}`, JSON.stringify(dataRiwayat));
        }

        function resetDataPaksa() {
            if (confirm("Adakah anda pasti mahu memuat semula data asal (reset) untuk cabang ini?")) {
                dataBahan = JSON.parse(JSON.stringify(defaultBahan));
                dataRiwayat = [];
                saveData();
                const activeTab = document.querySelector('.tab-content:not(.hidden)').id.replace('tab-', '');
                switchTab(activeTab);
                alert("Data berjaya dimuat semula ke senarai asal!");
            }
        }

        function gantiCabang() {
            loadCabangData();
            const activeTab = document.querySelector('.tab-content:not(.hidden)').id.replace('tab-', '');
            switchTab(activeTab);
        }

        document.getElementById('opnameTanggal').valueAsDate = new Date();

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(el => {
                el.classList.remove('bg-brand-900', 'text-white', 'shadow');
                el.classList.add('text-brand-100', 'hover:text-white', 'hover:bg-brand-700/50');
            });

            document.getElementById(`tab-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`nav-${tabId}`);
            activeBtn.classList.remove('text-brand-100', 'hover:bg-brand-700/50');
            activeBtn.classList.add('bg-brand-900', 'text-white', 'shadow');

            if (tabId === 'dashboard') renderDashboard();
            if (tabId === 'stok') renderBahan();
            if (tabId === 'opname') renderOpnameForm();
            if (tabId === 'riwayat') renderRiwayat();

            lucide.createIcons();
        }

        function renderDashboard() {
            const totalBahan = dataBahan.length;
            const menipis = dataBahan.filter(b => b.stok <= b.min);
            
            document.getElementById('statTotalBahan').innerText = totalBahan;
            document.getElementById('statBahanMenipis').innerText = menipis.length;
            document.getElementById('statTotalRiwayat').innerText = dataRiwayat.length;

            const hariIni = new Date().toISOString().split('T')[0];
            const opnameHariIni = dataRiwayat.find(r => r.tanggal === hariIni);
            if (opnameHariIni) {
                document.getElementById('statStatusHariIni').innerText = "Sudah Selesai";
                document.getElementById('statStatusSub').innerText = "Opname hari ini tercatat";
                document.getElementById('statStatusIcon').className = "p-4 bg-emerald-50 text-emerald-600 rounded-2xl";
                document.getElementById('statStatusIcon').innerHTML = '<i data-lucide="check-circle-2" class="w-7 h-7"></i>';
            } else {
                document.getElementById('statStatusHariIni').innerText = "Belum Dilakukan";
                document.getElementById('statStatusSub').innerText = "Segera lakukan pengecekan";
                document.getElementById('statStatusIcon').className = "p-4 bg-rose-50 text-rose-600 rounded-2xl";
                document.getElementById('statStatusIcon').innerHTML = '<i data-lucide="clock" class="w-7 h-7"></i>';
            }

            const tbody = document.getElementById('tabelMenipisBody');
            if (menipis.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="p-6 text-center text-slate-500">Semua stok item di outlet ini aman.</td></tr>`;
            } else {
                tbody.innerHTML = menipis.map(b => `
                    <tr class="border-b border-slate-100 hover:bg-slate-50">
                        <td class="p-4 font-semibold text-slate-900">${b.nama}</td>
                        <td class="p-4"><span class="px-3 py-1 bg-brand-50 text-brand-700 rounded-full text-xs font-medium">${b.kategori}</span></td>
                        <td class="p-4 text-amber-600 font-bold">${b.stok}</td>
                        <td class="p-4 text-slate-700">${b.min}</td>
                        <td class="p-4 text-slate-500">${b.satuan}</td>
                    </tr>
                `).join('');
            }
        }

        function renderBahan() {
            const search = document.getElementById('searchBahan').value.toLowerCase();
            const kategori = document.getElementById('filterKategori').value;
            const tbody = document.getElementById('tabelBahanBody');

            let filtered = dataBahan.filter(b => {
                const matchSearch = b.nama.toLowerCase().includes(search);
                const matchKat = kategori === "" || b.kategori === kategori;
                return matchSearch && matchKat;
            });

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="p-6 text-center text-slate-500">Tidak ada data item ditemukan.</td></tr>`;
                return;
            }

            tbody.innerHTML = filtered.map((b, index) => {
                const realIndex = dataBahan.indexOf(b);
                const isLow = b.stok <= b.min;
                return `
                    <tr class="hover:bg-slate-50 transition">
                        <td class="p-4 font-semibold text-slate-900">${b.nama}</td>
                        <td class="p-4"><span class="px-3 py-1 bg-brand-50 text-brand-700 rounded-full text-xs font-medium">${b.kategori}</span></td>
                        <td class="p-4 font-bold ${isLow ? 'text-amber-600' : 'text-slate-900'}">
                            ${b.stok} ${isLow ? '<span class="text-[10px] bg-amber-100 text-amber-800 px-2 py-0.5 rounded-full ml-1 font-semibold">Menipis</span>' : ''}
                        </td>
                        <td class="p-4 text-slate-700">${b.min}</td>
                        <td class="p-4 text-slate-500">${b.satuan}</td>
                        <td class="p-4 text-center">
                            <button onclick="editBahan(${realIndex})" class="p-2 bg-brand-50 text-brand-700 hover:bg-brand-100 rounded-xl mr-1.5 transition" title="Edit"><i data-lucide="edit-3" class="w-4 h-4"></i></button>
                            <button onclick="hapusBahan(${realIndex})" class="p-2 bg-rose-50 text-rose-600 hover:bg-rose-100 rounded-xl transition" title="Hapus"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                        </td>
                    </tr>
                `;
            }).join('');
            lucide.createIcons();
        }

        function openModalBahan(index = null) {
            document.getElementById('modalBahan').classList.remove('hidden');
            document.getElementById('modalBahan').classList.add('flex');
            if (index !== null) {
                document.getElementById('modalBahanTitle').innerText = "Edit Item";
                document.getElementById('bahanIndex').value = index;
                document.getElementById('inputNama').value = dataBahan[index].nama;
                document.getElementById('inputKategori').value = dataBahan[index].kategori;
                document.getElementById('inputStok').value = dataBahan[index].stok;
                document.getElementById('inputMin').value = dataBahan[index].min;
                document.getElementById('inputSatuan').value = dataBahan[index].satuan;
            } else {
                document.getElementById('modalBahanTitle').innerText = "Tambah Item Baru";
                document.getElementById('formBahan').reset();
                document.getElementById('bahanIndex').value = "";
            }
            lucide.createIcons();
        }

        function closeModalBahan() {
            document.getElementById('modalBahan').classList.remove('flex');
            document.getElementById('modalBahan').classList.add('hidden');
        }

        function saveBahan(e) {
            e.preventDefault();
            const index = document.getElementById('bahanIndex').value;
            const item = {
                nama: document.getElementById('inputNama').value,
                kategori: document.getElementById('inputKategori').value,
                stok: parseFloat(document.getElementById('inputStok').value),
                min: parseFloat(document.getElementById('inputMin').value),
                satuan: document.getElementById('inputSatuan').value
            };

            if (index === "") {
                dataBahan.push(item);
            } else {
                dataBahan[index] = item;
            }

            saveData();
            closeModalBahan();
            renderBahan();
        }

        function editBahan(index) {
            openModalBahan(index);
        }

        function hapusBahan(index) {
            if (confirm(`Yakin ingin menghapus item "${dataBahan[index].nama}"?`)) {
                dataBahan.splice(index, 1);
                saveData();
                renderBahan();
            }
        }

        function renderOpnameForm() {
            const tbody = document.getElementById('tabelOpnameBody');
            if (dataBahan.length === 0) {
                tbody.innerHTML = `<tr><td colspan="6" class="p-6 text-center text-slate-500">Belum ada data item di outlet ini. Silakan tambahkan terlebih dahulu.</td></tr>`;
                return;
            }

            tbody.innerHTML = dataBahan.map((b, i) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-4 font-semibold text-slate-900">${b.nama}</td>
                    <td class="p-4"><span class="px-3 py-1 bg-brand-50 text-brand-700 rounded-full text-xs font-medium">${b.kategori}</span></td>
                    <td class="p-4 font-semibold text-slate-700">${b.stok} ${b.satuan}</td>
                    <td class="p-4">
                        <input type="number" step="any" id="fisik_${i}" value="${b.stok}" oninput="hitungSelisih(${i}, ${b.stok})" class="w-32 px-3.5 py-2 border border-slate-200 rounded-2xl text-sm font-semibold bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500">
                    </td>
                    <td class="p-4 font-bold" id="selisih_text_${i}">0 ${b.satuan}</td>
                    <td class="p-4">
                        <input type="text" id="catatan_${i}" placeholder="Keterangan (opsional)" class="w-full px-3.5 py-2 border border-slate-200 rounded-2xl text-sm bg-slate-50 focus:outline-none focus:ring-2 focus:ring-brand-500">
                    </td>
                </tr>
            `).join('');
        }

        function hitungSelisih(index, stokSistem) {
            const fisikVal = parseFloat(document.getElementById(`fisik_${index}`).value) || 0;
            const selisih = fisikVal - stokSistem;
            const el = document.getElementById(`selisih_text_${index}`);
            const satuan = dataBahan[index].satuan;

            if (selisih === 0) {
                el.className = "p-4 font-bold text-emerald-600";
                el.innerText = `0 ${satuan} (Sesuai)`;
            } else if (selisih > 0) {
                el.className = "p-4 font-bold text-brand-600";
                el.innerText = `+${selisih} ${satuan} (Lebih)`;
            } else {
                el.className = "p-4 font-bold text-rose-600";
                el.innerText = `${selisih} ${satuan} (Kurang)`;
            }
        }

        function simpanOpname() {
            const tanggal = document.getElementById('opnameTanggal').value;
            if (!tanggal) {
                alert("Silakan pilih tanggal opname terlebih dahulu.");
                return;
            }

            let detailOpname = [];
            let adaSelisihCount = 0;

            dataBahan.forEach((b, i) => {
                const fisikVal = parseFloat(document.getElementById(`fisik_${i}`).value) || 0;
                const selisih = fisikVal - b.stok;
                const catatan = document.getElementById(`catatan_${i}`).value || "-";

                if (selisih !== 0) adaSelisihCount++;

                detailOpname.push({
                    nama: b.nama,
                    kategori: b.kategori,
                    stokSistem: b.stok,
                    stokFisik: fisikVal,
                    selisih: selisih,
                    satuan: b.satuan,
                    catatan: catatan
                });

                b.stok = fisikVal;
            });

            const laporanBaru = {
                id: Date.now(),
                tanggal: tanggal,
                totalBahan: dataBahan.length,
                jumlahSelisih: adaSelisihCount,
                detail: detailOpname
            };

            dataRiwayat = dataRiwayat.filter(r => r.tanggal !== tanggal);
            dataRiwayat.unshift(laporanBaru);

            saveData();
            alert("Berhasil! Stok opname harian outlet ini telah disimpan.");
            switchTab('riwayat');
        }

        function renderRiwayat() {
            const tbody = document.getElementById('tabelRiwayatBody');
            if (dataRiwayat.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" class="p-6 text-center text-slate-500">Belum ada riwayat laporan opname di outlet ini.</td></tr>`;
                return;
            }

            tbody.innerHTML = dataRiwayat.map((r, index) => `
                <tr class="hover:bg-slate-50 transition">
                    <td class="p-4 font-semibold text-slate-900">${r.tanggal}</td>
                    <td class="p-4">${r.totalBahan} Jenis Item</td>
                    <td class="p-4">
                        <span class="px-3 py-1 rounded-full text-xs font-semibold ${r.jumlahSelisih > 0 ? 'bg-amber-50 text-amber-700' : 'bg-emerald-50 text-emerald-700'}">
                            ${r.jumlahSelisih} item berselisih
                        </span>
                    </td>
                    <td class="p-4 text-center">
                        <button onclick="bukaDetailRiwayat(${index})" class="px-4 py-2 bg-brand-50 text-brand-700 hover:bg-brand-100 rounded-xl text-xs font-medium transition inline-flex items-center gap-1.5">
                            <i data-lucide="eye" class="w-3.5 h-3.5"></i> Lihat Detail
                        </button>
                    </td>
                </tr>
            `).join('');
            lucide.createIcons();
        }

        function bukaDetailRiwayat(index) {
            const item = dataRiwayat[index];
            document.getElementById('modalDetailTitle').innerText = `Laporan Opname - ${item.tanggal}`;
            const tbody = document.getElementById('tabelDetailBody');

            tbody.innerHTML = item.detail.map(d => `
                <tr class="border-b border-slate-100">
                    <td class="p-3 font-semibold text-slate-900">${d.nama}</td>
                    <td class="p-3 text-slate-700">${d.stokSistem} ${d.satuan}</td>
                    <td class="p-3 font-semibold">${d.stokFisik} ${d.satuan}</td>
                    <td class="p-3 font-bold ${d.selisih < 0 ? 'text-rose-600' : d.selisih > 0 ? 'text-brand-600' : 'text-emerald-600'}">
                        ${d.selisih > 0 ? '+' + d.selisih : d.selisih} ${d.satuan}
                    </td>
                    <td class="p-3 text-xs text-slate-500">${d.catatan}</td>
                </tr>
            `).join('');

            document.getElementById('modalDetail').classList.remove('hidden');
            document.getElementById('modalDetail').classList.add('flex');
        }

        function closeModalDetail() {
            document.getElementById('modalDetail').classList.remove('flex');
            document.getElementById('modalDetail').classList.add('hidden');
        }

        function exportData() {
            const backupObj = { cabang: currentCabang, bahan: dataBahan, riwayat: dataRiwayat };
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(backupObj, null, 2));
            const dlAnchor = document.createElement('a');
            dlAnchor.setAttribute("href", dataStr);
            dlAnchor.setAttribute("download", `backup_stok_kopi7_${currentCabang}_${new Date().toISOString().split('T')[0]}.json`);
            document.body.appendChild(dlAnchor);
            dlAnchor.click();
            dlAnchor.remove();
        }

        function importData(event) {
            const fileReader = new FileReader();
            if (event.target.files[0]) {
                fileReader.readAsText(event.target.files[0], "UTF-8");
                fileReader.onload = e => {
                    try {
                        const parsed = JSON.parse(e.target.result);
                        if (parsed.bahan && parsed.riwayat) {
                            dataBahan = parsed.bahan;
                            dataRiwayat = parsed.riwayat;
                            saveData();
                            renderDashboard();
                            alert(`Data untuk outlet ini berhasil dipulihkan!`);
                        } else {
                            alert("Format file backup tidak valid.");
                        }
                    } catch (err) {
                        alert("Gagal membaca file JSON.");
                    }
                };
            }
        }

        loadCabangData();
        switchTab('dashboard');
    </script>
</body>
</html>
