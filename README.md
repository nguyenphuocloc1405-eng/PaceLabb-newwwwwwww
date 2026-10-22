<!DOCTYPE html>
<html lang="vi" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PaceLab Pro - Running Hub & Training Dashboard</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#fff7ed',
                            100: '#ffedd5',
                            500: '#f97316',
                            600: '#ea580c',
                            700: '#c2410c',
                        },
                        darkbg: '#0f172a',
                        darkcard: '#1e293b',
                        darkborder: '#334155'
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0b0f19;
            color: #f3f4f6;
        }
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0f172a;
        }
        ::-webkit-scrollbar-thumb {
            background: #334155;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #f97316;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col antialiased selection:bg-orange-500 selection:text-white">

    <!-- Top Navigation Header -->
    <header class="sticky top-0 z-40 bg-slate-900/90 backdrop-blur-md border-b border-slate-800">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="switchTab('dashboard')">
                    <div class="p-2 bg-gradient-to-tr from-orange-600 to-amber-500 rounded-xl shadow-lg shadow-orange-500/20 text-white">
                        <i data-lucide="zap" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <span class="text-xl font-extrabold tracking-tight bg-gradient-to-r from-orange-400 to-amber-300 bg-clip-text text-transparent">PaceLab Pro</span>
                        <span class="text-xs text-slate-400 block -mt-1 font-medium">Running Hub & Calendar</span>
                    </div>
                </div>

                <!-- Desktop Nav Tabs -->
                <nav class="hidden md:flex space-x-1 lg:space-x-2">
                    <button onclick="switchTab('dashboard')" id="nav-dashboard" class="nav-btn active flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all">
                        <i data-lucide="layout-dashboard" class="w-4 h-4"></i>
                        <span>Tổng Quan</span>
                    </button>
                    <button onclick="switchTab('calendar')" id="nav-calendar" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="calendar-days" class="w-4 h-4"></i>
                        <span>Lịch & Nhật Ký</span>
                    </button>
                    <button onclick="switchTab('plans')" id="nav-plans" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="file-spreadsheet" class="w-4 h-4"></i>
                        <span>Giáo Án</span>
                    </button>
                    <button onclick="switchTab('shoes')" id="nav-shoes" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="footprints" class="w-4 h-4"></i>
                        <span>Tủ Giày</span>
                    </button>
                    <button onclick="switchTab('workout')" id="nav-workout" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="dumbbell" class="w-4 h-4"></i>
                        <span>Bổ Trợ</span>
                    </button>
                    <button onclick="switchTab('nutrition')" id="nav-nutrition" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="utensils" class="w-4 h-4"></i>
                        <span>Dinh Dưỡng</span>
                    </button>
                    <button onclick="switchTab('calculator')" id="nav-calculator" class="nav-btn flex items-center space-x-2 px-3 py-2 rounded-lg text-sm font-semibold transition-all text-slate-400 hover:text-slate-200 hover:bg-slate-800">
                        <i data-lucide="calculator" class="w-4 h-4"></i>
                        <span>Tính Pace</span>
                    </button>
                </nav>

                <!-- Action & Backup Buttons -->
                <div class="flex items-center gap-2">
                    <button onclick="exportData()" title="Xuất dữ liệu dự phòng" class="hidden sm:flex items-center p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-lg text-xs transition">
                        <i data-lucide="download" class="w-4 h-4"></i>
                    </button>
                    <label title="Nhập dữ liệu dự phòng" class="hidden sm:flex items-center p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 rounded-lg text-xs cursor-pointer transition">
                        <i data-lucide="upload" class="w-4 h-4"></i>
                        <input type="file" id="import-file-input" onchange="importData(event)" class="hidden" accept=".json">
                    </label>
                    <button onclick="openLogRunModal()" class="flex items-center space-x-2 bg-orange-600 hover:bg-orange-500 text-white font-medium px-4 py-2 rounded-lg text-sm transition shadow-lg shadow-orange-600/30">
                        <i data-lucide="plus-circle" class="w-4 h-4"></i>
                        <span class="hidden sm:inline">Ghi Chép Run</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Bottom Nav -->
        <div class="md:hidden flex justify-around border-t border-slate-800 bg-slate-900 py-2 px-1 text-[11px]">
            <button onclick="switchTab('dashboard')" id="m-nav-dashboard" class="flex flex-col items-center py-1 text-orange-500">
                <i data-lucide="layout-dashboard" class="w-4 h-4"></i>
                <span>Tổng quan</span>
            </button>
            <button onclick="switchTab('calendar')" id="m-nav-calendar" class="flex flex-col items-center py-1 text-slate-400">
                <i data-lucide="calendar-days" class="w-4 h-4"></i>
                <span>Lịch Run</span>
            </button>
            <button onclick="switchTab('plans')" id="m-nav-plans" class="flex flex-col items-center py-1 text-slate-400">
                <i data-lucide="file-spreadsheet" class="w-4 h-4"></i>
                <span>Giáo án</span>
            </button>
            <button onclick="switchTab('shoes')" id="m-nav-shoes" class="flex flex-col items-center py-1 text-slate-400">
                <i data-lucide="footprints" class="w-4 h-4"></i>
                <span>Tủ giày</span>
            </button>
            <button onclick="switchTab('workout')" id="m-nav-workout" class="flex flex-col items-center py-1 text-slate-400">
                <i data-lucide="dumbbell" class="w-4 h-4"></i>
                <span>Bổ trợ</span>
            </button>
            <button onclick="switchTab('calculator')" id="m-nav-calculator" class="flex flex-col items-center py-1 text-slate-400">
                <i data-lucide="calculator" class="w-4 h-4"></i>
                <span>Tính pace</span>
            </button>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6">

        <!-- ================= PAGE 1: DASHBOARD ================= -->
        <section id="tab-dashboard" class="tab-content space-y-6">
            <!-- Hero Goal Display -->
            <div class="relative overflow-hidden rounded-2xl bg-gradient-to-r from-slate-900 via-slate-800 to-orange-950/40 p-6 md:p-8 border border-slate-800 shadow-xl">
                <div class="relative z-10 flex flex-col md:flex-row md:items-center justify-between gap-6">
                    <div>
                        <div class="inline-flex items-center space-x-2 bg-orange-500/10 border border-orange-500/30 text-orange-400 px-3 py-1 rounded-full text-xs font-semibold uppercase tracking-wider mb-3">
                            <i data-lucide="target" class="w-3.5 h-3.5"></i>
                            <span>Mục tiêu hiện tại</span>
                        </div>
                        <h1 class="text-2xl md:text-4xl font-black tracking-tight text-white flex items-center gap-3">
                            Chinh Phục <span id="dash-goal-title" class="text-orange-500">5K Sub25</span>
                        </h1>
                        <p class="text-slate-400 text-sm md:text-base mt-2 max-w-xl">
                            Pace mục tiêu: <span id="dash-goal-pace" class="font-bold text-white">4:59 min/km</span>. 
                            Kiên trì tích lũy khối lượng nhẹ và tập luyện đúng ngưỡng lactate để chạm đích an toàn!
                        </p>
                    </div>

                    <!-- Target / Custom Plan Selector -->
                    <div class="bg-slate-900/90 border border-slate-700/80 rounded-xl p-4 shadow-lg flex flex-col gap-3 min-w-[260px]">
                        <label class="text-xs font-semibold text-slate-400 uppercase tracking-wider">Đổi giáo án / mục tiêu:</label>
                        <select id="goal-select" onchange="changeGoal(this.value)" class="bg-slate-800 text-white font-bold text-sm border border-slate-700 rounded-lg px-3 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                            <optgroup label="Tự Tạo (Custom Plans)" id="custom-plans-optgroup">
                                <!-- Populated dynamically -->
                            </optgroup>
                            <optgroup label="Giáo Án Cự Ly 5KM">
                                <option value="5k-sub25" selected>5K - Sub 25 min (Pace 4:59)</option>
                                <option value="5k-sub24">5K - Sub 24 min (Pace 4:47)</option>
                                <option value="5k-sub23">5K - Sub 23 min (Pace 4:35)</option>
                                <option value="5k-sub22">5K - Sub 22 min (Pace 4:23)</option>
                                <option value="5k-sub21">5K - Sub 21 min (Pace 4:11)</option>
                                <option value="5k-sub20">5K - Sub 20 min (Pace 3:59)</option>
                                <option value="5k-sub19">5K - Sub 19 min (Pace 3:47)</option>
                                <option value="5k-sub18">5K - Sub 18 min (Pace 3:35)</option>
                                <option value="5k-sub17">5K - Sub 17 min (Pace 3:23)</option>
                            </optgroup>
                            <optgroup label="Giáo Án Cự Ly 10KM">
                                <option value="10k-sub60">10K - Sub 60 min (Pace 5:59)</option>
                                <option value="10k-sub55">10K - Sub 55 min (Pace 5:29)</option>
                                <option value="10k-sub50">10K - Sub 50 min (Pace 4:59)</option>
                                <option value="10k-sub45">10K - Sub 45 min (Pace 4:29)</option>
                                <option value="10k-sub40">10K - Sub 40 min (Pace 3:59)</option>
                            </optgroup>
                        </select>
                        <button onclick="switchTab('plans')" class="w-full bg-slate-800 hover:bg-slate-700 text-orange-400 font-semibold text-xs py-2 rounded-lg border border-slate-700 transition flex items-center justify-center gap-2">
                            <i data-lucide="edit-3" class="w-4 h-4"></i> Xem / Tự Tạo Giáo Án
                        </button>
                    </div>
                </div>
            </div>

            <!-- Dashboard Mileage Breakdown Stat Cards -->
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
                <div class="bg-slate-800/80 border border-slate-700/60 rounded-xl p-4 sm:p-5">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase">Tuần Này</span>
                        <div class="p-2 bg-orange-500/10 text-orange-400 rounded-lg"><i data-lucide="calendar" class="w-4 h-4"></i></div>
                    </div>
                    <h3 class="text-xl sm:text-2xl font-black text-white" id="stat-week-km">0.0 km</h3>
                    <p class="text-[11px] text-slate-500 mt-1">7 ngày qua</p>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 rounded-xl p-4 sm:p-5">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase">Tháng Này</span>
                        <div class="p-2 bg-blue-500/10 text-blue-400 rounded-lg"><i data-lucide="trending-up" class="w-4 h-4"></i></div>
                    </div>
                    <h3 class="text-xl sm:text-2xl font-black text-white" id="stat-month-km">0.0 km</h3>
                    <p class="text-[11px] text-slate-500 mt-1">Trong tháng hiện tại</p>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 rounded-xl p-4 sm:p-5">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase">Năm Này</span>
                        <div class="p-2 bg-purple-500/10 text-purple-400 rounded-lg"><i data-lucide="award" class="w-4 h-4"></i></div>
                    </div>
                    <h3 class="text-xl sm:text-2xl font-black text-white" id="stat-year-km">0.0 km</h3>
                    <p class="text-[11px] text-slate-500 mt-1">Năm <span id="current-year-txt">2026</span></p>
                </div>

                <div class="bg-slate-800/80 border border-slate-700/60 rounded-xl p-4 sm:p-5">
                    <div class="flex items-center justify-between mb-2">
                        <span class="text-xs font-semibold text-slate-400 uppercase">Tất Cả Thời Gian</span>
                        <div class="p-2 bg-emerald-500/10 text-emerald-400 rounded-lg"><i data-lucide="activity" class="w-4 h-4"></i></div>
                    </div>
                    <h3 class="text-xl sm:text-2xl font-black text-white" id="stat-total-km">0.0 km</h3>
                    <p class="text-[11px] text-slate-500 mt-1">Tổng dồn lịch sử</p>
                </div>
            </div>

            <!-- Active Shoe & Recent Activity Logs -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                <!-- Recent Logs list (2 cols) -->
                <div class="lg:col-span-2 bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                    <div class="flex items-center justify-between mb-4">
                        <h2 class="text-lg font-bold text-white flex items-center gap-2">
                            <i data-lucide="history" class="w-5 h-5 text-orange-400"></i>
                            Nhật Ký Buổi Chạy Gần Đây
                        </h2>
                        <button onclick="switchTab('calendar')" class="text-xs text-orange-400 hover:text-orange-300 font-semibold flex items-center gap-1">
                            Xem tất cả trên Lịch →
                        </button>
                    </div>

                    <div id="recent-logs-list" class="space-y-3">
                        <!-- Rendered dynamically -->
                    </div>
                </div>

                <!-- Shoe Status Brief -->
                <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-4">
                            <h2 class="text-lg font-bold text-white flex items-center gap-2">
                                <i data-lucide="footprints" class="w-5 h-5 text-orange-400"></i>
                                Giày Chạy Chính
                            </h2>
                            <button onclick="switchTab('shoes')" class="text-xs text-orange-400 hover:underline font-medium">Tủ giày</button>
                        </div>
                        
                        <div id="dash-active-shoe-card" class="bg-slate-900/80 p-4 rounded-xl border border-slate-700">
                            <!-- Rendered dynamically -->
                        </div>
                    </div>

                    <div class="mt-6 p-4 rounded-xl bg-orange-500/10 border border-orange-500/20">
                        <p class="text-xs text-orange-300 font-medium">
                            💡 Gợi ý: Mỗi bài chạy đều tự động tính cộng dồn số km trực tiếp vào đôi giày bạn chọn sử dụng!
                        </p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= PAGE 2: INTERACTIVE CALENDAR & HISTORY ================= -->
        <section id="tab-calendar" class="tab-content hidden space-y-6">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <div>
                    <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                        <i data-lucide="calendar-days" class="w-6 h-6 text-orange-500"></i>
                        Lịch Tập & Nhật Ký Bài Chạy
                    </h1>
                    <p class="text-slate-400 text-sm mt-1">Xem khối lượng vận động theo Tuần / Tháng / Năm và quản lý, chỉnh sửa chi tiết nhật ký chạy cũ.</p>
                </div>
                <div class="flex items-center gap-2 bg-slate-900 p-1 rounded-xl border border-slate-700">
                    <button onclick="setCalendarView('week')" id="btn-view-week" class="px-3 py-1.5 rounded-lg text-xs font-semibold transition bg-orange-600 text-white">Tuần</button>
                    <button onclick="setCalendarView('month')" id="btn-view-month" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-400 hover:text-white transition">Tháng</button>
                    <button onclick="setCalendarView('year')" id="btn-view-year" class="px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-400 hover:text-white transition">Năm</button>
                </div>
            </div>

            <!-- Calendar Dynamic Container -->
            <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                <!-- Navigation controls for date -->
                <div class="flex items-center justify-between mb-6">
                    <button onclick="navigateCalendar(-1)" class="p-2 bg-slate-700 hover:bg-slate-600 rounded-lg text-white transition">
                        <i data-lucide="chevron-left" class="w-5 h-5"></i>
                    </button>
                    <h2 class="text-lg font-bold text-white" id="calendar-period-label">Tuần này</h2>
                    <button onclick="navigateCalendar(1)" class="p-2 bg-slate-700 hover:bg-slate-600 rounded-lg text-white transition">
                        <i data-lucide="chevron-right" class="w-5 h-5"></i>
                    </button>
                </div>

                <!-- Interactive Calendar View Render -->
                <div id="calendar-view-container">
                    <!-- Dynamic Grid / Chart -->
                </div>
            </div>

            <!-- Run History Table / List with Search & Filter -->
            <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 mb-6">
                    <h2 class="text-lg font-bold text-white flex items-center gap-2">
                        <i data-lucide="list-checks" class="w-5 h-5 text-orange-400"></i>
                        Lịch Sử Nhật Ký Toàn Bộ Buổi Chạy
                    </h2>
                    <div class="flex items-center gap-3 w-full sm:w-auto">
                        <input type="text" id="log-search-input" onkeyup="renderLogHistory()" placeholder="Tìm kiếm ghi chú, giày..." class="bg-slate-900 border border-slate-700 text-white text-xs rounded-xl px-3 py-2 w-full sm:w-48 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                        <button onclick="openLogRunModal()" class="shrink-0 bg-orange-600 hover:bg-orange-500 text-white font-semibold text-xs px-3 py-2 rounded-xl transition flex items-center gap-1">
                            <i data-lucide="plus" class="w-4 h-4"></i> Thêm bài
                        </button>
                    </div>
                </div>

                <div class="overflow-x-auto">
                    <table class="w-full text-left text-sm text-slate-300">
                        <thead class="bg-slate-900/80 text-xs uppercase font-semibold text-slate-400">
                            <tr>
                                <th class="p-3">Ngày</th>
                                <th class="p-3">Cự Ly (KM)</th>
                                <th class="p-3">Pace</th>
                                <th class="p-3">Loại Bài</th>
                                <th class="p-3">Giày Chạy</th>
                                <th class="p-3">Ghi Chú</th>
                                <th class="p-3 text-right">Thao Tác</th>
                            </tr>
                        </thead>
                        <tbody id="log-history-tbody" class="divide-y divide-slate-800">
                            <!-- Populated dynamically -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- ================= PAGE 3: TRAINING PLANS & CUSTOM BUILDER ================= -->
        <section id="tab-plans" class="tab-content hidden space-y-6">
            <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <div>
                    <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                        <i data-lucide="file-spreadsheet" class="w-6 h-6 text-orange-500"></i>
                        Giáo Án Tập Luyện & Tự Đề Ra
                    </h1>
                    <p class="text-slate-400 text-sm mt-1">Sử dụng giáo án chuẩn hóa có sẵn hoặc tự biên soạn giáo án cá nhân riêng phù hợp lịch trình của bạn!</p>
                </div>
                <div class="flex items-center gap-3">
                    <button onclick="openCustomPlanModal()" class="bg-orange-600 hover:bg-orange-500 text-white font-semibold px-4 py-2.5 rounded-xl text-xs transition flex items-center gap-2 shadow-lg shadow-orange-600/30">
                        <i data-lucide="plus" class="w-4 h-4"></i> Tự Tạo Giáo Án Mới
                    </button>
                </div>
            </div>

            <!-- Target Pace Reference Cards -->
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                <div class="bg-slate-800/40 border border-blue-500/30 rounded-xl p-4">
                    <span class="text-xs font-bold text-blue-400 uppercase tracking-wider">Easy Pace (Chạy Nhẹ)</span>
                    <p class="text-xl font-extrabold text-white mt-1" id="zone-easy-pace">6:15 - 6:45 /km</p>
                    <p class="text-xs text-slate-400 mt-1">Tích lũy hiếu khí, tim dưới zone 2-3.</p>
                </div>
                <div class="bg-slate-800/40 border border-amber-500/30 rounded-xl p-4">
                    <span class="text-xs font-bold text-amber-400 uppercase tracking-wider">Tempo Pace (Lactate)</span>
                    <p class="text-xl font-extrabold text-white mt-1" id="zone-tempo-pace">5:05 - 5:15 /km</p>
                    <p class="text-xs text-slate-400 mt-1">Ngưỡng chịu đựng mỏi cơ liên tục.</p>
                </div>
                <div class="bg-slate-800/40 border border-red-500/30 rounded-xl p-4">
                    <span class="text-xs font-bold text-red-400 uppercase tracking-wider">Interval Pace (Tốc Độ)</span>
                    <p class="text-xl font-extrabold text-white mt-1" id="zone-interval-pace">4:35 - 4:45 /km</p>
                    <p class="text-xs text-slate-400 mt-1">Biến tốc ngắn gia tăng VO2Max.</p>
                </div>
            </div>

            <!-- Active Plan Container -->
            <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                <div class="flex items-center justify-between mb-4">
                    <h2 class="text-lg font-bold text-white">Lịch Tập Trong Tuần: <span id="plan-target-name" class="text-orange-400">5K Sub25</span></h2>
                    <div id="custom-plan-actions" class="hidden flex items-center gap-2">
                        <button onclick="editCurrentCustomPlan()" class="text-xs text-orange-400 hover:underline">Sửa giáo án này</button>
                        <span class="text-slate-600">|</span>
                        <button onclick="deleteCurrentCustomPlan()" class="text-xs text-red-400 hover:underline">Xóa giáo án này</button>
                    </div>
                </div>

                <div class="space-y-4" id="weekly-schedule-container">
                    <!-- Rendered dynamically -->
                </div>
            </div>
        </section>

        <!-- ================= PAGE 4: ADVANCED SHOE TRACKER ================= -->
        <section id="tab-shoes" class="tab-content hidden space-y-6">
            <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <div>
                    <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                        <i data-lucide="footprints" class="w-6 h-6 text-orange-500"></i>
                        Quản Lý & Theo Dõi Tủ Giày
                    </h1>
                    <p class="text-slate-400 text-sm mt-1">Số km tự động cập nhật chính xác từ các buổi chạy. Thay giày kịp thời giúp phòng tránh chấn thương khớp gối & cổ chân!</p>
                </div>
                <button onclick="openAddShoeModal()" class="bg-orange-600 hover:bg-orange-500 text-white font-semibold px-4 py-2.5 rounded-xl text-sm transition flex items-center gap-2 shadow-lg shadow-orange-600/30">
                    <i data-lucide="plus" class="w-4 h-4"></i> Thêm Đôi Giày Mới
                </button>
            </div>

            <!-- Shoes Grid -->
            <div id="shoes-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Rendered dynamically -->
            </div>
        </section>

        <!-- ================= PAGE 5: BODYWEIGHT WORKOUT & MOBILITY ================= -->
        <section id="tab-workout" class="tab-content hidden space-y-6">
            <div class="bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="dumbbell" class="w-6 h-6 text-orange-500"></i>
                    Bổ Trợ Sức Mạnh & Cổ Khớp Tại Nhà
                </h1>
                <p class="text-slate-400 text-sm mt-1">Không cần dụng cụ! Dành riêng cho runner sinh viên giúp cải thiện guồng chân (Cadence), khớp dẻo dai và tránh chấn thương.</p>
            </div>

            <!-- Interactive Interval Timer -->
            <div class="bg-gradient-to-br from-slate-900 to-slate-800 border border-slate-700/80 rounded-2xl p-6 text-center max-w-xl mx-auto shadow-2xl">
                <h3 class="text-xs font-bold text-orange-400 uppercase tracking-widest mb-2">Đồng Hồ Đếm Giờ Tập Luyện</h3>
                <div class="text-5xl font-black text-white my-3 tracking-mono" id="timer-display">00:45</div>
                <p id="timer-status" class="text-sm text-slate-400 mb-4 font-medium">Sẵn sàng tập (45s tập / 15s nghỉ)</p>
                <div class="flex justify-center gap-3">
                    <button onclick="startTimer(45)" class="bg-emerald-600 hover:bg-emerald-500 text-white font-semibold px-5 py-2.5 rounded-xl text-sm transition flex items-center gap-2">
                        <i data-lucide="play" class="w-4 h-4"></i> Bắt đầu 45s
                    </button>
                    <button onclick="resetTimer()" class="bg-slate-700 hover:bg-slate-600 text-white font-semibold px-4 py-2.5 rounded-xl text-sm transition flex items-center gap-2">
                        <i data-lucide="rotate-ccw" class="w-4 h-4"></i> Đặt lại
                    </button>
                </div>
            </div>

            <!-- Exercises Lists -->
            <div class="space-y-8">
                <div>
                    <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2 border-b border-slate-800 pb-2">
                        <i data-lucide="zap" class="w-5 h-5 text-orange-400"></i>
                        1. Bài Tập Sức Mạnh Chân & Mông (Lower Body)
                    </h2>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="exercises-strength"></div>
                </div>

                <div>
                    <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2 border-b border-slate-800 pb-2">
                        <i data-lucide="shield" class="w-5 h-5 text-amber-400"></i>
                        2. Bài Tập Cơ Core Giữ Form Chạy
                    </h2>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="exercises-core"></div>
                </div>

                <div>
                    <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2 border-b border-slate-800 pb-2">
                        <i data-lucide="activity" class="w-5 h-5 text-emerald-400"></i>
                        3. Bài Tập Linh Hoạt Cổ Chân & Háng (Mobility)
                    </h2>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="exercises-mobility"></div>
                </div>
            </div>
        </section>

        <!-- ================= PAGE 6: STUDENT NUTRITION & BUDGET ================= -->
        <section id="tab-nutrition" class="tab-content hidden space-y-6">
            <div class="bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="utensils" class="w-6 h-6 text-orange-500"></i>
                    Dinh Dưỡng Tiết Kiệm Cho Sinh Viên Run
                </h1>
                <p class="text-slate-400 text-sm mt-1">Ăn đồ ngoài vẫn đủ Đạm (Protein) và Tinh Bột (Carb) để đập tan mốc sub5k/10k mà không "cháy túi".</p>
            </div>

            <!-- Protein & Carb Cards -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                    <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2 text-orange-400">
                        <i data-lucide="beef" class="w-5 h-5"></i> Nguồn Đạm (Protein) Rẻ & Dễ Kiếm
                    </h2>
                    <div class="space-y-4">
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Trứng Gà Luộc</h3>
                                <p class="text-xs text-slate-400">~3.000đ / quả | ~6g Protein</p>
                                <p class="text-xs text-slate-500 mt-1">Luộc sẵn 2 quả bổ sung vào dĩa cơm tiệm.</p>
                            </div>
                            <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-full">Siêu Rẻ</span>
                        </div>
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Đậu Hũ (Đậu Phụ)</h3>
                                <p class="text-xs text-slate-400">~5.000đ / bìa | ~10g Protein</p>
                                <p class="text-xs text-slate-500 mt-1">Đạm thực vật lành tính, dễ mua ở mọi chợ.</p>
                            </div>
                            <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-full">Tốt</span>
                        </div>
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Sữa Đậu Nành / Sữa Tươi</h3>
                                <p class="text-xs text-slate-400">~7.000đ - 10.000đ | ~7g Protein</p>
                                <p class="text-xs text-slate-500 mt-1">Uống ngay sau khi chạy 30 phút để phục hồi cơ.</p>
                            </div>
                            <span class="text-xs font-bold text-blue-400 bg-blue-500/10 px-2.5 py-1 rounded-full">Nhanh gọn</span>
                        </div>
                    </div>
                </div>

                <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6">
                    <h2 class="text-lg font-bold text-white mb-4 flex items-center gap-2 text-amber-400">
                        <i data-lucide="wheat" class="w-5 h-5"></i> Nạp Tinh Bột & Năng Lượng Chạy
                    </h2>
                    <div class="space-y-4">
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Chuối Chín</h3>
                                <p class="text-xs text-slate-400">~3.000đ / quả | Giàu Kali & Carb</p>
                                <p class="text-xs text-slate-500 mt-1">Ăn 1 quả trước chạy 30-45 phút phòng chuột rút.</p>
                            </div>
                            <span class="text-xs font-bold text-amber-400 bg-amber-500/10 px-2.5 py-1 rounded-full">Trước Run</span>
                        </div>
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Yến Mạch Cán Mỏng</h3>
                                <p class="text-xs text-slate-400">~50.000đ / bịch 800g (dùng 2 tuần)</p>
                                <p class="text-xs text-slate-500 mt-1">Pha nước sôi cho bữa sáng nhanh, tinh bột hấp thu chậm.</p>
                            </div>
                            <span class="text-xs font-bold text-emerald-400 bg-emerald-500/10 px-2.5 py-1 rounded-full">Bữa Sáng</span>
                        </div>
                        <div class="bg-slate-900/60 p-4 rounded-xl border border-slate-800 flex justify-between items-center">
                            <div>
                                <h3 class="font-bold text-white">Khoai Lang Luộc</h3>
                                <p class="text-xs text-slate-400">~15.000đ / kg</p>
                                <p class="text-xs text-slate-500 mt-1">Tuyệt vời cho bữa phụ chiều trước khi đi tập.</p>
                            </div>
                            <span class="text-xs font-bold text-amber-400 bg-amber-500/10 px-2.5 py-1 rounded-full">Nạp Năng Lượng</span>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ================= PAGE 7: PACE CALCULATOR ================= -->
        <section id="tab-calculator" class="tab-content hidden space-y-6">
            <div class="bg-slate-800/80 p-6 rounded-2xl border border-slate-700/60">
                <h1 class="text-2xl font-bold text-white flex items-center gap-2">
                    <i data-lucide="calculator" class="w-6 h-6 text-orange-500"></i>
                    Công Cụ Tính Pace & Split Thời Gian
                </h1>
                <p class="text-slate-400 text-sm mt-1">Tính toán chính xác tốc độ chạy cần thiết để đạt mục tiêu mơ ước của bạn.</p>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6 space-y-4">
                    <h2 class="text-lg font-bold text-white mb-2">Nhập Thông Số Mục Tiêu</h2>

                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Cự ly chạy (KM)</label>
                        <input type="number" id="calc-distance" value="5" step="0.1" class="w-full bg-slate-900 text-white font-bold border border-slate-700 rounded-xl px-4 py-3 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>

                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Thời gian: Phút</label>
                            <input type="number" id="calc-minutes" value="24" class="w-full bg-slate-900 text-white font-bold border border-slate-700 rounded-xl px-4 py-3 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Giây</label>
                            <input type="number" id="calc-seconds" value="59" class="w-full bg-slate-900 text-white font-bold border border-slate-700 rounded-xl px-4 py-3 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                        </div>
                    </div>

                    <button onclick="calculatePace()" class="w-full bg-orange-600 hover:bg-orange-500 text-white font-bold py-3 rounded-xl transition shadow-lg shadow-orange-600/30 flex items-center justify-center gap-2">
                        <i data-lucide="calculator" class="w-5 h-5"></i> Tính Tốc Độ Pace
                    </button>
                </div>

                <div class="bg-slate-800/60 border border-slate-700/60 rounded-2xl p-6 flex flex-col justify-between">
                    <div>
                        <h2 class="text-lg font-bold text-white mb-4">Kết Quả Bắt Buộc</h2>
                        <div class="bg-slate-900/80 border border-slate-700 p-5 rounded-xl text-center mb-6">
                            <span class="text-xs font-bold text-slate-400 uppercase tracking-widest">Pace Trung Bình Yêu Cầu</span>
                            <div class="text-4xl font-extrabold text-orange-500 mt-2" id="calc-result-pace">4:59 <span class="text-sm font-normal text-slate-400">min/km</span></div>
                            <p class="text-xs text-slate-400 mt-2" id="calc-result-speed">Tốc độ tương đương: 12.0 km/h</p>
                        </div>
                        <h3 class="text-sm font-bold text-white mb-2">Chia Split Mỗi KM</h3>
                        <div class="space-y-2 max-h-48 overflow-y-auto pr-2" id="calc-split-list"></div>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Modal 1: Log Run (Add & Edit supported) -->
    <div id="modal-log-run" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-700 w-full max-w-md rounded-2xl p-6 shadow-2xl relative max-h-[90vh] overflow-y-auto">
            <button onclick="closeLogRunModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h2 class="text-xl font-bold text-white mb-4 flex items-center gap-2" id="modal-log-title">
                <i data-lucide="plus-circle" class="w-5 h-5 text-orange-500"></i>
                <span>Ghi Chép Buổi Chạy</span>
            </h2>

            <form onsubmit="handleSaveRun(event)" class="space-y-4">
                <input type="hidden" id="log-edit-id" value="">

                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Ngày thực hiện</label>
                    <input type="date" id="log-date" required class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Quãng đường (KM)</label>
                        <input type="number" step="0.01" id="log-distance" required placeholder="5.2" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Pace (Phút:Giây)</label>
                        <input type="text" id="log-pace" placeholder="5:10" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Loại bài tập</label>
                    <select id="log-type" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-orange-500">
                        <option value="Chạy tự do ngoài trời">Chạy tự do ngoài trời 🌳</option>
                        <option value="Easy Run">Easy Run (Chạy nhẹ)</option>
                        <option value="Tempo">Tempo (Chạy ngưỡng)</option>
                        <option value="Interval">Interval (Biến tốc)</option>
                        <option value="Long Run">Long Run (Chạy dài)</option>
                        <option value="Recovery Run">Recovery Run (Chạy phục hồi)</option>
                        <option value="Race">Race (Thi đấu / Chạy giải)</option>
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Chọn Đôi Giày Sử Dụng</label>
                    <select id="log-shoe-select" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-3 py-2.5 focus:ring-2 focus:ring-orange-500">
                        <!-- Populated dynamically -->
                    </select>
                </div>

                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Ghi chú / Cảm nhận</label>
                    <input type="text" id="log-note" placeholder="Cảm giác chân tốt, mệt ở km 4..." class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                </div>

                <button type="submit" id="log-submit-btn" class="w-full bg-orange-600 hover:bg-orange-500 text-white font-bold py-3 rounded-xl transition shadow-lg shadow-orange-600/30">
                    Lưu Buổi Chạy
                </button>
            </form>
        </div>
    </div>

    <!-- Modal 2: Add Shoe -->
    <div id="modal-add-shoe" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-700 w-full max-w-md rounded-2xl p-6 shadow-2xl relative">
            <button onclick="closeAddShoeModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h2 class="text-xl font-bold text-white mb-4 flex items-center gap-2">
                <i data-lucide="footprints" class="w-5 h-5 text-orange-500"></i>
                Thêm Đôi Giày Mới
            </h2>

            <form onsubmit="handleSaveShoe(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Tên Giày</label>
                    <input type="text" id="shoe-name" required placeholder="Ví dụ: Zocker Speed Light / Bitis Hunter" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                </div>

                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Số KM Ban Đầu</label>
                        <input type="number" step="0.1" id="shoe-initial-km" value="0" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Giới Hạn KM (Max)</label>
                        <input type="number" id="shoe-max-km" value="600" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                </div>

                <button type="submit" class="w-full bg-orange-600 hover:bg-orange-500 text-white font-bold py-3 rounded-xl transition shadow-lg shadow-orange-600/30">
                    Thêm Vào Tủ Giày
                </button>
            </form>
        </div>
    </div>

    <!-- Modal 3: Custom Plan Builder -->
    <div id="modal-custom-plan" class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-700 w-full max-w-2xl rounded-2xl p-6 shadow-2xl relative max-h-[90vh] overflow-y-auto">
            <button onclick="closeCustomPlanModal()" class="absolute top-4 right-4 text-slate-400 hover:text-white">
                <i data-lucide="x" class="w-5 h-5"></i>
            </button>
            <h2 class="text-xl font-bold text-white mb-2 flex items-center gap-2">
                <i data-lucide="file-plus" class="w-5 h-5 text-orange-500"></i>
                Tạo / Chỉnh Sửa Giáo Án Cá Nhân
            </h2>
            <p class="text-xs text-slate-400 mb-4">Tự thiết kế lịch chạy trong tuần từ Thứ 2 tới Chủ Nhật theo ý thích.</p>

            <form onsubmit="handleSaveCustomPlan(event)" class="space-y-4">
                <input type="hidden" id="custom-plan-id">
                
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Tên Giáo Án</label>
                        <input type="text" id="custom-plan-title" required placeholder="Ví dụ: Giáo Án Tăng Tốc 5K Cá Nhân" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-slate-400 uppercase mb-1">Pace Mục Tiêu (min/km)</label>
                        <input type="text" id="custom-plan-pace" required placeholder="4:50" class="w-full bg-slate-800 text-white border border-slate-700 rounded-xl px-4 py-2.5 focus:ring-2 focus:ring-orange-500 focus:outline-none">
                    </div>
                </div>

                <h3 class="text-sm font-bold text-white border-b border-slate-800 pb-2 pt-2">Lịch Tập 7 Ngày Trong Tuần</h3>

                <div class="space-y-3" id="custom-plan-days-container">
                    <!-- Populated 7 days JS -->
                </div>

                <button type="submit" class="w-full bg-orange-600 hover:bg-orange-500 text-white font-bold py-3 rounded-xl transition shadow-lg shadow-orange-600/30">
                    Lưu Giáo Án Cá Nhân
                </button>
            </form>
        </div>
    </div>

    <!-- Application Script -->
    <script>
        // ================= PRESET TARGETS CONFIG =================
        const PRESET_TARGETS = {
            '5k-sub25': { name: '5K Sub25', dist: 5, pace: '4:59', easyPace: '6:15 - 6:45', tempoPace: '5:05 - 5:15', intervalPace: '4:35 - 4:45' },
            '5k-sub24': { name: '5K Sub24', dist: 5, pace: '4:47', easyPace: '6:00 - 6:30', tempoPace: '4:50 - 5:00', intervalPace: '4:20 - 4:30' },
            '5k-sub23': { name: '5K Sub23', dist: 5, pace: '4:35', easyPace: '5:45 - 6:15', tempoPace: '4:40 - 4:50', intervalPace: '4:10 - 4:20' },
            '5k-sub22': { name: '5K Sub22', dist: 5, pace: '4:23', easyPace: '5:30 - 6:00', tempoPace: '4:30 - 4:40', intervalPace: '3:58 - 4:10' },
            '5k-sub21': { name: '5K Sub21', dist: 5, pace: '4:11', easyPace: '5:15 - 5:45', tempoPace: '4:18 - 4:28', intervalPace: '3:48 - 3:58' },
            '5k-sub20': { name: '5K Sub20', dist: 5, pace: '3:59', easyPace: '5:00 - 5:30', tempoPace: '4:05 - 4:15', intervalPace: '3:35 - 3:45' },
            '5k-sub19': { name: '5K Sub19', dist: 5, pace: '3:47', easyPace: '4:45 - 5:15', tempoPace: '3:53 - 4:02', intervalPace: '3:25 - 3:35' },
            '5k-sub18': { name: '5K Sub18', dist: 5, pace: '3:35', easyPace: '4:30 - 5:00', tempoPace: '3:40 - 3:50', intervalPace: '3:15 - 3:25' },
            '5k-sub17': { name: '5K Sub17', dist: 5, pace: '3:23', easyPace: '4:15 - 4:45', tempoPace: '3:28 - 3:38', intervalPace: '3:05 - 3:15' },
            '10k-sub60': { name: '10K Sub60', dist: 10, pace: '5:59', easyPace: '7:00 - 7:30', tempoPace: '6:05 - 6:15', intervalPace: '5:30 - 5:45' },
            '10k-sub55': { name: '10K Sub55', dist: 10, pace: '5:29', easyPace: '6:30 - 7:00', tempoPace: '5:35 - 5:45', intervalPace: '5:00 - 5:15' },
            '10k-sub50': { name: '10K Sub50', dist: 10, pace: '4:59', easyPace: '6:00 - 6:30', tempoPace: '5:05 - 5:15', intervalPace: '4:35 - 4:45' },
            '10k-sub45': { name: '10K Sub45', dist: 10, pace: '4:29', easyPace: '5:30 - 6:00', tempoPace: '4:35 - 4:45', intervalPace: '4:05 - 4:15' },
            '10k-sub40': { name: '10K Sub40', dist: 10, pace: '3:59', easyPace: '5:00 - 5:25', tempoPace: '4:05 - 4:15', intervalPace: '3:38 - 3:48' }
        };

        const EXERCISES = {
            strength: [
                { name: 'Air Squat (Gánh đùi)', reps: '3 hiệp x 15-20 lần', desc: 'Tăng sức mạnh đùi trước và mông. Đảm bảo lưng thẳng.' },
                { name: 'Walking Lunges', reps: '3 hiệp x 12 bước/chân', desc: 'Mô phỏng bước chạy, tăng cường độ dẻo cơ đùi sau & hông.' },
                { name: 'Single-Leg Calf Raise', reps: '3 hiệp x 20 lần/chân', desc: 'Đứng 1 chân nhón gót giúp bắp chân khỏe, bảo vệ gân Achilles.' }
            ],
            core: [
                { name: 'Plank Chuẩn', reps: '3 hiệp x 45-60 giây', desc: 'Siết chặt cơ bụng, giữ form người thẳng khi mệt.' },
                { name: 'Russian Twists', reps: '3 hiệp x 20 lần', desc: 'Tăng cường cơ bụng xiên, giúp ổn định thân trên không lắc lư.' },
                { name: 'Bird-Dog', reps: '3 hiệp x 12 lần/bên', desc: 'Cơ lưng dưới và mông phối hợp, giảm đau lưng khi chạy dài.' }
            ],
            mobility: [
                { name: 'Ankle Knee-to-Wall', reps: '2 phút mỗi bên', desc: 'Đẩy gối chạm tường không nhón gót, tăng biên độ cổ chân.' },
                { name: 'Leg Swings (Đu chân)', reps: '20 lần trước-sau', desc: 'Thả lỏng khớp háng trước khi chạy giúp bước chạy dài hơn.' },
                { name: 'Glute Bridge (Cầu mông)', reps: '3 hiệp x 15 lần', desc: 'Kích hoạt cơ mông làm việc trước các buổi chạy tốc độ.' }
            ]
        };

        const DAYS_OF_WEEK = ['Thứ 2', 'Thứ 3', 'Thứ 4', 'Thứ 5', 'Thứ 6', 'Thứ 7', 'Chủ Nhật'];

        // App Initial State
        let appState = {
            currentGoal: '5k-sub25',
            calendarView: 'week', // week, month, year
            calendarOffset: 0,
            logs: [],
            shoes: [
                { id: 'shoe-1', name: 'Zocker Speed Light', initialKm: 0, maxKm: 600, isActive: true },
                { id: 'shoe-2', name: 'Bitis Hunter Running', initialKm: 20, maxKm: 500, isActive: false }
            ],
            customPlans: {},
            completedTasks: {}
        };

        // ================= LOCAL STORAGE =================
        function loadState() {
            const saved = localStorage.getItem('pacelab_pro_state');
            if (saved) {
                try {
                    appState = Object.assign(appState, JSON.parse(saved));
                } catch(e) { console.error(e); }
            }
        }

        function saveState() {
            localStorage.setItem('pacelab_pro_state', JSON.stringify(appState));
        }

        // Data Import & Export
        function exportData() {
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(appState, null, 2));
            const downloadAnchor = document.createElement('a');
            downloadAnchor.setAttribute("href", dataStr);
            downloadAnchor.setAttribute("download", `pacelab_backup_${new Date().toISOString().split('T')[0]}.json`);
            document.body.appendChild(downloadAnchor);
            downloadAnchor.click();
            downloadAnchor.remove();
        }

        function importData(event) {
            const fileReader = new FileReader();
            fileReader.onload = function(e) {
                try {
                    const parsed = JSON.parse(e.target.result);
                    if (parsed && typeof parsed === 'object') {
                        appState = Object.assign(appState, parsed);
                        saveState();
                        renderDashboard();
                        renderCalendarView();
                        renderPlans();
                        renderShoes();
                        alert("Đã khôi phục dữ liệu thành công!");
                    }
                } catch (err) {
                    alert("Tệp JSON không hợp lệ!");
                }
            };
            if (event.target.files[0]) {
                fileReader.readAsText(event.target.files[0]);
            }
        }

        // ================= TAB SWITCHING =================
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active', 'text-orange-400', 'bg-slate-800');
                btn.classList.add('text-slate-400');
            });
            const activeNav = document.getElementById(`nav-${tabId}`);
            if (activeNav) activeNav.classList.add('active', 'text-orange-400', 'bg-slate-800');

            ['dashboard', 'calendar', 'plans', 'shoes', 'workout', 'nutrition', 'calculator'].forEach(t => {
                const mNav = document.getElementById(`m-nav-${t}`);
                if (mNav) {
                    mNav.className = `flex flex-col items-center py-1 ${t === tabId ? 'text-orange-500' : 'text-slate-400'}`;
                }
            });

            if (tabId === 'calendar') renderCalendarView();
        }

        // ================= GOAL & CUSTOM PLANS LOGIC =================
        function populateGoalSelector() {
            const customGroup = document.getElementById('custom-plans-optgroup');
            customGroup.innerHTML = '';
            
            const customKeys = Object.keys(appState.customPlans);
            if (customKeys.length === 0) {
                customGroup.innerHTML = `<option disabled>(Chưa tạo giáo án riêng)</option>`;
            } else {
                customKeys.forEach(key => {
                    const plan = appState.customPlans[key];
                    customGroup.innerHTML += `<option value="${key}">${plan.title} (Pace ${plan.pace})</option>`;
                });
            }
            
            document.getElementById('goal-select').value = appState.currentGoal;
        }

        function changeGoal(goalKey) {
            appState.currentGoal = goalKey;
            saveState();
            renderDashboard();
            renderPlans();
        }

        function getCurrentPlanData() {
            if (appState.customPlans[appState.currentGoal]) {
                return {
                    isCustom: true,
                    data: appState.customPlans[appState.currentGoal]
                };
            }
            const preset = PRESET_TARGETS[appState.currentGoal] || PRESET_TARGETS['5k-sub25'];
            return {
                isCustom: false,
                data: preset
            };
        }

        // ================= DASHBOARD & STATS CALCULATIONS =================
        function renderDashboard() {
            populateGoalSelector();
            const planInfo = getCurrentPlanData();
            
            if (planInfo.isCustom) {
                document.getElementById('dash-goal-title').innerText = planInfo.data.title;
                document.getElementById('dash-goal-pace').innerText = `${planInfo.data.pace} min/km`;
            } else {
                document.getElementById('dash-goal-title').innerText = planInfo.data.name;
                document.getElementById('dash-goal-pace').innerText = `${planInfo.data.pace} min/km`;
            }

            // Date & Mileage Aggregations
            const now = new Date();
            document.getElementById('current-year-txt').innerText = now.getFullYear();

            let totalKm = 0, yearKm = 0, monthKm = 0, weekKm = 0;

            const startOfWeek = new Date(now);
            const day = startOfWeek.getDay() || 7;
            startOfWeek.setDate(startOfWeek.getDate() - day + 1);
            startOfWeek.setHours(0,0,0,0);

            const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
            const startOfYear = new Date(now.getFullYear(), 0, 1);

            appState.logs.forEach(log => {
                const dist = parseFloat(log.distance || 0);
                totalKm += dist;
                
                const logDate = new Date(log.date);
                if (logDate >= startOfYear) yearKm += dist;
                if (logDate >= startOfMonth) monthKm += dist;
                if (logDate >= startOfWeek) weekKm += dist;
            });

            document.getElementById('stat-total-km').innerText = `${totalKm.toFixed(1)} km`;
            document.getElementById('stat-year-km').innerText = `${yearKm.toFixed(1)} km`;
            document.getElementById('stat-month-km').innerText = `${monthKm.toFixed(1)} km`;
            document.getElementById('stat-week-km').innerText = `${weekKm.toFixed(1)} km`;

            // Active Shoe Info
            renderActiveShoeCard();

            // Recent Logs
            renderRecentLogs();
        }

        function calculateShoeKm(shoeId) {
            const shoe = appState.shoes.find(s => s.id === shoeId);
            if (!shoe) return 0;
            const logKm = appState.logs
                .filter(l => l.shoeId === shoeId)
                .reduce((sum, l) => sum + parseFloat(l.distance || 0), 0);
            return (shoe.initialKm || 0) + logKm;
        }

        function renderActiveShoeCard() {
            const activeShoe = appState.shoes.find(s => s.isActive) || appState.shoes[0];
            const container = document.getElementById('dash-active-shoe-card');

            if (!activeShoe) {
                container.innerHTML = `<p class="text-xs text-slate-500">Chưa có giày trong tủ.</p>`;
                return;
            }

            const currentKm = calculateShoeKm(activeShoe.id);
            const percent = Math.min(100, Math.round((currentKm / activeShoe.maxKm) * 100));

            container.innerHTML = `
                <div class="flex items-center justify-between">
                    <span class="font-bold text-white text-sm">${activeShoe.name}</span>
                    <span class="text-xs font-bold text-orange-400">${currentKm.toFixed(1)} / ${activeShoe.maxKm} km</span>
                </div>
                <div class="w-full bg-slate-900 rounded-full h-2 my-2 overflow-hidden border border-slate-700">
                    <div class="bg-gradient-to-r from-orange-500 to-amber-400 h-2 rounded-full" style="width: ${percent}%"></div>
                </div>
                <p class="text-[11px] text-slate-400">Mức độ sử dụng: ${percent}% ${percent >= 90 ? '⚠️ Nên cân nhắc thay giày' : ''}</p>
            `;
        }

        function renderRecentLogs() {
            const container = document.getElementById('recent-logs-list');
            if (appState.logs.length === 0) {
                container.innerHTML = `<p class="text-center py-6 text-slate-500 text-xs">Chưa có nhật ký bài chạy nào.</p>`;
                return;
            }

            const sorted = [...appState.logs].sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0, 4);
            container.innerHTML = sorted.map(log => {
                const shoe = appState.shoes.find(s => s.id === log.shoeId);
                return `
                    <div class="bg-slate-900/70 border border-slate-800 p-3.5 rounded-xl flex items-center justify-between">
                        <div class="flex items-center gap-3">
                            <div class="p-2 bg-orange-500/10 text-orange-400 rounded-lg font-bold text-xs">
                                ${log.type || 'Run'}
                            </div>
                            <div>
                                <h4 class="font-bold text-white text-sm">${log.distance} KM <span class="text-xs text-slate-400 font-normal">(${log.pace || 'N/A'} pace)</span></h4>
                                <p class="text-xs text-slate-500">${log.date} ${shoe ? '• ' + shoe.name : ''}</p>
                            </div>
                        </div>
                        <div class="flex items-center gap-2">
                            ${log.note ? `<span class="text-xs text-slate-400 italic max-w-[120px] truncate hidden sm:inline">"${log.note}"</span>` : ''}
                            <button onclick="editRunLog('${log.id}')" class="text-xs bg-slate-800 hover:bg-slate-700 text-slate-300 p-1.5 rounded-lg border border-slate-700 transition">
                                <i data-lucide="edit-2" class="w-3.5 h-3.5"></i>
                            </button>
                        </div>
                    </div>
                `;
            }).join('');
            lucide.createIcons();
        }

        // ================= CALENDAR & HISTORY LOGIC =================
        function setCalendarView(view) {
            appState.calendarView = view;
            appState.calendarOffset = 0;
            ['week', 'month', 'year'].forEach(v => {
                const btn = document.getElementById(`btn-view-${v}`);
                if (v === view) {
                    btn.className = 'px-3 py-1.5 rounded-lg text-xs font-semibold transition bg-orange-600 text-white';
                } else {
                    btn.className = 'px-3 py-1.5 rounded-lg text-xs font-semibold text-slate-400 hover:text-white transition';
                }
            });
            renderCalendarView();
        }

        function navigateCalendar(dir) {
            appState.calendarOffset += dir;
            renderCalendarView();
        }

        function renderCalendarView() {
            const container = document.getElementById('calendar-view-container');
            const label = document.getElementById('calendar-period-label');
            const now = new Date();

            if (appState.calendarView === 'week') {
                const currWeekStart = new Date(now);
                const day = currWeekStart.getDay() || 7;
                currWeekStart.setDate(currWeekStart.getDate() - day + 1 + (appState.calendarOffset * 7));
                currWeekStart.setHours(0,0,0,0);

                const currWeekEnd = new Date(currWeekStart);
                currWeekEnd.setDate(currWeekEnd.getDate() + 6);

                label.innerText = `${currWeekStart.toLocaleDateString('vi-VN')} - ${currWeekEnd.toLocaleDateString('vi-VN')}`;

                let daysHtml = '';
                let weekTotal = 0;

                for (let i = 0; i < 7; i++) {
                    const d = new Date(currWeekStart);
                    d.setDate(d.getDate() + i);
                    const dateStr = d.toISOString().split('T')[0];

                    const dayLogs = appState.logs.filter(l => l.date === dateStr);
                    const dayKm = dayLogs.reduce((sum, l) => sum + parseFloat(l.distance || 0), 0);
                    weekTotal += dayKm;

                    const isToday = d.toDateString() === now.toDateString();

                    daysHtml += `
                        <div class="bg-slate-900/80 border ${isToday ? 'border-orange-500' : 'border-slate-800'} rounded-xl p-3 flex flex-col justify-between min-h-[110px]">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-slate-400">${DAYS_OF_WEEK[i]}</span>
                                <span class="text-slate-500">${d.getDate()}/${d.getMonth()+1}</span>
                            </div>
                            <div class="my-2">
                                ${dayKm > 0 ? `
                                    <span class="text-lg font-black text-orange-400 block">${dayKm.toFixed(1)} km</span>
                                    <span class="text-[10px] text-slate-400">${dayLogs.length} buổi</span>
                                ` : `<span class="text-xs text-slate-600 font-medium">Nghỉ</span>`}
                            </div>
                            <button onclick="openLogRunModalWithDate('${dateStr}')" class="text-[10px] text-slate-500 hover:text-orange-400 font-medium text-left">+ Thêm</button>
                        </div>
                    `;
                }

                container.innerHTML = `
                    <div class="mb-3 text-right text-xs text-orange-400 font-bold">Tổng tuần: ${weekTotal.toFixed(1)} KM</div>
                    <div class="grid grid-cols-2 sm:grid-cols-4 lg:grid-cols-7 gap-3">${daysHtml}</div>
                `;

            } else if (appState.calendarView === 'month') {
                const targetMonth = new Date(now.getFullYear(), now.getMonth() + appState.calendarOffset, 1);
                label.innerText = `Tháng ${targetMonth.getMonth()+1} / ${targetMonth.getFullYear()}`;

                const lastDay = new Date(targetMonth.getFullYear(), targetMonth.getMonth() + 1, 0);
                
                let monthTotal = 0;
                let daysGrid = '';

                for (let day = 1; day <= lastDay.getDate(); day++) {
                    const dateObj = new Date(targetMonth.getFullYear(), targetMonth.getMonth(), day);
                    const dateStr = dateObj.toISOString().split('T')[0];

                    const dayLogs = appState.logs.filter(l => l.date === dateStr);
                    const dayKm = dayLogs.reduce((sum, l) => sum + parseFloat(l.distance || 0), 0);
                    monthTotal += dayKm;

                    daysGrid += `
                        <div class="bg-slate-900/60 border border-slate-800/80 p-2 rounded-lg text-center ${dayKm > 0 ? 'bg-orange-950/20 border-orange-500/40' : ''}">
                            <span class="text-[10px] text-slate-500 block">${day}</span>
                            <span class="text-xs font-bold ${dayKm > 0 ? 'text-orange-400' : 'text-slate-600'}">${dayKm > 0 ? dayKm.toFixed(1) : '-'}</span>
                        </div>
                    `;
                }

                container.innerHTML = `
                    <div class="mb-3 text-right text-xs text-orange-400 font-bold">Tổng tháng: ${monthTotal.toFixed(1)} KM</div>
                    <div class="grid grid-cols-5 sm:grid-cols-7 gap-2">${daysGrid}</div>
                `;

            } else if (appState.calendarView === 'year') {
                const targetYear = now.getFullYear() + appState.calendarOffset;
                label.innerText = `Năm ${targetYear}`;

                let monthsGrid = '';
                let yearTotal = 0;

                for (let m = 0; m < 12; m++) {
                    const mLogs = appState.logs.filter(l => {
                        const d = new Date(l.date);
                        return d.getFullYear() === targetYear && d.getMonth() === m;
                    });
                    const mKm = mLogs.reduce((sum, l) => sum + parseFloat(l.distance || 0), 0);
                    yearTotal += mKm;

                    monthsGrid += `
                        <div class="bg-slate-900/80 border border-slate-800 p-4 rounded-xl flex justify-between items-center">
                            <div>
                                <h4 class="font-bold text-white text-sm">Tháng ${m+1}</h4>
                                <p class="text-xs text-slate-500">${mLogs.length} buổi chạy</p>
                            </div>
                            <span class="text-lg font-black text-orange-400">${mKm.toFixed(1)} KM</span>
                        </div>
                    `;
                }

                container.innerHTML = `
                    <div class="mb-4 text-right text-xs text-orange-400 font-bold">Tổng năm ${targetYear}: ${yearTotal.toFixed(1)} KM</div>
                    <div class="grid grid-cols-1 sm:grid-cols-3 lg:grid-cols-4 gap-4">${monthsGrid}</div>
                `;
            }

            renderLogHistory();
        }

        function renderLogHistory() {
            const tbody = document.getElementById('log-history-tbody');
            const search = (document.getElementById('log-search-input').value || '').toLowerCase();

            let filtered = [...appState.logs].sort((a,b) => new Date(b.date) - new Date(a.date));

            if (search) {
                filtered = filtered.filter(l => {
                    const shoe = appState.shoes.find(s => s.id === l.shoeId);
                    const shoeName = shoe ? shoe.name.toLowerCase() : '';
                    return (l.note || '').toLowerCase().includes(search) || 
                           (l.type || '').toLowerCase().includes(search) || 
                           shoeName.includes(search) ||
                           l.date.includes(search);
                });
            }

            if (filtered.length === 0) {
                tbody.innerHTML = `<tr><td colspan="7" class="text-center py-6 text-slate-500 text-xs">Không tìm thấy nhật ký bài chạy phù hợp.</td></tr>`;
                return;
            }

            tbody.innerHTML = filtered.map(log => {
                const shoe = appState.shoes.find(s => s.id === log.shoeId);
                return `
                    <tr class="hover:bg-slate-800/40 transition">
                        <td class="p-3 font-semibold text-white">${log.date}</td>
                        <td class="p-3 font-bold text-orange-400">${log.distance} km</td>
                        <td class="p-3">${log.pace || '-'}</td>
                        <td class="p-3"><span class="bg-slate-800 text-orange-300 text-xs px-2 py-0.5 rounded font-medium">${log.type || 'Run'}</span></td>
                        <td class="p-3 text-xs text-slate-300">${shoe ? shoe.name : 'Chưa chọn'}</td>
                        <td class="p-3 text-xs text-slate-400 italic max-w-[200px] truncate">${log.note || '-'}</td>
                        <td class="p-3 text-right space-x-2">
                            <button onclick="editRunLog('${log.id}')" class="text-xs text-orange-400 hover:text-orange-300 font-medium">Sửa</button>
                            <button onclick="deleteRunLog('${log.id}')" class="text-xs text-red-400 hover:text-red-300 font-medium">Xóa</button>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        function editRunLog(logId) {
            const log = appState.logs.find(l => l.id === logId);
            if (!log) return;

            document.getElementById('log-edit-id').value = log.id;
            document.getElementById('log-date').value = log.date;
            document.getElementById('log-distance').value = log.distance;
            document.getElementById('log-pace').value = log.pace || '';
            document.getElementById('log-type').value = log.type || 'Chạy tự do ngoài trời';
            document.getElementById('log-note').value = log.note || '';

            populateShoeDropdown(log.shoeId);

            document.getElementById('modal-log-title').innerHTML = `
                <i data-lucide="edit-3" class="w-5 h-5 text-orange-500"></i>
                <span>Chỉnh Sửa Nhật Ký Chạy</span>
            `;
            document.getElementById('log-submit-btn').innerText = 'Cập Nhật Nhập Liệu';

            document.getElementById('modal-log-run').classList.remove('hidden');
            lucide.createIcons();
        }

        function deleteRunLog(logId) {
            if (confirm("Bạn có chắc chắn muốn xóa bài chạy này? Mọi số KM của đôi giày liên quan sẽ được tự động trừ bù.")) {
                appState.logs = appState.logs.filter(l => l.id !== logId);
                saveState();
                renderDashboard();
                renderCalendarView();
                renderShoes();
            }
        }

        // ================= TRAINING PLAN BUILDER =================
        function renderPlans() {
            const planInfo = getCurrentPlanData();
            const container = document.getElementById('weekly-schedule-container');
            const customActions = document.getElementById('custom-plan-actions');

            if (planInfo.isCustom) {
                document.getElementById('plan-target-name').innerText = planInfo.data.title;
                document.getElementById('zone-easy-pace').innerText = `${planInfo.data.pace} /km`;
                document.getElementById('zone-tempo-pace').innerText = `${planInfo.data.pace} /km`;
                document.getElementById('zone-interval-pace').innerText = `${planInfo.data.pace} /km`;
                customActions.classList.remove('hidden');

                container.innerHTML = planInfo.data.days.map((dayData, idx) => {
                    const taskKey = `${appState.currentGoal}-w${idx}`;
                    const isChecked = !!appState.completedTasks[taskKey];

                    return `
                        <div class="bg-slate-900/80 border ${isChecked ? 'border-emerald-500/40 bg-emerald-950/10' : 'border-slate-800'} p-4 rounded-xl flex items-start justify-between gap-4">
                            <div class="flex items-start gap-3">
                                <input type="checkbox" onchange="toggleTask('${taskKey}')" ${isChecked ? 'checked' : ''} class="mt-1 w-5 h-5 rounded text-orange-500 bg-slate-800 border-slate-700 focus:ring-orange-500">
                                <div>
                                    <div class="flex items-center gap-2">
                                        <span class="text-xs font-bold px-2 py-0.5 rounded bg-slate-800 text-orange-400">${DAYS_OF_WEEK[idx]}</span>
                                        <h3 class="font-bold text-white text-sm ${isChecked ? 'line-through text-slate-400' : ''}">${dayData.type} - ${dayData.dist} KM</h3>
                                    </div>
                                    <p class="text-xs text-slate-400 mt-1">${dayData.note || 'Không có ghi chú'}</p>
                                    <span class="inline-block text-[11px] font-semibold text-slate-500 mt-1">Pace target: ${dayData.pace || planInfo.data.pace}</span>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');

            } else {
                document.getElementById('plan-target-name').innerText = planInfo.data.name;
                document.getElementById('zone-easy-pace').innerText = `${planInfo.data.easyPace} /km`;
                document.getElementById('zone-tempo-pace').innerText = `${planInfo.data.tempoPace} /km`;
                document.getElementById('zone-interval-pace').innerText = `${planInfo.data.intervalPace} /km`;
                customActions.classList.add('hidden');

                const defaultWorkouts = [
                    { day: 'Thứ 2', title: 'Easy Run + Bài tập Khớp & Cổ Chân', dist: '4 - 5 km', desc: `Chạy thả lỏng nhẹ nhàng ở pace ${planInfo.data.easyPace}. Sau khi chạy xong tập 10 phút Mobility cổ chân.` },
                    { day: 'Thứ 4', title: 'Interval (Biến Tốc) hoặc Tempo', dist: '5 km tổng', desc: `Khởi động 1km. Chạy 5x400m tốc độ pace ${planInfo.data.intervalPace}. Hoặc chạy Tempo 3km ở pace ${planInfo.data.tempoPace}.` },
                    { day: 'Thứ 6', title: 'Easy Run + Bổ Trợ Thân Dưới (Bodyweight)', dist: '4 km', desc: `Chạy nhẹ nhàng pace ${planInfo.data.easyPace}. Về phòng thực hiện 3 hiệp Squat + Lunges.` },
                    { day: 'Chủ Nhật', title: 'Long Run (Chạy Dài Tích Lũy)', dist: planInfo.data.dist === 5 ? '7 - 8 km' : '12 - 14 km', desc: `Chạy duy trì đều đặn ở pace ${planInfo.data.easyPace}. Giúp xây dựng sức bền tim mạch vững chắc.` }
                ];

                container.innerHTML = defaultWorkouts.map((w, idx) => {
                    const taskKey = `${appState.currentGoal}-w${idx}`;
                    const isChecked = !!appState.completedTasks[taskKey];

                    return `
                        <div class="bg-slate-900/80 border ${isChecked ? 'border-emerald-500/40 bg-emerald-950/10' : 'border-slate-800'} p-4 rounded-xl flex items-start justify-between gap-4">
                            <div class="flex items-start gap-3">
                                <input type="checkbox" onchange="toggleTask('${taskKey}')" ${isChecked ? 'checked' : ''} class="mt-1 w-5 h-5 rounded text-orange-500 bg-slate-800 border-slate-700 focus:ring-orange-500">
                                <div>
                                    <div class="flex items-center gap-2">
                                        <span class="text-xs font-bold px-2 py-0.5 rounded bg-slate-800 text-orange-400">${w.day}</span>
                                        <h3 class="font-bold text-white text-sm ${isChecked ? 'line-through text-slate-400' : ''}">${w.title}</h3>
                                    </div>
                                    <p class="text-xs text-slate-400 mt-1">${w.desc}</p>
                                    <span class="inline-block text-[11px] font-semibold text-slate-500 mt-1">Cự ly gợi ý: ${w.dist}</span>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');
            }
        }

        function toggleTask(taskKey) {
            appState.completedTasks[taskKey] = !appState.completedTasks[taskKey];
            saveState();
            renderDashboard();
            renderPlans();
        }

        function openCustomPlanModal() {
            document.getElementById('custom-plan-id').value = '';
            document.getElementById('custom-plan-title').value = '';
            document.getElementById('custom-plan-pace').value = '';

            const daysContainer = document.getElementById('custom-plan-days-container');
            daysContainer.innerHTML = DAYS_OF_WEEK.map((d, i) => `
                <div class="bg-slate-800/80 p-3 rounded-xl border border-slate-700/60 grid grid-cols-1 sm:grid-cols-4 gap-2 text-xs">
                    <span class="font-bold text-orange-400 flex items-center">${d}</span>
                    <input type="text" id="cp-type-${i}" placeholder="Loại bài (Easy/Tempo/Rest)" class="bg-slate-900 text-white p-2 rounded border border-slate-700">
                    <input type="text" id="cp-dist-${i}" placeholder="Cự ly (KM)" class="bg-slate-900 text-white p-2 rounded border border-slate-700">
                    <input type="text" id="cp-note-${i}" placeholder="Ghi chú thêm" class="bg-slate-900 text-white p-2 rounded border border-slate-700">
                </div>
            `).join('');

            document.getElementById('modal-custom-plan').classList.remove('hidden');
        }

        function closeCustomPlanModal() {
            document.getElementById('modal-custom-plan').classList.add('hidden');
        }

        function handleSaveCustomPlan(e) {
            e.preventDefault();
            const planId = document.getElementById('custom-plan-id').value || ('custom-' + Date.now());
            const title = document.getElementById('custom-plan-title').value;
            const pace = document.getElementById('custom-plan-pace').value;

            const days = [];
            for (let i = 0; i < 7; i++) {
                days.push({
                    type: document.getElementById(`cp-type-${i}`).value || 'Rest',
                    dist: document.getElementById(`cp-dist-${i}`).value || '0',
                    note: document.getElementById(`cp-note-${i}`).value || '',
                    pace: pace
                });
            }

            appState.customPlans[planId] = {
                title: title,
                pace: pace,
                days: days
            };

            appState.currentGoal = planId;
            saveState();
            closeCustomPlanModal();
            renderDashboard();
            renderPlans();
        }

        function editCurrentCustomPlan() {
            const plan = appState.customPlans[appState.currentGoal];
            if (!plan) return;

            openCustomPlanModal();
            document.getElementById('custom-plan-id').value = appState.currentGoal;
            document.getElementById('custom-plan-title').value = plan.title;
            document.getElementById('custom-plan-pace').value = plan.pace;

            plan.days.forEach((d, i) => {
                document.getElementById(`cp-type-${i}`).value = d.type;
                document.getElementById(`cp-dist-${i}`).value = d.dist;
                document.getElementById(`cp-note-${i}`).value = d.note;
            });
        }

        function deleteCurrentCustomPlan() {
            if (confirm("Xóa giáo án cá nhân này?")) {
                delete appState.customPlans[appState.currentGoal];
                appState.currentGoal = '5k-sub25';
                saveState();
                renderDashboard();
                renderPlans();
            }
        }

        // ================= ADVANCED SHOE TRACKER =================
        function renderShoes() {
            const grid = document.getElementById('shoes-grid');
            if (appState.shoes.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-8 text-slate-500">Chưa có đôi giày nào trong tủ. Hãy thêm đôi giày đầu tiên!</div>`;
                return;
            }

            grid.innerHTML = appState.shoes.map(s => {
                const totalKm = calculateShoeKm(s.id);
                const percent = Math.min(100, Math.round((totalKm / s.maxKm) * 100));

                let statusBadge = `<span class="bg-emerald-500/10 text-emerald-400 border border-emerald-500/30 text-xs px-2.5 py-1 rounded-full font-semibold">Đế Tốt</span>`;
                if (percent >= 85 && percent < 100) {
                    statusBadge = `<span class="bg-amber-500/10 text-amber-400 border border-amber-500/30 text-xs px-2.5 py-1 rounded-full font-semibold">Sắp Đạt Giới Hạn</span>`;
                } else if (percent >= 100) {
                    statusBadge = `<span class="bg-red-500/10 text-red-400 border border-red-500/30 text-xs px-2.5 py-1 rounded-full font-semibold">Nên Thay Giày</span>`;
                }

                return `
                    <div class="bg-slate-800/60 border ${s.isActive ? 'border-orange-500/80 shadow-lg shadow-orange-500/10' : 'border-slate-700/60'} rounded-2xl p-5 flex flex-col justify-between relative">
                        ${s.isActive ? `<span class="absolute -top-2.5 left-4 bg-orange-600 text-white text-[10px] uppercase font-black px-2 py-0.5 rounded-full">Đang dùng chính</span>` : ''}
                        <div>
                            <div class="flex items-start justify-between gap-2 mb-3">
                                <h3 class="font-bold text-white text-base">${s.name}</h3>
                                ${statusBadge}
                            </div>

                            <div class="space-y-2 my-4">
                                <div class="flex justify-between text-xs">
                                    <span class="text-slate-400">Đã chạy:</span>
                                    <span class="font-bold text-white">${totalKm.toFixed(1)} / ${s.maxKm} KM</span>
                                </div>
                                <div class="w-full bg-slate-900 rounded-full h-2.5 overflow-hidden border border-slate-700">
                                    <div class="bg-gradient-to-r from-orange-500 to-amber-400 h-2.5 rounded-full transition-all duration-500" style="width: ${percent}%"></div>
                                </div>
                            </div>
                        </div>

                        <div class="flex items-center justify-between gap-2 pt-3 border-t border-slate-700/60 text-xs">
                            ${!s.isActive ? `
                                <button onclick="setActiveShoe('${s.id}')" class="text-orange-400 hover:text-orange-300 font-semibold">Đặt làm chính</button>
                            ` : `<span class="text-slate-500">Giày mặc định</span>`}
                            <button onclick="deleteShoe('${s.id}')" class="text-slate-500 hover:text-red-400 transition">Xóa</button>
                        </div>
                    </div>
                `;
            }).join('');

            populateShoeDropdown();
        }

        function populateShoeDropdown(selectedShoeId = null) {
            const shoeSelect = document.getElementById('log-shoe-select');
            if (shoeSelect) {
                shoeSelect.innerHTML = appState.shoes.map(s => {
                    const isSel = selectedShoeId ? (s.id === selectedShoeId) : s.isActive;
                    return `<option value="${s.id}" ${isSel ? 'selected' : ''}>${s.name} (${calculateShoeKm(s.id).toFixed(1)} km)</option>`;
                }).join('');
            }
        }

        function setActiveShoe(shoeId) {
            appState.shoes.forEach(s => s.isActive = (s.id === shoeId));
            saveState();
            renderShoes();
            renderDashboard();
        }

        function deleteShoe(shoeId) {
            if (confirm("Bạn có chắc chắn muốn xóa đôi giày này?")) {
                appState.shoes = appState.shoes.filter(s => s.id !== shoeId);
                saveState();
                renderShoes();
                renderDashboard();
            }
        }

        // ================= WORKOUT & EXERCISES =================
        function renderWorkouts() {
            const renderCardGroup = (list, containerId) => {
                const container = document.getElementById(containerId);
                container.innerHTML = list.map(item => `
                    <div class="bg-slate-800/60 border border-slate-700/60 rounded-xl p-5 hover:border-slate-600 transition flex flex-col justify-between">
                        <div>
                            <span class="text-xs font-bold text-orange-400 bg-orange-500/10 px-2 py-0.5 rounded">${item.reps}</span>
                            <h3 class="font-bold text-white text-base mt-2">${item.name}</h3>
                            <p class="text-xs text-slate-400 mt-2 leading-relaxed">${item.desc}</p>
                        </div>
                    </div>
                `).join('');
            };

            renderCardGroup(EXERCISES.strength, 'exercises-strength');
            renderCardGroup(EXERCISES.core, 'exercises-core');
            renderCardGroup(EXERCISES.mobility, 'exercises-mobility');
        }

        // Timer
        let timerInterval = null;
        let timerSeconds = 45;

        function startTimer(seconds) {
            clearInterval(timerInterval);
            timerSeconds = seconds;
            updateTimerDisplay();
            document.getElementById('timer-status').innerText = 'Đang tập luyện! Cố lên...';

            timerInterval = setInterval(() => {
                timerSeconds--;
                updateTimerDisplay();
                if (timerSeconds <= 0) {
                    clearInterval(timerInterval);
                    document.getElementById('timer-status').innerText = '🎉 Hoàn thành! Nghỉ ngơi 15 giây.';
                }
            }, 1000);
        }

        function resetTimer() {
            clearInterval(timerInterval);
            timerSeconds = 45;
            updateTimerDisplay();
            document.getElementById('timer-status').innerText = 'Sẵn sàng tập (45s tập / 15s nghỉ)';
        }

        function updateTimerDisplay() {
            const mins = Math.floor(timerSeconds / 60);
            const secs = timerSeconds % 60;
            document.getElementById('timer-display').innerText = 
                `${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
        }

        // ================= PACE CALCULATOR =================
        function calculatePace() {
            const dist = parseFloat(document.getElementById('calc-distance').value) || 0;
            const mins = parseInt(document.getElementById('calc-minutes').value) || 0;
            const secs = parseInt(document.getElementById('calc-seconds').value) || 0;

            if (dist <= 0) return;

            const totalSeconds = (mins * 60) + secs;
            const secondsPerKm = totalSeconds / dist;

            const paceMins = Math.floor(secondsPerKm / 60);
            const paceSecs = Math.round(secondsPerKm % 60);

            const formattedPace = `${paceMins}:${paceSecs.toString().padStart(2, '0')}`;
            const speedKmH = (dist / (totalSeconds / 3600)).toFixed(1);

            document.getElementById('calc-result-pace').innerHTML = `${formattedPace} <span class="text-sm font-normal text-slate-400">min/km</span>`;
            document.getElementById('calc-result-speed').innerText = `Tốc độ trung bình: ${speedKmH} km/h`;

            const splitList = document.getElementById('calc-split-list');
            splitList.innerHTML = '';
            for (let i = 1; i <= Math.ceil(dist); i++) {
                const kmMark = i > dist ? dist.toFixed(1) : i;
                splitList.innerHTML += `
                    <div class="flex justify-between items-center bg-slate-900/50 px-3 py-1.5 rounded text-xs">
                        <span class="text-slate-400">KM ${kmMark}</span>
                        <span class="font-bold text-white">${formattedPace}</span>
                    </div>
                `;
            }
        }

        // ================= MODAL HANDLERS =================
        function openLogRunModal() {
            document.getElementById('log-edit-id').value = '';
            document.getElementById('log-date').value = new Date().toISOString().split('T')[0];
            document.getElementById('log-distance').value = '';
            document.getElementById('log-pace').value = '';
            document.getElementById('log-note').value = '';
            document.getElementById('modal-log-title').innerHTML = `
                <i data-lucide="plus-circle" class="w-5 h-5 text-orange-500"></i>
                <span>Ghi Chép Buổi Chạy</span>
            `;
            document.getElementById('log-submit-btn').innerText = 'Lưu Buổi Chạy';

            populateShoeDropdown();
            document.getElementById('modal-log-run').classList.remove('hidden');
            lucide.createIcons();
        }

        function openLogRunModalWithDate(dateStr) {
            openLogRunModal();
            document.getElementById('log-date').value = dateStr;
        }

        function closeLogRunModal() {
            document.getElementById('modal-log-run').classList.add('hidden');
        }

        function openAddShoeModal() {
            document.getElementById('modal-add-shoe').classList.remove('hidden');
        }

        function closeAddShoeModal() {
            document.getElementById('modal-add-shoe').classList.add('hidden');
        }

        function handleSaveRun(e) {
            e.preventDefault();
            const editId = document.getElementById('log-edit-id').value;
            const date = document.getElementById('log-date').value;
            const dist = parseFloat(document.getElementById('log-distance').value);
            const pace = document.getElementById('log-pace').value;
            const type = document.getElementById('log-type').value;
            const shoeId = document.getElementById('log-shoe-select').value;
            const note = document.getElementById('log-note').value;

            if (editId) {
                // Update Existing Log
                const index = appState.logs.findIndex(l => l.id === editId);
                if (index !== -1) {
                    appState.logs[index] = {
                        id: editId,
                        date: date,
                        distance: dist,
                        pace: pace,
                        type: type,
                        shoeId: shoeId,
                        note: note
                    };
                }
            } else {
                // Create New Log
                const newLog = {
                    id: 'log-' + Date.now(),
                    date: date,
                    distance: dist,
                    pace: pace,
                    type: type,
                    shoeId: shoeId,
                    note: note
                };
                appState.logs.push(newLog);
            }

            saveState();
            closeLogRunModal();
            renderDashboard();
            renderCalendarView();
            renderShoes();

            e.target.reset();
        }

        function handleSaveShoe(e) {
            e.preventDefault();
            const name = document.getElementById('shoe-name').value;
            const initialKm = parseFloat(document.getElementById('shoe-initial-km').value) || 0;
            const maxKm = parseFloat(document.getElementById('shoe-max-km').value) || 600;

            const newShoe = {
                id: 'shoe-' + Date.now(),
                name: name,
                initialKm: initialKm,
                maxKm: maxKm,
                isActive: appState.shoes.length === 0
            };

            appState.shoes.push(newShoe);
            saveState();

            closeAddShoeModal();
            renderShoes();
            renderDashboard();

            e.target.reset();
        }

        // ================= INIT =================
        window.onload = function() {
            loadState();

            renderDashboard();
            renderPlans();
            renderShoes();
            renderWorkouts();
            calculatePace();

            lucide.createIcons();
        };
    </script>
</body>
</html>
