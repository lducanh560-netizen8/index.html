# index.html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Hub Pro - Nền tảng học tập thông minh</title>
    <!-- Tailwind CSS & FontAwesome -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        .glass { background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(12px); }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 flex h-screen overflow-hidden">

    <!-- Sidebar Navigation -->
    <aside class="w-64 bg-slate-900 text-slate-300 flex flex-col justify-between hidden md:flex shadow-xl z-20">
        <div>
            <div class="p-6 flex items-center space-x-3 border-b border-slate-800">
                <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-indigo-500 to-violet-500 flex items-center justify-center text-white font-bold text-xl shadow-lg shadow-indigo-500/30">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <div>
                    <h1 class="font-bold text-white text-lg tracking-wide">Study Hub</h1>
                    <span class="text-xs text-indigo-400 font-medium">Pro Edition v2.0</span>
                </div>
            </div>
            
            <nav class="p-4 space-y-1.5">
                <button onclick="switchTab('dashboard')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-semibold transition-all bg-indigo-600 text-white shadow-lg shadow-indigo-600/20" data-tab="dashboard">
                    <i class="fa-solid fa-house-chimney w-5"></i><span>Tổng quan</span>
                </button>
                <button onclick="switchTab('pomodoro')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all hover:bg-slate-800 hover:text-white" data-tab="pomodoro">
                    <i class="fa-solid fa-clock w-5"></i><span>Đồng hồ Pomodoro</span>
                </button>
                <button onclick="switchTab('tasks')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all hover:bg-slate-800 hover:text-white" data-tab="tasks">
                    <i class="fa-solid fa-list-check w-5"></i><span>Việc cần làm</span>
                </button>
                <button onclick="switchTab('schedule')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all hover:bg-slate-800 hover:text-white" data-tab="schedule">
                    <i class="fa-solid fa-calendar-days w-5"></i><span>Thời khóa biểu</span>
                </button>
                <button onclick="switchTab('flashcard')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all hover:bg-slate-800 hover:text-white" data-tab="flashcard">
                    <i class="fa-solid fa-clone w-5"></i><span>Flashcard Học tập</span>
                </button>
                <button onclick="switchTab('notes')" class="nav-btn w-full flex items-center space-x-3 px-4 py-3 rounded-xl text-sm font-medium transition-all hover:bg-slate-800 hover:text-white" data-tab="notes">
                    <i class="fa-solid fa-note-sticky w-5"></i><span>Sổ ghi chú & AI</span>
                </button>
            </nav>
        </div>

        <div class="p-4 border-t border-slate-800">
            <div class="bg-slate-800/60 p-3.5 rounded-xl border border-slate-700/50">
                <div class="flex items-center space-x-3">
                    <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?w=100&auto=format&fit=crop&q=80" class="w-9 h-9 rounded-full object-cover ring-2 ring-indigo-500" alt="Avatar">
                    <div class="overflow-hidden">
                        <p class="text-xs font-bold text-white truncate">Học viên ưu tú</p>
                        <p class="text-[11px] text-emerald-400 font-medium flex items-center gap-1">
                            <span class="w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span> Đang trực tuyến
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </aside>

    <!-- Main Content Area -->
    <div class="flex-1 flex flex-col h-screen overflow-hidden">
        
        <!-- Top Navbar -->
        <header class="h-16 bg-white border-b border-slate-200 flex items-center justify-between px-6 z-10 shrink-0">
            <div class="flex items-center space-x-4">
                <button onclick="toggleMobileMenu()" class="md:hidden text-slate-600 hover:text-slate-900 text-lg">
                    <i class="fa-solid fa-bars"></i>
                </button>
                <div>
                    <h2 id="page-title" class="font-bold text-slate-800 text-lg">Tổng quan học tập</h2>
                    <p id="current-date" class="text-xs text-slate-500 font-medium"></p>
                </div>
            </div>

            <div class="flex items-center space-x-4">
                <!-- Quick Pomodoro Widget Pill -->
                <div class="hidden sm:flex items-center space-x-2 bg-indigo-50 border border-indigo-100 px-3 py-1.5 rounded-full text-indigo-700 font-semibold text-sm">
                    <i class="fa-solid fa-stopwatch animate-spin"></i>
                    <span id="quick-timer-display">25:00</span>
                </div>
                <button onclick="triggerConfetti()" class="bg-amber-50 hover:bg-amber-100 text-amber-600 border border-amber-200 px-3.5 py-1.5 rounded-xl text-sm font-semibold transition flex items-center gap-1.5 shadow-sm">
                    <i class="fa-solid fa-fire"></i> <span id="streak-count">5</span> Ngày chuỗi
                </button>
            </div>
        </header>

        <!-- Scrollable Body Views -->
        <main class="flex-1 overflow-y-auto p-6 bg-slate-50">
            
            <!-- TAB 1: DASHBOARD -->
            <div id="tab-dashboard" class="tab-content active space-y-6">
                <!-- Welcome Banner -->
                <div class="bg-gradient-to-r from-indigo-600 via-violet-600 to-purple-600 rounded-2xl p-6 text-white shadow-xl relative overflow-hidden">
                    <div class="absolute -right-10 -bottom-10 w-64 h-64 bg-white/10 rounded-full blur-2xl pointer-events-none"></div>
                    <div class="max-w-xl relative z-10">
                        <span class="bg-white/20 text-white text-xs font-semibold px-3 py-1 rounded-full uppercase tracking-wider">Hôm nay tuyệt vời!</span>
                        <h3 class="text-2xl font-bold mt-3">Chào mừng trở lại, bạn đã sẵn sàng bứt phá chưa?</h3>
                        <p class="text-indigo-100 text-sm mt-1">Hôm nay bạn có <span id="dash-task-count" class="font-bold underline">0</span> việc cần hoàn thành. Hãy bắt đầu với mục tiêu quan trọng nhất.</p>
                        <div class="mt-5 flex items-center space-x-3">
                            <button onclick="switchTab('pomodoro')" class="bg-white text-indigo-700 px-4 py-2 rounded-xl text-sm font-bold shadow-md hover:bg-indigo-50 transition">
                                <i class="fa-solid fa-play mr-1.5"></i> Bắt đầu Pomodoro
                            </button>
                            <button onclick="switchTab('tasks')" class="bg-indigo-500/50 hover:bg-indigo-500/70 text-white border border-white/20 px-4 py-2 rounded-xl text-sm font-semibold transition">
                                Quản lý công việc
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Stats Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-5">
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Thời gian học</p>
                            <h4 class="text-2xl font-extrabold text-slate-800 mt-1"><span id="stat-study-time">3.5</span> giờ</h4>
                            <span class="text-xs text-emerald-600 font-semibold mt-1 inline-block"><i class="fa-solid fa-arrow-up"></i> +12% so với hôm qua</span>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-indigo-50 text-indigo-600 flex items-center justify-center text-xl shadow-inner">
                            <i class="fa-solid fa-clock-rotate-left"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Việc hoàn thành</p>
                            <h4 class="text-2xl font-extrabold text-slate-800 mt-1"><span id="stat-done-tasks">0</span>/<span id="stat-total-tasks">0</span></h4>
                            <span class="text-xs text-indigo-600 font-semibold mt-1 inline-block">Tiến độ hôm nay</span>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl shadow-inner">
                            <i class="fa-solid fa-circle-check"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Flashcard đã ôn</p>
                            <h4 class="text-2xl font-extrabold text-slate-800 mt-1">24 thẻ</h4>
                            <span class="text-xs text-amber-600 font-semibold mt-1 inline-block"><i class="fa-solid fa-fire"></i> Ghi nhớ tốt</span>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-amber-50 text-amber-600 flex items-center justify-center text-xl shadow-inner">
                            <i class="fa-solid fa-brain"></i>
                        </div>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-100 shadow-sm flex items-center justify-between">
                        <div>
                            <p class="text-xs font-bold uppercase tracking-wider text-slate-400">Hiệu suất tuần</p>
                            <h4 class="text-2xl font-extrabold text-slate-800 mt-1">88%</h4>
                            <span class="text-xs text-emerald-600 font-semibold mt-1 inline-block"><i class="fa-solid fa-arrow-up"></i> Rất cao</span>
                        </div>
                        <div class="w-12 h-12 rounded-2xl bg-purple-50 text-purple-600 flex items-center justify-center text-xl shadow-inner">
                            <i class="fa-solid fa-chart-line"></i>
                        </div>
                    </div>
                </div>

                <!-- Charts & Quick Tasks Row -->
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <div class="lg:col-span-2 bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-bold text-slate-800">Biểu đồ thời gian tập trung (Giờ)</h3>
                            <span class="text-xs bg-slate-100 text-slate-600 px-2.5 py-1 rounded-lg font-semibold">Tuần này</span>
                        </div>
                        <div class="h-64">
                            <canvas id="studyChart"></canvas>
                        </div>
                    </div>

                    <!-- Quick Task Checklist -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm flex flex-col">
                        <h3 class="font-bold text-slate-800 mb-3">Nhiệm vụ nhanh</h3>
                        <div id="dash-task-list" class="space-y-2.5 flex-1 overflow-y-auto max-h-[220px]">
                            <!-- Rendered by JS -->
                        </div>
                        <div class="mt-4 pt-3 border-t border-slate-100">
                            <button onclick="switchTab('tasks')" class="w-full py-2 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 font-semibold rounded-xl text-sm transition text-center">
                                Xem tất cả việc cần làm
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 2: POMODORO -->
            <div id="tab-pomodoro" class="tab-content">
                <div class="max-w-xl mx-auto bg-white p-8 rounded-2xl border border-slate-100 shadow-xl text-center space-y-6">
                    <!-- Mode Buttons -->
                    <div class="inline-flex bg-slate-100 p-1.5 rounded-2xl space-x-1">
                        <button onclick="setPomodoroMode('pomodoro', 25)" id="btn-mode-pomodoro" class="px-5 py-2 rounded-xl text-sm font-bold bg-white text-indigo-600 shadow-sm transition">Tập trung (25m)</button>
                        <button onclick="setPomodoroMode('shortBreak', 5)" id="btn-mode-shortBreak" class="px-5 py-2 rounded-xl text-sm font-medium text-slate-600 hover:text-slate-900 transition">Nghỉ ngắn (5m)</button>
                        <button onclick="setPomodoroMode('longBreak', 15)" id="btn-mode-longBreak" class="px-5 py-2 rounded-xl text-sm font-medium text-slate-600 hover:text-slate-900 transition">Nghỉ dài (15m)</button>
                    </div>

                    <!-- Timer Circle / Display -->
                    <div class="relative w-64 h-64 mx-auto flex items-center justify-center rounded-full bg-gradient-to-tr from-indigo-50 to-purple-50 border-8 border-indigo-100 shadow-inner">
                        <div class="text-center">
                            <h2 id="timer-display" class="text-5xl font-extrabold text-slate-800 tracking-wider">25:00</h2>
                            <p id="timer-status" class="text-sm font-semibold text-indigo-600 mt-2 uppercase tracking-wide">Sẵn sàng tập trung</p>
                        </div>
                    </div>

                    <!-- Controls -->
                    <div class="flex items-center justify-center space-x-4">
                        <button onclick="toggleTimer()" id="btn-start-pause" class="w-36 py-3.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-2xl shadow-lg shadow-indigo-600/30 transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-play" id="timer-icon"></i> <span id="timer-btn-text">Bắt đầu</span>
                        </button>
                        <button onclick="resetTimer()" class="px-5 py-3.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold rounded-2xl transition">
                            <i class="fa-solid fa-rotate-right"></i>
                        </button>
                    </div>

                    <!-- Ambient Sounds -->
                    <div class="pt-6 border-t border-slate-100">
                        <h4 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-3">Âm thanh môi trường tập trung</h4>
                        <div class="grid grid-cols-3 gap-3">
                            <button onclick="toggleSound('rain')" id="sound-rain" class="p-3 rounded-xl border border-slate-200 hover:border-indigo-400 text-sm font-semibold text-slate-700 transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-cloud-rain text-indigo-500"></i> Tiếng mưa
                            </button>
                            <button onclick="toggleSound('cafe')" id="sound-cafe" class="p-3 rounded-xl border border-slate-200 hover:border-indigo-400 text-sm font-semibold text-slate-700 transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-mug-hot text-amber-500"></i> Quán cà phê
                            </button>
                            <button onclick="toggleSound('lofi')" id="sound-lofi" class="p-3 rounded-xl border border-slate-200 hover:border-indigo-400 text-sm font-semibold text-slate-700 transition flex items-center justify-center gap-2">
                                <i class="fa-solid fa-headphones text-purple-500"></i> Lofi Beats
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 3: TASKS -->
            <div id="tab-tasks" class="tab-content">
                <div class="max-w-3xl mx-auto space-y-6">
                    <!-- Add Task Form -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
                        <h3 class="font-bold text-slate-800 mb-4">Thêm công việc mới</h3>
                        <form id="task-form" onsubmit="addTask(event)" class="space-y-4">
                            <div>
                                <input type="text" id="task-title" placeholder="Tên công việc cần làm (VD: Ôn tập chương 3 Toán cao cấp)..." required class="w-full px-4 py-3 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm">
                            </div>
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Mức độ ưu tiên</label>
                                    <select id="task-priority" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                                        <option value="high">Cao (Gấp)</option>
                                        <option value="medium" selected>Trung bình</option>
                                        <option value="low">Thấp</option>
                                    </select>
                                </div>
                                <div>
                                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Hạn chót</label>
                                    <input type="date" id="task-date" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                                </div>
                            </div>
                            <div class="flex justify-end">
                                <button type="submit" class="px-6 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl text-sm transition shadow-md shadow-indigo-600/20">
                                    Thêm việc ngay
                                </button>
                            </div>
                        </form>
                    </div>

                    <!-- Task List Filters & Items -->
                    <div class="bg-white p-6 rounded-2xl border border-slate-100 shadow-sm">
                        <div class="flex items-center justify-between mb-4">
                            <h3 class="font-bold text-slate-800">Danh sách công việc (<span id="total-task-badge">0</span>)</h3>
                            <div class="flex space-x-2">
                                <button onclick="filterTasks('all')" class="filter-btn px-3 py-1.5 rounded-lg text-xs font-bold bg-indigo-50 text-indigo-600" data-filter="all">Tất cả</button>
                                <button onclick="filterTasks('active')" class="filter-btn px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-100" data-filter="active">Chưa xong</button>
                                <button onclick="filterTasks('completed')" class="filter-btn px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-100" data-filter="completed">Đã xong</button>
                            </div>
                        </div>

                        <div id="full-task-list" class="space-y-3">
                            <!-- Tasks injected here -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- TAB 4: SCHEDULE -->
            <div id="tab-schedule" class="tab-content">
                <div class="max-w-4xl mx-auto bg-white p-6 rounded-2xl border border-slate-100 shadow-sm space-y-6">
                    <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
                        <div>
                            <h3 class="font-bold text-slate-800 text-lg">Thời khóa biểu tuần</h3>
                            <p class="text-xs text-slate-500 font-medium">Quản lý lịch học và môn học hàng tuần của bạn</p>
                        </div>
                        <button onclick="openScheduleModal()" class="px-4 py-2 bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-bold rounded-xl shadow-md transition flex items-center gap-2">
                            <i class="fa-solid fa-plus"></i> Thêm môn học
                        </button>
                    </div>

                    <!-- Schedule Table Grid -->
                    <div class="overflow-x-auto">
                        <table class="w-full border-collapse">
                            <thead>
                                <tr class="bg-slate-50 border-b border-slate-200 text-left text-xs font-bold uppercase text-slate-500">
                                    <th class="p-3 w-28">Thứ / Giờ</th>
                                    <th class="p-3">Sáng (07:30 - 11:30)</th>
                                    <th class="p-3">Chiều (13:30 - 17:00)</th>
                                    <th class="p-3">Tối (19:00 - 21:30)</th>
                                </tr>
                            </thead>
                            <tbody id="schedule-table-body" class="divide-y divide-slate-100 text-sm">
                                <!-- Generated by JS -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- TAB 5: FLASHCARD -->
            <div id="tab-flashcard" class="tab-content">
                <div class="max-w-xl mx-auto space-y-6 text-center">
                    <div>
                        <h3 class="font-bold text-slate-800 text-xl">Thẻ ghi nhớ Flashcard</h3>
                        <p class="text-xs text-slate-500 font-medium mt-1">Ôn tập kiến thức nhanh bằng phương pháp lật thẻ</p>
                    </div>

                    <!-- Card Box -->
                    <div onclick="flipCard()" class="relative w-full h-72 bg-gradient-to-tr from-indigo-600 to-violet-600 text-white rounded-3xl p-8 shadow-2xl cursor-pointer flex flex-col justify-between items-center transition-all duration-300 transform hover:scale-[1.02]">
                        <div class="w-full flex justify-between items-center text-xs font-bold tracking-wider uppercase text-indigo-200">
                            <span>Chủ đề: Tiếng Anh / Từ vựng</span>
                            <span id="card-counter">Thẻ 1 / 4</span>
                        </div>
                        
                        <div class="my-auto">
                            <h2 id="card-text" class="text-2xl font-bold">Resilience</h2>
                            <p id="card-subtext" class="text-sm text-indigo-200 mt-2 italic">(Bấm vào thẻ để xem nghĩa)</p>
                        </div>

                        <div class="text-xs text-indigo-200 font-medium bg-white/10 px-3 py-1.5 rounded-full">
                            <i class="fa-solid fa-rotate mr-1"></i> Bấm để lật
                        </div>
                    </div>

                    <!-- Card Navigation Controls -->
                    <div class="flex items-center justify-center space-x-4">
                        <button onclick="prevCard()" class="px-5 py-2.5 bg-white border border-slate-200 hover:bg-slate-50 text-slate-700 font-bold rounded-xl shadow-sm transition">
                            <i class="fa-solid fa-arrow-left mr-1"></i> Trước
                        </button>
                        <button onclick="nextCard()" class="px-5 py-2.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl shadow-md transition">
                            Tiếp theo <i class="fa-solid fa-arrow-right ml-1"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- TAB 6: NOTES -->
            <div id="tab-notes" class="tab-content">
                <div class="max-w-4xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- Note List Sidebar -->
                    <div class="bg-white p-4 rounded-2xl border border-slate-100 shadow-sm flex flex-col h-[500px]">
                        <div class="flex items-center justify-between mb-3 pb-2 border-b border-slate-100">
                            <h3 class="font-bold text-slate-800 text-sm">Sổ ghi chú</h3>
                            <button onclick="addNewNote()" class="w-8 h-8 rounded-xl bg-indigo-50 hover:bg-indigo-100 text-indigo-600 font-bold flex items-center justify-center transition">
                                <i class="fa-solid fa-plus text-xs"></i>
                            </button>
                        </div>
                        <div id="note-list" class="space-y-2 flex-1 overflow-y-auto">
                            <!-- Notes list items -->
                        </div>
                    </div>

                    <!-- Note Editor Area -->
                    <div class="md:col-span-2 bg-white p-6 rounded-2xl border border-slate-100 shadow-sm flex flex-col h-[500px]">
                        <input type="text" id="note-title-input" oninput="saveCurrentNote()" placeholder="Tiêu đề ghi chú..." class="font-bold text-lg text-slate-800 border-none focus:outline-none mb-3 pb-2 border-b border-slate-100">
                        <textarea id="note-content-input" oninput="saveCurrentNote()" placeholder="Viết ghi chú, công thức, tóm tắt bài học tại đây..." class="w-full flex-1 resize-none border-none focus:outline-none text-sm text-slate-700 leading-relaxed"></textarea>
                        
                        <div class="pt-3 border-t border-slate-100 flex items-center justify-between text-xs text-slate-400">
                            <span id="note-saved-status">Tự động lưu thay đổi</span>
                            <button onclick="deleteCurrentNote()" class="text-rose-500 hover:text-rose-700 font-semibold transition">
                                <i class="fa-solid fa-trash mr-1"></i> Xóa ghi chú
                            </button>
                        </div>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- Add Schedule Modal -->
    <div id="schedule-modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-2xl space-y-4">
            <div class="flex items-center justify-between pb-3 border-b border-slate-100">
                <h3 class="font-bold text-slate-800 text-lg">Thêm môn học vào thời khóa biểu</h3>
                <button onclick="closeScheduleModal()" class="text-slate-400 hover:text-slate-600"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <form id="schedule-form" onsubmit="addScheduleItem(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Thứ trong tuần</label>
                    <select id="sch-day" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                        <option value="Thứ 2">Thứ 2</option>
                        <option value="Thứ 3">Thứ 3</option>
                        <option value="Thứ 4">Thứ 4</option>
                        <option value="Thứ 5">Thứ 5</option>
                        <option value="Thứ 6">Thứ 6</option>
                        <option value="Thứ 7">Thứ 7</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Buổi học</label>
                    <select id="sch-session" class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm bg-white">
                        <option value="Sáng">Sáng (07:30 - 11:30)</option>
                        <option value="Chiều">Chiều (13:30 - 17:00)</option>
                        <option value="Tối">Tối (19:00 - 21:30)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-bold text-slate-500 uppercase mb-1">Tên môn học</label>
                    <input type="text" id="sch-subject" placeholder="VD: Lập trình Web nâng cao" required class="w-full px-4 py-2.5 rounded-xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-sm">
                </div>
                <div class="flex justify-end space-x-3 pt-2">
                    <button type="button" onclick="closeScheduleModal()" class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold rounded-xl text-sm transition">Hủy</button>
                    <button type="submit" class="px-5 py-2 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl text-sm transition">Thêm lịch</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Script Application Logic -->
    <script>
        // --- Init Date ---
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        document.getElementById('current-date').innerText = new Date().toLocaleDateString('vi-VN', options);

        // --- Tabs Navigation ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(el => {
                el.classList.remove('bg-indigo-600', 'text-white', 'shadow-lg', 'shadow-indigo-600/20');
                el.classList.add('hover:bg-slate-800', 'hover:text-white');
            });

            document.getElementById(`tab-${tabId}`).classList.add('active');
            const activeBtn = document.querySelector(`[data-tab="${tabId}"]`);
            if(activeBtn) {
                activeBtn.classList.add('bg-indigo-600', 'text-white', 'shadow-lg', 'shadow-indigo-600/20');
                activeBtn.classList.remove('hover:bg-slate-800', 'hover:text-white');
            }

            const titles = {
                'dashboard': 'Tổng quan học tập',
                'pomodoro': 'Đồng hồ Pomodoro tập trung',
                'tasks': 'Quản lý việc cần làm',
                'schedule': 'Thời khóa biểu tuần',
                'flashcard': 'Thẻ ghi nhớ Flashcard',
                'notes': 'Sổ ghi chú học tập'
            };
            document.getElementById('page-title').innerText = titles[tabId] || 'Study Hub';
        }

        function toggleMobileMenu() {
            const sidebar = document.querySelector('aside');
            sidebar.classList.toggle('hidden');
        }

        // --- Data State ---
        let tasks = JSON.parse(localStorage.getItem('study_tasks')) || [
            { id: 1, title: 'Hoàn thành bài tập Cấu trúc dữ liệu', priority: 'high', completed: false, date: '2026-06-10' },
            { id: 2, title: 'Đọc 20 trang sách tiếng Anh', priority: 'medium', completed: true, date: '2026-06-10' },
            { id: 3, title: 'Chuẩn bị slide thuyết trình nhóm', priority: 'high', completed: false, date: '2026-06-12' }
        ];

        let schedule = JSON.parse(localStorage.getItem('study_schedule')) || [
            { day: 'Thứ 2', session: 'Sáng', subject: 'Toán cao cấp C3' },
            { day: 'Thứ 3', session: 'Chiều', subject: 'Lập trình Web nâng cao' },
            { day: 'Thứ 4', session: 'Sáng', subject: 'Cơ sở dữ liệu phân tán' },
            { day: 'Thứ 5', session: 'Tối', subject: 'Tiếng Anh giao tiếp' },
            { day: 'Thứ 6', session: 'Chiều', subject: 'Kiến trúc máy tính' }
        ];

        let notes = JSON.parse(localStorage.getItem('study_notes')) || [
            { id: 1, title: 'Công thức Toán cao cấp', content: 'Đạo hàm, tích phân từng phần, chuỗi số Fourier...' },
            { id: 2, title: 'Từ vựng IELTS Chủ đề Environment', content: 'Sustainability, Renewable energy, Carbon footprint...' }
        ];
        let currentNoteId = notes.length > 0 ? notes[0].id : null;

        // --- Tasks Functions ---
        function saveTasks() {
            localStorage.setItem('study_tasks', JSON.stringify(tasks));
            renderTasks();
        }

        function addTask(e) {
            e.preventDefault();
            const title = document.getElementById('task-title').value;
            const priority = document.getElementById('task-priority').value;
            const date = document.getElementById('task-date').value;

            tasks.unshift({ id: Date.now(), title, priority, completed: false, date });
            document.getElementById('task-form').reset();
            saveTasks();
        }

        function toggleTask(id) {
            tasks = tasks.map(t => t.id === id ? { ...t, completed: !t.completed } : t);
            saveTasks();
        }

        function deleteTask(id) {
            tasks = tasks.filter(t => t.id !== id);
            saveTasks();
        }

        let currentFilter = 'all';
        function filterTasks(filter) {
            currentFilter = filter;
            document.querySelectorAll('.filter-btn').forEach(btn => {
                if(btn.dataset.filter === filter) {
                    btn.className = 'filter-btn px-3 py-1.5 rounded-lg text-xs font-bold bg-indigo-50 text-indigo-600';
                } else {
                    btn.className = 'filter-btn px-3 py-1.5 rounded-lg text-xs font-medium text-slate-600 hover:bg-slate-100';
                }
            });
            renderTasks();
        }

        function renderTasks() {
            // Dashboard quick list
            const dashList = document.getElementById('dash-task-list');
            const uncompletedTasks = tasks.filter(t => !t.completed);
            document.getElementById('dash-task-count').innerText = uncompletedTasks.length;
            document.getElementById('stat-total-tasks').innerText = tasks.length;
            document.getElementById('stat-done-tasks').innerText = tasks.filter(t => t.completed).length;

            dashList.innerHTML = uncompletedTasks.length === 0 
                ? '<p class="text-xs text-slate-400 text-center py-4">Tuyệt vời! Bạn đã hoàn thành tất cả việc cần làm.</p>'
                : uncompletedTasks.slice(0, 4).map(t => `
                    <div class="flex items-center justify-between p-2.5 bg-slate-50 rounded-xl border border-slate-100 text-sm">
                        <div class="flex items-center space-x-2.5 overflow-hidden">
                            <input type="checkbox" onclick="toggleTask(${t.id})" class="w-4 h-4 rounded text-indigo-600 focus:ring-indigo-500 cursor-pointer">
                            <span class="truncate font-medium text-slate-700 text-xs">${t.title}</span>
                        </div>
                        <span class="text-[10px] px-2 py-0.5 rounded font-bold uppercase ${t.priority === 'high' ? 'bg-rose-50 text-rose-600' : 'bg-amber-50 text-amber-600'}">${t.priority}</span>
                    </div>
                `).join('');

            // Full task list
            const fullList = document.getElementById('full-task-list');
            document.getElementById('total-task-badge').innerText = tasks.length;

            let filtered = tasks;
            if (currentFilter === 'active') filtered = tasks.filter(t => !t.completed);
            if (currentFilter === 'completed') filtered = tasks.filter(t => t.completed);

            fullList.innerHTML = filtered.length === 0
                ? '<p class="text-xs text-slate-400 text-center py-8">Không có công việc nào trong danh mục này.</p>'
                : filtered.map(t => `
                    <div class="flex items-center justify-between p-3.5 bg-slate-50/70 hover:bg-slate-50 rounded-xl border border-slate-100 transition">
                        <div class="flex items-center space-x-3 overflow-hidden">
                            <input type="checkbox" ${t.completed ? 'checked' : ''} onclick="toggleTask(${t.id})" class="w-5 h-5 rounded text-indigo-600 focus:ring-indigo-500 cursor-pointer">
                            <div>
                                <p class="text-sm font-semibold text-slate-800 ${t.completed ? 'line-through text-slate-400' : ''}">${t.title}</p>
                                ${t.date ? `<span class="text-[11px] text-slate-400"><i class="fa-regular fa-calendar mr-1"></i> ${t.date}</span>` : ''}
                            </div>
                        </div>
                        <div class="flex items-center space-x-3">
                            <span class="text-[10px] px-2.5 py-1 rounded-lg font-bold uppercase ${t.priority === 'high' ? 'bg-rose-50 text-rose-600' : t.priority === 'medium' ? 'bg-amber-50 text-amber-600' : 'bg-emerald-50 text-emerald-600'}">${t.priority}</span>
                            <button onclick="deleteTask(${t.id})" class="text-slate-400 hover:text-rose-600 transition"><i class="fa-solid fa-trash-can text-sm"></i></button>
                        </div>
                    </div>
                `).join('');
        }

        // --- Pomodoro Timer ---
        let timerMinutes = 25;
        let timerSeconds = 0;
        let timerInterval = null;
        let isTimerRunning = false;
        let currentMode = 'pomodoro';

        function setPomodoroMode(mode, mins) {
            clearInterval(timerInterval);
            isTimerRunning = false;
            currentMode = mode;
            timerMinutes = mins;
            timerSeconds = 0;
            updateTimerDisplay();

            document.querySelectorAll('[id^="btn-mode-"]').forEach(btn => {
                btn.className = 'px-5 py-2 rounded-xl text-sm font-medium text-slate-600 hover:text-slate-900 transition';
            });
            const activeBtn = document.getElementById(`btn-mode-${mode}`);
            activeBtn.className = 'px-5 py-2 rounded-xl text-sm font-bold bg-white text-indigo-600 shadow-sm transition';

            document.getElementById('timer-status').innerText = mode === 'pomodoro' ? 'Sẵn sàng tập trung' : 'Thời gian nghỉ ngơi';
            document.getElementById('timer-btn-text').innerText = 'Bắt đầu';
            document.getElementById('timer-icon').className = 'fa-solid fa-play';
        }

        function toggleTimer() {
            if (isTimerRunning) {
                clearInterval(timerInterval);
                isTimerRunning = false;
                document.getElementById('timer-btn-text').innerText = 'Tiếp tục';
                document.getElementById('timer-icon').className = 'fa-solid fa-play';
                document.getElementById('timer-status').innerText = 'Đã tạm dừng';
            } else {
                isTimerRunning = true;
                document.getElementById('timer-btn-text').innerText = 'Tạm dừng';
                document.getElementById('timer-icon').className = 'fa-solid fa-pause';
                document.getElementById('timer-status').innerText = 'Đang tập trung cao độ...';

                timerInterval = setInterval(() => {
                    if (timerSeconds === 0) {
                        if (timerMinutes === 0) {
                            clearInterval(timerInterval);
                            isTimerRunning = false;
                            alert('Hoàn thành phiên Pomodoro! Hãy nghỉ ngơi nào.');
                            triggerConfetti();
                            return;
                        }
                        timerMinutes--;
                        timerSeconds = 59;
                    } else {
                        timerSeconds--;
                    }
                    updateTimerDisplay();
                }, 1000);
            }
        }

        function resetTimer() {
            clearInterval(timerInterval);
            isTimerRunning = false;
            if (currentMode === 'pomodoro') timerMinutes = 25;
            else if (currentMode === 'shortBreak') timerMinutes = 5;
            else timerMinutes = 15;
            timerSeconds = 0;
            updateTimerDisplay();
            document.getElementById('timer-btn-text').innerText = 'Bắt đầu';
            document.getElementById('timer-icon').className = 'fa-solid fa-play';
            document.getElementById('timer-status').innerText = 'Đã đặt lại';
        }

        function updateTimerDisplay() {
            const mStr = String(timerMinutes).padStart(2, '0');
            const sStr = String(timerSeconds).padStart(2, '0');
            const timeStr = `${mStr}:${sStr}`;
            document.getElementById('timer-display').innerText = timeStr;
            document.getElementById('quick-timer-display').innerText = timeStr;
        }

        function toggleSound(type) {
            const btn = document.getElementById(`sound-${type}`);
            btn.classList.toggle('border-indigo-600');
            btn.classList.toggle('bg-indigo-50/50');
            // Mock audio toggle indicator
        }

        // --- Schedule Functions ---
        function saveSchedule() {
            localStorage.setItem('study_schedule', JSON.stringify(schedule));
            renderSchedule();
        }

        function openScheduleModal() { document.getElementById('schedule-modal').classList.remove('hidden'); }
        function closeScheduleModal() { document.getElementById('schedule-modal').classList.add('hidden'); }

        function addScheduleItem(e) {
            e.preventDefault();
            const day = document.getElementById('sch-day').value;
            const session = document.getElementById('sch-session').value;
            const subject = document.getElementById('sch-subject').value;

            schedule.push({ day, session, subject });
            document.getElementById('schedule-form').reset();
            closeScheduleModal();
            saveSchedule();
        }

        function renderSchedule() {
            const days = ['Thứ 2', 'Thứ 3', 'Thứ 4', 'Thứ 5', 'Thứ 6', 'Thứ 7'];
            const tbody = document.getElementById('schedule-table-body');
            
            tbody.innerHTML = days.map(day => {
                const morning = schedule.filter(s => s.day === day && s.session === 'Sáng').map(s => s.subject).join(', ') || '-';
                const afternoon = schedule.filter(s => s.day === day && s.session === 'Chiều').map(s => s.subject).join(', ') || '-';
                const evening = schedule.filter(s => s.day === day && s.session === 'Tối').map(s => s.subject).join(', ') || '-';

                return `
                    <tr class="hover:bg-slate-50/50">
                        <td class="p-3 font-bold text-indigo-600">${day}</td>
                        <td class="p-3 text-slate-700">${morning}</td>
                        <td class="p-3 text-slate-700">${afternoon}</td>
                        <td class="p-3 text-slate-700">${evening}</td>
                    </tr>
                `;
            }).join('');
        }

        // --- Flashcard Data & Logic ---
        const flashcards = [
            { term: 'Resilience', meaning: 'Khả năng phục hồi, kiên cường' },
            { term: 'Algorithmic Complexity', meaning: 'Độ phức tạp thuật toán (Big O Notation)' },
            { term: 'Photosynthesis', meaning: 'Quá trình quang hợp ở thực vật' },
            { term: 'Procrastination', meaning: 'Hành vi trì hoãn công việc' }
        ];
        let currentCardIndex = 0;
        let isCardFlipped = false;

        function updateCardDisplay() {
            isCardFlipped = false;
            const card = flashcards[currentCardIndex];
            document.getElementById('card-text').innerText = card.term;
            document.getElementById('card-subtext').innerText = '(Bấm vào thẻ để xem nghĩa)';
            document.getElementById('card-counter').innerText = `Thẻ ${currentCardIndex + 1} / ${flashcards.length}`;
        }

        function flipCard() {
            isCardFlipped = !isCardFlipped;
            const card = flashcards[currentCardIndex];
            if (isCardFlipped) {
                document.getElementById('card-text').innerText = card.meaning;
                document.getElementById('card-subtext').innerText = '(Nghĩa của từ)';
            } else {
                document.getElementById('card-text').innerText = card.term;
                document.getElementById('card-subtext').innerText = '(Bấm vào thẻ để xem nghĩa)';
            }
        }

        function nextCard() {
            currentCardIndex = (currentCardIndex + 1) % flashcards.length;
            updateCardDisplay();
        }

        function prevCard() {
            currentCardIndex = (currentCardIndex - 1 + flashcards.length) % flashcards.length;
            updateCardDisplay();
        }

        // --- Notes Functions ---
        function saveNotesStorage() {
            localStorage.setItem('study_notes', JSON.stringify(notes));
            renderNotesList();
        }

        function addNewNote() {
            const newN = { id: Date.now(), title: 'Ghi chú mới', content: '' };
            notes.unshift(newN);
            currentNoteId = newN.id;
            saveNotesStorage();
            loadCurrentNoteToEditor();
        }

        function selectNote(id) {
            currentNoteId = id;
            loadCurrentNoteToEditor();
        }

        function saveCurrentNote() {
            const title = document.getElementById('note-title-input').value;
            const content = document.getElementById('note-content-input').value;

            notes = notes.map(n => n.id === currentNoteId ? { ...n, title, content } : n);
            localStorage.setItem('study_notes', JSON.stringify(notes));
            renderNotesList(false);
        }

        function deleteCurrentNote() {
            if(notes.length <= 1) {
                alert('Cần giữ lại ít nhất một ghi chú!');
                return;
            }
            notes = notes.filter(n => n.id !== currentNoteId);
            currentNoteId = notes[0].id;
            saveNotesStorage();
            loadCurrentNoteToEditor();
        }

        function loadCurrentNoteToEditor() {
            const note = notes.find(n => n.id === currentNoteId) || notes[0];
            if(note) {
                document.getElementById('note-title-input').value = note.title;
                document.getElementById('note-content-input').value = note.content;
            }
            renderNotesList(false);
        }

        function renderNotesList(rebuild = true) {
            const listEl = document.getElementById('note-list');
            listEl.innerHTML = notes.map(n => `
                <div onclick="selectNote(${n.id})" class="p-3 rounded-xl cursor-pointer transition ${n.id === currentNoteId ? 'bg-indigo-50 border border-indigo-100 text-indigo-700 font-bold' : 'hover:bg-slate-50 text-slate-700'}">
                    <p class="text-xs truncate font-semibold">${n.title || 'Chưa có tiêu đề'}</p>
                </div>
            `).join('');
        }

        // --- Chart.js Init ---
        function initChart() {
            const ctx = document.getElementById('studyChart').getContext('2d');
            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Thứ 2', 'Thứ 3', 'Thứ 4', 'Thứ 5', 'Thứ 6', 'Thứ 7', 'CN'],
                    datasets: [{
                        label: 'Số giờ tập trung',
                        data: [3.5, 4.0, 2.5, 5.0, 4.5, 3.0, 2.0],
                        backgroundColor: '#6366f1',
                        borderRadius: 8,
                        barThickness: 24
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    scales: {
                        y: { beginAtZero: true, grid: { color: '#f1f5f9' } },
                        x: { grid: { display: false } }
                    }
                }
            });
        }

        function triggerConfetti() {
            // Simple visual reward feedback
            alert('🎉 Chúc mừng bạn đã hoàn thành xuất sắc mục tiêu học tập hôm nay!');
        }

        // --- Initialize Everything on Load ---
        window.onload = function() {
            renderTasks();
            renderSchedule();
            renderNotesList();
            loadCurrentNoteToEditor();
            initChart();
        };
    </script>
</body>
</html>
