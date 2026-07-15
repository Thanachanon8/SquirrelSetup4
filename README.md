[SquirrelSetup4.html](https://github.com/user-attachments/files/30054332/SquirrelSetup4.html)
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบเช็คชื่อสภา (ฝ่ายกิจกรรม) - โทนฟ้าขาวพรีเมียม</title>
    <!-- Tailwind CSS สำหรับจัดแต่งหน้าจอ UI อัจฉริยะ -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome สำหรับไอคอนประกอบความสวยงามและใช้งานง่าย -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts แบบพร้อมใช้งาน ฟอนต์ Prompt และ Sarabun -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600;700&family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        body {
            font-family: 'Prompt', 'Sarabun', sans-serif;
            background-color: #f0f7ff;
        }
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #93c5fd;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #2563eb;
        }
        .status-btn {
            transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .status-btn:active {
            transform: scale(0.92);
        }
        .pulse-glowing {
            box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.7);
            animation: pulse-ring 2s infinite;
        }
        @keyframes pulse-ring {
            0% { transform: scale(0.98); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0.4); }
            70% { transform: scale(1); box-shadow: 0 0 0 10px rgba(59, 130, 246, 0); }
            100% { transform: scale(0.98); box-shadow: 0 0 0 0 rgba(59, 130, 246, 0); }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col text-slate-800">

    <!-- แถบนำทางด้านบน (Premium White-Blue Navbar) -->
    <nav class="bg-white border-b border-blue-100 shadow-sm sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- ฝั่งซ้าย: โลโก้สภาฝ่ายกิจกรรม -->
                <div class="flex items-center space-x-3">
                    <div class="bg-blue-600 p-2.5 rounded-xl shadow-md text-white">
                        <i class="fa-solid fa-id-card-clip text-lg"></i>
                    </div>
                    <div>
                        <h1 class="text-base sm:text-lg font-bold tracking-wide text-blue-950">ระบบเช็คชื่อสภา</h1>
                        <div class="flex items-center space-x-1">
                            <span class="text-[10px] sm:text-xs text-blue-600 font-semibold">ฝ่ายกิจกรรมสภานักเรียน</span>
                            <span class="text-[10px] text-slate-400">|</span>
                            <span class="text-[10px] text-slate-500 font-medium bg-slate-100 px-1.5 py-0.5 rounded flex items-center">
                                <i class="fa-solid fa-code-branch mr-1 text-blue-500"></i>v7.0
                            </span>
                        </div>
                    </div>
                </div>

                <!-- ฝั่งขวา: แสดงผู้จัดทำและสิทธิการใช้งาน -->
                <div class="flex items-center space-x-2">
                    <!-- ข้อมูลผู้จัดทำ (รองประธาน) บน Navbar -->
                    <div class="hidden lg:flex items-center space-x-2 bg-gradient-to-r from-blue-50 to-sky-50 border border-blue-100 px-3 py-1.5 rounded-xl text-xs text-blue-950 font-medium">
                        <div class="w-2.5 h-2.5 rounded-full bg-blue-500 animate-pulse"></div>
                        <span>ผู้จัดทำ: <b>นายธนัชนนท์ ทาทอง (รองประธาน)</b></span>
                    </div>

                    <!-- แสดงสถานะบทบาทผู้ใช้งานปัจจุบัน -->
                    <div id="role-badge" class="bg-blue-50 border border-blue-100 px-3 py-1.5 rounded-xl text-xs font-semibold text-blue-700 flex items-center space-x-1.5">
                        <i id="role-icon" class="fa-solid fa-eye text-blue-500 animate-pulse"></i>
                        <span id="role-text">โหมดดูอย่างเดียว (เพื่อน)</span>
                    </div>
                    
                    <!-- เมนูปุ่มกดเข้าใช้ระบบ -->
                    <div class="flex items-center space-x-1" id="auth-buttons-container">
                        <button id="member-login-btn" onclick="openMemberLoginModal()" class="bg-blue-100 text-blue-700 hover:bg-blue-200 px-3 py-1.5 rounded-xl text-xs font-bold transition-all flex items-center space-x-1">
                            <i class="fa-solid fa-user"></i>
                            <span class="hidden sm:inline">เป็นสมาชิก</span>
                        </button>
                        <button id="auth-action-btn" onclick="openAuthModal()" class="bg-blue-600 text-white hover:bg-blue-700 px-3 py-1.5 rounded-xl text-xs font-bold shadow-sm transition-all flex items-center space-x-1">
                            <i class="fa-solid fa-lock"></i>
                            <span>หัวหน้าฝ่าย</span>
                        </button>
                    </div>
                    
                    <!-- ไฟสถานะเรียลไทม์คลาวด์ -->
                    <div class="hidden md:flex items-center space-x-1 bg-slate-50 px-2 py-1.5 rounded-xl border border-slate-100">
                        <span class="relative flex h-2 w-2">
                            <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
                            <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500" id="sync-dot"></span>
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <!-- พื้นที่ทำงานหลักของระบบ -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">
        
        <!-- แบนเนอร์ผู้จัดทำระบบ สำหรับอุปกรณ์มือถือ -->
        <div class="lg:hidden bg-gradient-to-r from-blue-600 to-sky-500 p-4 rounded-2xl text-white flex items-center justify-between shadow-md">
            <div class="space-y-0.5">
                <p class="text-[10px] text-blue-100 uppercase tracking-wider font-semibold">ผู้จัดทำและพัฒนาหลัก</p>
                <h4 class="text-sm font-bold">นายธนัชนนท์ ทาทอง</h4>
                <p class="text-xs text-sky-100 font-medium">รองประธานสภานักเรียน</p>
            </div>
            <div class="bg-white/10 p-2.5 rounded-xl">
                <i class="fa-solid fa-crown text-xl text-yellow-300"></i>
            </div>
        </div>

        <!-- กล่องแดชบอร์ดข้อมูลเฉพาะสมาชิก (เฉพาะฉัน) - จะแสดงเมื่อสมาชิกเข้าสู่ระบบสำเร็จ -->
        <div id="personal-stats-section" class="hidden bg-gradient-to-r from-blue-50 to-indigo-50 p-5 rounded-2xl border border-blue-200 space-y-3 shadow-md transition-all">
            <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                <div class="flex items-center space-x-3">
                    <div class="bg-blue-600 text-white p-2.5 rounded-xl shadow-sm">
                        <i class="fa-solid fa-user-check text-base"></i>
                    </div>
                    <div>
                        <h4 class="text-sm font-bold text-slate-800">แดชบอร์ดเฉพาะของฉัน (ข้อมูลสมาชิกสภา)</h4>
                        <p class="text-xs text-slate-500">สรุปผลการเข้าปฏิบัติงานของบัญชีคุณย้อนหลังทุกรอบกิจกรรม</p>
                    </div>
                </div>
                <!-- ปุ่มแก้ไขรูปภาพโปรไฟล์โดยเฉพาะ ใช้ง่าย และเห็นชัดเจน -->
                <button id="personal-edit-avatar-btn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-xl text-xs font-bold transition-all flex items-center justify-center space-x-1.5 shadow-sm">
                    <i class="fa-solid fa-image-portrait"></i>
                    <span>แก้ไขรูปภาพโปรไฟล์</span>
                </button>
            </div>
            <div class="grid grid-cols-3 gap-3 pt-1">
                <div class="bg-white p-3 rounded-xl border border-blue-100 text-center shadow-sm">
                    <span class="text-slate-400 text-[10px] sm:text-xs font-semibold block mb-0.5">ปฏิบัติงาน</span>
                    <span id="personal-stat-working" class="text-sm sm:text-lg font-bold text-emerald-600">0 ครั้ง</span>
                </div>
                <div class="bg-white p-3 rounded-xl border border-blue-100 text-center shadow-sm">
                    <span class="text-slate-400 text-[10px] sm:text-xs font-semibold block mb-0.5">ลาพัก</span>
                    <span id="personal-stat-leave" class="text-sm sm:text-lg font-bold text-amber-500">0 ครั้ง</span>
                </div>
                <!-- แสดงช่องจำนวนที่ขาดทำงานตามที่ต้องการเน้น -->
                <div class="bg-red-50/50 p-3 rounded-xl border border-rose-200 text-center shadow-sm">
                    <span class="text-rose-600 text-[10px] sm:text-xs font-bold block mb-0.5">
                        <i class="fa-solid fa-triangle-exclamation mr-1"></i>ขาดทำงาน
                    </span>
                    <span id="personal-stat-absent" class="text-sm sm:text-lg font-extrabold text-rose-600">0 ครั้ง</span>
                </div>
            </div>
        </div>

        <!-- แดชบอร์ดข้อมูลสถิติรวมของสภาประจำวัน (Dashboard Grid) -->
        <div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
            <!-- ยอดสมาชิกสภาทั้งหมด -->
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-blue-50 flex items-center space-x-4">
                <div class="p-3 bg-blue-50 text-blue-600 rounded-xl">
                    <i class="fa-solid fa-users text-2xl"></i>
                </div>
                <div>
                    <p class="text-xs text-slate-400 font-medium">สมาชิกทั้งหมด</p>
                    <h3 class="text-lg sm:text-xl font-bold text-slate-800" id="stat-total">0 คน</h3>
                </div>
            </div>
            <!-- ยอดผู้ทำงานวันนี้ -->
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-blue-50 flex items-center space-x-4">
                <div class="p-3 bg-emerald-50 text-emerald-600 rounded-xl">
                    <i class="fa-solid fa-briefcase text-2xl"></i>
                </div>
                <div>
                    <p class="text-xs text-slate-400 font-medium">ทำงาน</p>
                    <h3 class="text-lg sm:text-xl font-bold text-emerald-600" id="stat-working">0 คน</h3>
                </div>
            </div>
            <!-- ยอดผู้ที่ขอลา -->
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-blue-50 flex items-center space-x-4">
                <div class="p-3 bg-amber-50 text-amber-600 rounded-xl">
                    <i class="fa-solid fa-plane-departure text-2xl"></i>
                </div>
                <div>
                    <p class="text-xs text-slate-400 font-medium">ลา</p>
                    <h3 class="text-lg sm:text-xl font-bold text-amber-500" id="stat-leave">0 คน</h3>
                </div>
            </div>
            <!-- ยอดผู้ที่ไม่ทำงาน/ขาดงาน -->
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-blue-50 flex items-center space-x-4">
                <div class="p-3 bg-rose-50 text-rose-600 rounded-xl">
                    <i class="fa-solid fa-ban text-2xl"></i>
                </div>
                <div class="flex-grow">
                    <p class="text-xs text-slate-400 font-medium">ไม่ทำงาน</p>
                    <div class="flex items-baseline justify-between">
                        <h3 class="text-lg sm:text-xl font-bold text-rose-500" id="stat-not-working">0 คน</h3>
                        <span class="text-xs bg-blue-50 text-blue-700 px-2 rounded-full font-bold" id="stat-percent">0%</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- แถบเลือกกิจกรรมและการจัดการรอบแบบเรียงรายเดี่ยว -->
        <div class="bg-white p-5 rounded-2xl shadow-sm border border-blue-50 flex flex-col lg:flex-row lg:items-center justify-between gap-4">
            <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-3 flex-grow">
                <div class="min-w-[240px]">
                    <label class="block text-xs font-semibold text-slate-500 mb-1">รอบที่เช็คชื่อ / กิจกรรม</label>
                    <select id="session-select" class="w-full bg-slate-50 border border-slate-200 text-slate-700 py-2.5 px-3 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm font-medium">
                        <option value="">-- กำลังโหลดรอบกิจกรรม... --</option>
                    </select>
                </div>
                <!-- ปุ่มเสริมหัวหน้าฝ่ายในการจัดการรอบกิจกรรม -->
                <div id="admin-session-btns" class="flex items-end space-x-2 hidden">
                    <button onclick="openCreateSessionModal()" class="bg-blue-50 hover:bg-blue-100 text-blue-700 px-4 py-2.5 rounded-xl text-sm font-semibold transition-colors flex items-center space-x-1.5 h-11 mt-auto shadow-sm">
                        <i class="fa-solid fa-calendar-plus"></i>
                        <span>สร้างรอบใหม่</span>
                    </button>
                    <button onclick="triggerDeleteCurrentSession()" class="bg-rose-50 hover:bg-rose-100 text-rose-600 p-2.5 rounded-xl text-sm transition-colors h-11 flex items-center justify-center border border-rose-100" title="ลบรอบปัจจุบัน">
                        <i class="fa-solid fa-trash-can"></i>
                    </button>
                </div>
            </div>
            
            <!-- ปุ่มรีเซ็ตสำหรับแอดมิน (ลบคำสั่งเช็คทั้งหมดออกเพื่อกันสับสน) -->
            <div id="admin-batch-btns" class="flex flex-wrap items-center gap-2 hidden">
                <button onclick="triggerResetAllStatuses()" class="bg-slate-100 hover:bg-slate-200 text-slate-600 px-3.5 py-3 rounded-xl text-xs font-bold transition-all flex items-center space-x-1 border border-slate-200">
                    <i class="fa-solid fa-rotate-left"></i>
                    <span>รีเซ็ตทั้งหมด</span>
                </button>
            </div>

            <!-- ปุ่มดาวน์โหลดสรุปประวัติเป็น Excel สำหรับทุกคน -->
            <div>
                <button onclick="exportToCSV()" class="w-full bg-blue-50 hover:bg-blue-100 text-blue-700 px-4 py-3 rounded-xl text-xs font-bold transition-all flex items-center justify-center space-x-1 border border-blue-100">
                    <i class="fa-solid fa-file-excel text-base"></i>
                    <span>ส่งออกไฟล์ Excel (CSV)</span>
                </button>
            </div>
        </div>

        <!-- ตัวควบคุมตัวกรอง และ จัดการสมาชิกรายคน -->
        <div class="flex flex-col md:flex-row gap-4 items-center justify-between">
            <!-- ระบบค้นหา และ แท็บเลือกกรองระดับตำแหน่ง -->
            <div class="flex flex-col sm:flex-row items-stretch sm:items-center gap-3 w-full md:w-auto">
                <div class="relative flex-grow sm:max-w-xs">
                    <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-slate-400">
                        <i class="fa-solid fa-magnifying-glass text-sm"></i>
                    </span>
                    <input type="text" id="search-input" oninput="applyFilters()" placeholder="ค้นหารายชื่อสมาชิก..." class="w-full pl-9 pr-4 py-2.5 bg-white border border-blue-100 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                
                <!-- แท็บตัวกรองกลุ่มสภานักเรียน -->
                <div class="flex bg-white p-1 rounded-xl overflow-x-auto space-x-1 border border-blue-50">
                    <button onclick="setFilterRole('all')" id="filter-btn-all" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold transition-all bg-blue-600 text-white shadow-sm">ทั้งหมด</button>
                    <button onclick="setFilterRole('หัวหน้าฝ่าย')" id="filter-btn-head" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:text-slate-950 transition-all">หัวหน้าฝ่าย</button>
                    <button onclick="setFilterRole('รองหัวหน้าฝ่าย')" id="filter-btn-deputy" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:text-slate-950 transition-all">รองหัวหน้าฝ่าย</button>
                    <button onclick="setFilterRole('เลขาฝ่าย')" id="filter-btn-sec" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:text-slate-950 transition-all">เลขาฝ่าย</button>
                    <button onclick="setFilterRole('สมาชิก')" id="filter-btn-member" class="px-3.5 py-1.5 rounded-lg text-xs font-semibold text-slate-600 hover:text-slate-950 transition-all">สมาชิก</button>
                </div>
            </div>

            <!-- ปุ่มสำหรับแอดมินเข้าไปปรับปรุงรายชื่อ (สิทธิ์เฉพาะหัวหน้า) -->
            <button id="admin-manage-members-btn" onclick="openManageMembersModal()" class="w-full md:w-auto bg-blue-600 hover:bg-blue-700 text-white px-4 py-2.5 rounded-xl text-sm font-semibold transition-colors flex items-center justify-center space-x-1.5 shadow-sm hidden">
                <i class="fa-solid fa-users-gear"></i>
                <span>จัดการโครงสร้างสมาชิกสภา</span>
            </button>
        </div>

        <!-- กล่องสำหรับแสดงตารางสมาชิก (Desktop Table / Mobile Card Grid) -->
        <div class="bg-white rounded-2xl shadow-sm border border-blue-100 overflow-hidden">
            <!-- รูปแบบการแสดงผลบนหน้าจอคอมพิวเตอร์ (Desktop Table View) -->
            <div class="hidden md:block overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-50 text-slate-500 text-xs font-bold uppercase tracking-wider border-b border-blue-100">
                            <th class="py-4 px-6 text-center w-20">ลำดับ</th>
                            <th class="py-4 px-6 w-24">โปรไฟล์</th>
                            <th class="py-4 px-6">ชื่อ-สกุล</th>
                            <th class="py-4 px-6">ตำแหน่ง</th>
                            <th class="py-4 px-6 text-center w-[380px]">สถานะการทำงาน</th>
                        </tr>
                    </thead>
                    <tbody id="desktop-tbody" class="divide-y divide-slate-100">
                        <!-- รายชื่อถูกโหลดโดย JavaScript -->
                    </tbody>
                </table>
            </div>

            <!-- รูปแบบการแสดงผลบนสมาร์ตโฟน (Mobile Card View) -->
            <div id="mobile-card-container" class="md:hidden p-4 space-y-3">
                <!-- รายชื่อแบบการ์ดถูกโหลดโดย JavaScript -->
            </div>
            
            <!-- สถานะเมื่อไม่พบข้อมูลการค้นหา -->
            <div id="empty-state" class="hidden p-12 text-center">
                <div class="mx-auto w-16 h-16 bg-blue-50 rounded-full flex items-center justify-center mb-3 text-blue-400">
                    <i class="fa-solid fa-user-slash text-2xl"></i>
                </div>
                <p class="text-slate-500 font-semibold text-sm">ไม่พบข้อมูลรายชื่อสมาชิกสภา</p>
                <p class="text-xs text-slate-400 mt-1">ลองพิมพ์ชื่อตรวจสอบความถูกต้อง หรือเชื่อมต่ออินเทอร์เน็ตเพื่อดึงข้อมูลอีกครั้ง</p>
            </div>
        </div>

    </main>

    <!-- ส่วนท้ายหน้าเว็บ (Footer) -->
    <footer class="bg-blue-950 text-blue-200 py-8 mt-12 border-t border-blue-900">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 space-y-6">
            <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                <div>
                    <p class="font-semibold text-white text-base">ระบบเช็คชื่อสภาฝ่ายกิจกรรมสภานักเรียน</p>
                    <p class="mt-1 text-xs text-blue-300">ซิงค์แบบเรียลไทม์ผ่านคลาวด์อัตโนมัติบนโทนสีฟ้า-ขาว เพื่อความโปร่งใสและเป็นระเบียบ</p>
                </div>
                <!-- บัตรเกียรติยศผู้จัดทำระบบท้ายหน้าเว็บ -->
                <div class="bg-blue-900/40 border border-blue-800 p-4 rounded-2xl flex items-center space-x-3 max-w-sm w-full">
                    <div class="bg-blue-600/50 w-10 h-10 rounded-full flex items-center justify-center text-white text-lg">
                        <i class="fa-solid fa-crown text-yellow-300"></i>
                    </div>
                    <div>
                        <p class="text-[10px] text-blue-300 font-bold uppercase tracking-wide">ผู้พัฒนาและจัดทำระบบ</p>
                        <p class="text-sm font-bold text-white">นายธนัชนนท์ ทาทอง</p>
                        <p class="text-xs text-blue-200">รองประธานสภานักเรียน</p>
                    </div>
                </div>
            </div>
            
            <div class="pt-6 border-t border-blue-900/60 flex flex-col sm:flex-row justify-between items-center gap-4 text-xs">
                <span class="bg-blue-900/60 text-blue-100 px-3 py-1.5 rounded-md font-mono select-all border border-blue-800 text-[11px]">
                    แอปไอดี: <span id="current-user-id">กำลังโหลด...</span>
                </span>
                <span class="text-blue-400 text-[11px]">สภานักเรียนฝ่ายกิจกรรม © 2026. สงวนลิขสิทธิ์ความปลอดภัยระบบ</span>
            </div>
        </div>
    </footer>

    <!-- โมดอลป้อนรหัสความปลอดภัยเข้าใช้หัวหน้าฝ่าย (Passcode Auth Modal) -->
    <div id="auth-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-sm rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300">
            <div class="bg-blue-600 p-4 text-white flex items-center justify-between">
                <h3 class="font-bold text-base"><i class="fa-solid fa-lock mr-2"></i>เข้าสู่ระบบหัวหน้าฝ่าย</h3>
                <button onclick="closeAuthModal()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <div class="p-5 space-y-4">
                <div class="text-xs text-blue-700 bg-blue-50 p-3 rounded-xl border border-blue-100">
                    <p class="font-semibold mb-1">คำแนะนำสิทธิ์การเข้าใช้</p>
                    <p>รหัสผ่านนี้มีไว้สำหรับหัวหน้าฝ่ายกิจกรรมเพื่อใช้เช็คชื่อและจัดการข้อมูลสมาชิกสภาสภานักเรียน [cite: 1]</p>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">รหัสผ่านหัวหน้าฝ่าย (เริ่มต้น: 1234)</label>
                    <div class="relative">
                        <input type="password" id="auth-passcode" placeholder="ป้อนรหัสผ่าน..." class="w-full pl-3.5 pr-10 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm tracking-widest text-center font-bold">
                        <!-- ปุ่มตาสำหรับสลับแสดง/ซ่อนรหัสผ่าน -->
                        <button type="button" onclick="togglePasswordVisibility('auth-passcode', 'auth-passcode-eye')" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600 focus:outline-none">
                            <i id="auth-passcode-eye" class="fa-solid fa-eye-slash"></i>
                        </button>
                    </div>
                </div>
                <div class="flex space-x-3 pt-2">
                    <button onclick="closeAuthModal()" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 py-2.5 rounded-xl text-xs font-semibold transition-colors">ยกเลิก</button>
                    <button onclick="submitAuth()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors">ยืนยันและเข้าระบบ</button>
                </div>
            </div>
        </div>
    </div>

    <!-- โมดอลเข้าสู่ระบบของสมาชิกสภา / เพื่อน เพื่อเข้าไปแก้ไขรูปตัวเอง (ต้องกรอกรหัสผ่าน) -->
    <div id="member-login-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-sm rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300">
            <div class="bg-blue-600 p-4 text-white flex items-center justify-between">
                <h3 class="font-bold text-base"><i class="fa-solid fa-user-check mr-2"></i>เข้าสู่ระบบสมาชิก (เพื่อน)</h3>
                <button onclick="closeMemberLoginModal()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <div class="p-5 space-y-4">
                <div class="text-xs text-blue-700 bg-blue-50 p-3 rounded-xl border border-blue-100">
                    <p class="font-semibold mb-1">ยินดีต้อนรับเข้าใช้งาน!</p>
                    <p>เพื่อเข้าไปปรับแต่งรูปภาพโปรไฟล์และตรวจประวัติประเมินขาดทำงาน โปรดระบุรหัสความปลอดภัยของสภานักเรียน</p>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">เลือกชื่อบัญชีของคุณ</label>
                    <select id="member-login-select" class="w-full bg-slate-50 border border-slate-200 text-slate-700 py-2.5 px-3 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm font-medium">
                        <!-- โหลดรายการชื่อสมาชิกเข้ามา -->
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">รหัสผ่านสมาชิกสภา</label>
                    <div class="relative">
                        <input type="password" id="member-password-input" placeholder="ป้อนรหัสผ่าน..." class="w-full pl-3.5 pr-10 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm font-bold text-center">
                        <!-- ปุ่มตาสำหรับสลับแสดง/ซ่อนรหัสผ่านสำหรับสมาชิก -->
                        <button type="button" onclick="togglePasswordVisibility('member-password-input', 'member-password-eye')" class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600 focus:outline-none">
                            <i id="member-password-eye" class="fa-solid fa-eye-slash"></i>
                        </button>
                    </div>
                </div>
                <div class="flex space-x-3 pt-2">
                    <button onclick="closeMemberLoginModal()" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 py-2.5 rounded-xl text-xs font-semibold transition-colors">ยกเลิก</button>
                    <button onclick="submitMemberLogin()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors">เข้าใช้งาน</button>
                </div>
            </div>
        </div>
    </div>

    <!-- โมดอลเปลี่ยนรูปภาพโปรไฟล์ (Edit Avatar Modal - แฟนซีดีไซน์สุดเก๋ มีปุ่มชัดเจน) -->
    <div id="edit-avatar-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300">
            <div class="bg-blue-600 p-4 text-white flex items-center justify-between">
                <h3 class="font-bold text-base" id="avatar-modal-title"><i class="fa-solid fa-wand-magic-sparkles mr-2"></i>เครื่องมือปรับแต่งโปรไฟล์</h3>
                <button onclick="closeEditAvatarModal()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <div class="p-5 space-y-4 max-h-[80vh] overflow-y-auto">
                <div class="flex flex-col items-center space-y-3">
                    <!-- ส่วนแสดงภาพพรีวิวผลลัพธ์ -->
                    <div class="relative">
                        <img id="avatar-live-preview" src="" alt="Preview" class="w-24 h-24 rounded-full object-cover border-4 border-blue-100 shadow-md">
                        <span class="absolute bottom-0 right-0 bg-blue-600 text-white text-[10px] px-2 py-0.5 rounded-full font-bold shadow-sm">ตัวอย่าง</span>
                    </div>
                </div>

                <!-- 1. ตัวสร้างโปรไฟล์อิโมจิอัจฉริยะ (Emoji Maker - ง่ายที่สุด!) -->
                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200 space-y-3">
                    <p class="text-xs font-bold text-slate-600 flex items-center">
                        <i class="fa-solid fa-face-grin-stars text-amber-500 mr-1.5 text-sm"></i>
                        <span>วิธีที่ 1: สร้างรูปด้วย "อิโมจิ & สีพื้นหลัง"</span>
                    </p>
                    
                    <!-- เลือกสีพื้นหลัง -->
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 mb-1.5">เลือกสีพื้นหลังไล่เฉด (Gradient)</label>
                        <div class="grid grid-cols-5 gap-1.5" id="avatar-bg-grid">
                            <!-- โหลดด้วย JS -->
                        </div>
                    </div>

                    <!-- เลือกอิโมจิ -->
                    <div>
                        <label class="block text-[10px] font-bold text-slate-400 mb-1.5">เลือกตัวการ์ตูน / อิโมจิ</label>
                        <div class="grid grid-cols-6 gap-1.5 max-h-[110px] overflow-y-auto bg-white p-2 rounded-lg border border-slate-100 text-lg text-center" id="avatar-emoji-grid">
                            <!-- โหลดด้วย JS -->
                        </div>
                    </div>
                </div>

                <!-- 2. แนบลิงก์รูปภาพส่วนตัว -->
                <div class="space-y-1.5">
                    <label class="block text-xs font-bold text-slate-600 flex items-center">
                        <i class="fa-solid fa-link text-blue-500 mr-1.5"></i>
                        <span>วิธีที่ 2: วางลิงก์รูปภาพส่วนตัว (URL)</span>
                    </label>
                    <input type="text" id="edit-avatar-url" oninput="previewLiveAvatar(this.value)" placeholder="เช่น https://www.facebook.com/.../photo.jpg" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-xs">
                    <p class="text-[9px] text-slate-400"><i class="fa-solid fa-circle-info mr-1"></i>คุณสามารถนำภาพจากเว็บฝากรูปหรือแอปโซเชียลมาวางเพื่อตั้งรูปจริงได้</p>
                </div>

                <div class="flex space-x-3 pt-2">
                    <button onclick="closeEditAvatarModal()" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 py-2.5 rounded-xl text-xs font-semibold transition-colors">ยกเลิก</button>
                    <button id="save-avatar-btn" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors">บันทึกรูปโปรไฟล์</button>
                </div>
            </div>
        </div>
    </div>

    <!-- โมดอลสร้างรอบกิจกรรมการประชุมใหม่ (Create Session Modal) -->
    <div id="create-session-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300">
            <div class="bg-blue-600 p-4 text-white flex items-center justify-between">
                <h3 class="font-bold text-base"><i class="fa-solid fa-calendar-plus mr-2"></i>สร้างรอบกิจกรรมใหม่</h3>
                <button onclick="closeCreateSessionModal()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            <div class="p-5 space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">ชื่อรอบกิจกรรม / วันประชุมสภา</label>
                    <input type="text" id="new-session-name" placeholder="เช่น กิจกรรมกีฬาสี, การจัดเตรียมบอร์ดนิทรรศการ..." class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 mb-1">วันที่ปฏิบัติกิจกรรม</label>
                    <input type="date" id="new-session-date" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-blue-500 text-sm text-slate-700">
                </div>
                <div class="flex space-x-3 pt-2">
                    <button onclick="closeCreateSessionModal()" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 py-2.5 rounded-xl text-sm font-semibold transition-colors">ยกเลิก</button>
                    <button onclick="createNewSession()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-sm font-semibold transition-colors">เปิดรอบใหม่</button>
                </div>
            </div>
        </div>
    </div>

    <!-- โมดอลสำหรับการจัดการรายชื่อสมาชิก (Manage Members Modal - สำหรับแอดมินสิทธิ์เต็ม) -->
    <div id="manage-members-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-2xl rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300 flex flex-col max-h-[85vh]">
            <div class="bg-slate-900 p-4 text-white flex items-center justify-between flex-shrink-0">
                <h3 class="font-bold text-base"><i class="fa-solid fa-users-gear mr-2"></i>จัดการรายชื่อสมาชิกสภา & รูปภาพ</h3>
                <button onclick="closeManageMembersModal()" class="text-white/80 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            
            <!-- ส่วนลงทะเบียนหรือบันทึกสมาชิกรายใหม่ -->
            <div class="p-4 border-b border-slate-100 bg-slate-50 flex-shrink-0 space-y-3">
                <h4 class="text-xs font-bold text-slate-500 uppercase tracking-wider">เพิ่มหรือแก้ไขข้อมูลสมาชิกสภานักเรียน</h4>
                <div class="grid grid-cols-1 sm:grid-cols-4 gap-2">
                    <input type="text" id="new-member-name" placeholder="ชื่อ-นามสกุล..." class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <select id="new-member-role" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 text-slate-700">
                        <option value="สมาชิก">สมาชิก</option>
                        <option value="หัวหน้าฝ่าย">หัวหน้าฝ่าย</option>
                        <option value="รองหัวหน้าฝ่าย">รองหัวหน้าฝ่าย</option>
                        <option value="เลขาฝ่าย">เลขาฝ่าย</option>
                    </select>
                    <input type="text" id="new-member-avatar" placeholder="ลิงก์รูปภาพโปรไฟล์ (ถ้ามี)..." class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button onclick="addMember()" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-2 rounded-xl text-sm font-semibold transition-colors flex items-center justify-center space-x-1 shadow-sm">
                        <i class="fa-solid fa-plus"></i>
                        <span>บันทึกรายชื่อ</span>
                    </button>
                </div>
            </div>

            <!-- รายชื่อแบบตารางย่อยเลื่อนดูได้ -->
            <div class="flex-grow overflow-y-auto p-4">
                <table class="w-full text-left">
                    <thead>
                        <tr class="text-xs font-bold text-slate-400 border-b border-slate-100 uppercase tracking-wider pb-2">
                            <th class="pb-2 w-12 text-center">ลำดับ</th>
                            <th class="pb-2 w-16 text-center">โปรไฟล์</th>
                            <th class="pb-2">ชื่อ-สกุล</th>
                            <th class="pb-2">ตำแหน่ง</th>
                            <th class="pb-2 text-center w-24">จัดการ</th>
                        </tr>
                    </thead>
                    <tbody id="manage-members-tbody" class="divide-y divide-slate-100 text-sm">
                        <!-- โหลดรายการจัดการสมาชิกโดย Javascript -->
                    </tbody>
                </table>
            </div>
            
            <div class="p-4 bg-slate-50 border-t border-slate-100 flex flex-col sm:flex-row justify-between items-stretch sm:items-center gap-3 flex-shrink-0">
                <!-- ฟังก์ชันปรับแต่งรหัสผ่านหัวหน้าฝ่ายพร้อมระบบซ่อนตา -->
                <div class="flex items-center space-x-2">
                    <div class="relative">
                        <input type="password" id="change-passcode-input" placeholder="เปลี่ยนรหัสแอดมินใหม่..." class="px-3 pr-8 py-2 bg-white border border-slate-200 rounded-xl text-xs focus:outline-none focus:ring-1 focus:ring-blue-500 max-w-[180px]">
                        <button type="button" onclick="togglePasswordVisibility('change-passcode-input', 'change-passcode-eye')" class="absolute right-2 top-1/2 -translate-y-1/2 text-slate-400 hover:text-slate-600 focus:outline-none">
                            <i id="change-passcode-eye" class="fa-solid fa-eye-slash text-[10px]"></i>
                        </button>
                    </div>
                    <button onclick="changeAdminPasscode()" class="bg-slate-700 hover:bg-slate-800 text-white px-3.5 py-2 rounded-xl text-xs font-semibold transition-colors">แก้ไขรหัส</button>
                </div>
                <button onclick="closeManageMembersModal()" class="bg-slate-900 hover:bg-slate-950 text-white px-5 py-2.5 rounded-xl text-sm font-semibold transition-colors">เสร็จสิ้น</button>
            </div>
        </div>
    </div>

    <!-- ป๊อปอัพยืนยันความพึงพอใจการลบหรือดำเนินการต่างๆ (Custom Modal Confirm แทน confirm ดั้งเดิม) -->
    <div id="confirm-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-[60] flex items-center justify-center p-4 opacity-0 pointer-events-none transition-all duration-300">
        <div class="bg-white w-full max-w-sm rounded-2xl shadow-xl overflow-hidden transform scale-95 transition-all duration-300">
            <div class="p-6 text-center">
                <div class="mx-auto w-12 h-12 bg-blue-50 text-blue-600 rounded-full flex items-center justify-center mb-4 text-xl">
                    <i class="fa-solid fa-triangle-exclamation animate-bounce"></i>
                </div>
                <h3 class="text-base font-bold text-slate-900 mb-2" id="confirm-title">ยืนยันรายการ</h3>
                <p class="text-xs text-slate-500 mb-6 leading-relaxed" id="confirm-message">โปรดตรวจสอบรายละเอียดให้แน่ใจก่อนทำการกดยืนยัน</p>
                <div class="flex space-x-3">
                    <button onclick="closeConfirm()" class="flex-1 bg-slate-100 hover:bg-slate-200 text-slate-700 py-2.5 rounded-xl text-xs font-semibold transition-colors">ยกเลิก</button>
                    <button id="confirm-ok-btn" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors">ยืนยันทำรายการ</button>
                </div>
            </div>
        </div>
    </div>

    <!-- แถบแจ้งเตือน Toast Message ดำเนินการสำเร็จหรือผิดพลาด -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 bg-slate-900 text-white py-3.5 px-5 rounded-xl shadow-lg flex items-center space-x-2.5 transition-all duration-300 transform translate-y-10 opacity-0 pointer-events-none">
        <i class="fa-solid fa-circle-info text-blue-400 text-lg" id="toast-icon"></i>
        <span id="toast-message" class="text-sm font-medium">ข้อความแจ้งเตือนระบบ</span>
    </div>

    <!-- สคริปต์ควบคุมการทำงานเบื้องหลังแอปพลิเคชัน (Firebase SDK + Application Logic) -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, getDoc, setDoc, updateDoc, deleteDoc, onSnapshot, collection } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // กำหนดรายละเอียดคลาวด์แอปพลิเคชัน ID
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'school-council-attendance-blue-v7';
        const firebaseConfig = typeof __firebase_config !== 'undefined' 
            ? JSON.parse(__firebase_config) 
            : {
                apiKey: "demo-api-key",
                authDomain: "demo.firebaseapp.com",
                projectId: "demo-project-id",
                storageBucket: "demo.appspot.com",
                messagingSenderId: "123456789",
                appId: "1:123456789:web:demo"
            };

        let app, db, auth;
        let isFirebaseOnline = false;

        try {
            app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);
            isFirebaseOnline = firebaseConfig.apiKey !== "demo-api-key";
        } catch (e) {
            console.warn("การเชื่อมต่อระบบคลาวด์ทำงานขัดข้อง กำลังเปลี่ยนไปใช้ข้อมูลเครื่องทดแทน...", e);
            isFirebaseOnline = false;
        }

        // สถานะการทำงานภายในแอปพลิเคชันหลัก
        let currentUserId = "local-session";
        let members = [];
        let sessions = [];
        let currentSessionId = "";
        let filterRole = "all";
        
        // ----------------------------------------
        // โหมดการทำงาน (สิทธิ์ผู้ใช้งาน)
        // ----------------------------------------
        let isAuthorizedMode = false; // หัวหน้าฝ่าย (สิทธิ์ในการทำทุกอย่าง)
        let loggedInMemberId = null;  // สมาชิกสภา/เพื่อนที่เข้าระบบ (สิทธิ์ดูได้อย่างเดียว + แก้ไขรูปโปรไฟล์ตนเอง)
        let adminPasscode = "1234";   // รหัสผ่านยืนยันหัวหน้าฝ่ายเริ่มต้น
        const memberPasscode = "ฉันจะตั้งใจทำงาน55"; // รหัสผ่านความปลอดภัยที่สมาชิกต้องกรอกเข้าสู่ระบบ

        // ตารางเฉดสี Gradient สำหรับแต่งโปรไฟล์ด้วยอิโมจิ
        const avatarBgs = [
            "linear-gradient(135deg, #3b82f6, #1d4ed8)", // ฟ้า-น้ำเงินสว่าง
            "linear-gradient(135deg, #10b981, #047857)", // เขียวสะอาดตา
            "linear-gradient(135deg, #f59e0b, #d97706)", // ส้มทองสว่าง
            "linear-gradient(135deg, #ec4899, #be185d)", // ชมพูหวาน
            "linear-gradient(135deg, #8b5cf6, #5b21b6)", // ม่วงหรูหรา
            "linear-gradient(135deg, #ef4444, #b91c1c)", // แดงเพลิง
            "linear-gradient(135deg, #14b8a6, #0f766e)", // มิ้นต์เขียวปีกแมลงทับ
            "linear-gradient(135deg, #64748b, #334155)", // เทาแกรนิตสุดหรู
            "linear-gradient(135deg, #f43f5e, #e11d48)", // แดงโรส
            "linear-gradient(135deg, #06b6d4, #0891b2)"  // ฟ้าเทอร์ควอยซ์
        ];

        // รายการตัวเลือกอิโมจิแต่งรูปอวาตาร์
        const avatarEmojis = [
            "👩‍🎓", "👨‍🎓", "🦁", "🦊", "🐯", "🐼", "🐨", "🐸", "🐱", "🐶", "🐰", "🐙",
            "🦄", "⭐", "🔥", "🚀", "💡", "💖", "🎨", "⚽", "🎮", "📚", "🧑‍💻", "👩‍💻"
        ];

        // สถานะที่เลือกไว้ตอนแต่งรูป
        let selectedBgColor = avatarBgs[0];
        let selectedEmojiChar = avatarEmojis[0];

        // ข้อมูลโครงสร้างเริ่มต้น 18 รายชื่อที่ดึงจากสเปรดชีตของฝ่ายกิจกรรม [cite: 1]
        const defaultMembers = [
            { "order": 1, "name": "นางสาวอนัญญา คูณมา", "role": "หัวหน้าฝ่าย", "avatar": "" },
            { "order": 2, "name": "นางสาวติณณา ไชยนา", "role": "รองหัวหน้าฝ่าย", "avatar": "" },
            { "order": 3, "name": "นางสาวพัชรียา วริพันธ์", "role": "รองหัวหน้าฝ่าย", "avatar": "" },
            { "order": 4, "name": "นายจุลจักร โคกโพธิ์", "role": "เลขาฝ่าย", "avatar": "" },
            { "order": 5, "name": "นายกลวัชร สุวะรักษ์", "role": "สมาชิก", "avatar": "" },
            { "order": 6, "name": "นายเจตนิรันดร์ เกษกุล", "role": "สมาชิก", "avatar": "" },
            { "order": 7, "name": "นางสาวกัญญพัชร ดวงแข", "role": "สมาชิก", "avatar": "" },
            { "order": 8, "name": "นางสาวธีรดา ณุวงษ์ศรี", "role": "สมาชิก", "avatar": "" },
            { "order": 9, "name": "นางสาวสุภาวดี ช่างคำ", "role": "สมาชิก", "avatar": "" },
            { "order": 10, "name": "นายสิรวิชญ์ ทับทิมจันทร์", "role": "สมาชิก", "avatar": "" },
            { "order": 11, "name": "นางสาวปาลิตา คุณาสิทธิ์", "role": "สมาชิก", "avatar": "" },
            { "order": 12, "name": "นางสาวเขมจิรา แสงบุญเรือง", "role": "สมาชิก", "avatar": "" },
            { "order": 13, "name": "นายวโรทัย เจริญชัยสงค์", "role": "สมาชิก", "avatar": "" },
            { "order": 14, "name": "นายอัศวิน สมบูรณ์", "role": "สมาชิก", "avatar": "" },
            { "order": 15, "name": "นายณฐประภาส สว่างเนตร", "role": "สมาชิก", "avatar": "" },
            { "order": 16, "name": "นางสาวพัชรพร โสมอินทร์", "role": "สมาชิก", "avatar": "" },
            { "order": 17, "name": "นางสาวอริศรา สันตะวงศ์", "role": "สมาชิก", "avatar": "" },
            { "order": 18, "name": "นางสาวสุภัสสรา กีรตินันท์วัฒนา", "role": "สมาชิก", "avatar": "" }
        ];

        // อ้างอิงจุดเก็บคอลเลกชันในคลาวด์ Firestore
        const getMembersCollection = () => collection(db, 'artifacts', appId, 'public', 'data', 'members');
        const getSessionsCollection = () => collection(db, 'artifacts', appId, 'public', 'data', 'sessions');
        const getSettingsDoc = () => doc(db, 'artifacts', appId, 'public', 'data', 'settings', 'admin');

        // การยืนยันสิทธิ์เข้ารับการจัดการ
        async function authenticateUser() {
            if (!isFirebaseOnline) {
                document.getElementById('current-user-id').textContent = "Local (ออฟไลน์)";
                document.getElementById('sync-dot').className = "relative inline-flex rounded-full h-2 w-2 bg-amber-500";
                return;
            }
            try {
                const token = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
                let userCredential;
                if (token) {
                    userCredential = await signInWithCustomToken(auth, token);
                } else {
                    userCredential = await signInAnonymously(auth);
                }
                currentUserId = userCredential.user.uid;
                document.getElementById('current-user-id').textContent = appId;
                document.getElementById('sync-dot').className = "relative inline-flex rounded-full h-2 w-2 bg-green-500";
            } catch (error) {
                console.error("เข้าสู่ระบบคลาวด์ผิดพลาด กำลังประมวลผลบนเครื่องดั้งเดิม:", error);
                isFirebaseOnline = false;
                document.getElementById('current-user-id').textContent = "เครื่องผู้ใช้ (สำรอง)";
                document.getElementById('sync-dot').className = "relative inline-flex rounded-full h-2 w-2 bg-amber-500";
            }
        }

        // ประมวลผลและสร้างสารบัญข้อมูลเริ่มต้นเมื่อเริ่มเปิดโปรแกรม (Cloud & Local Storage)
        async function initializeDatabaseIfNeeded() {
            if (!isFirebaseOnline) {
                if (!localStorage.getItem('blue_local_members')) {
                    localStorage.setItem('blue_local_members', JSON.stringify(defaultMembers));
                }
                if (!localStorage.getItem('blue_local_sessions') || JSON.parse(localStorage.getItem('blue_local_sessions')).length === 0) {
                    const defaultSession = [{
                        id: 'session_default',
                        name: "การประชุมหรือปฏิบัติงานครั้งแรกของสัปดาห์",
                        date: new Date().toISOString().split('T')[0],
                        created_at: Date.now(),
                        attendance: {}
                    }];
                    localStorage.setItem('blue_local_sessions', JSON.stringify(defaultSession));
                }
                if (!localStorage.getItem('blue_admin_passcode')) {
                    localStorage.setItem('blue_admin_passcode', "1234");
                }
                adminPasscode = localStorage.getItem('blue_admin_passcode') || "1234";
                loadLocalData();
                return;
            }

            try {
                // ดึงรหัสผ่านปัจจุบันจากฐานข้อมูลระบบ
                const passSnap = await getDoc(getSettingsDoc());
                if (passSnap.exists()) {
                    adminPasscode = passSnap.data().passcode || "1234";
                } else {
                    await setDoc(getSettingsDoc(), { passcode: "1234" });
                    adminPasscode = "1234";
                }

                const membersRef = getMembersCollection();
                const initCheckDoc = doc(membersRef, 'init_check');
                const docSnap = await getDoc(initCheckDoc);

                if (!docSnap.exists()) {
                    showToast("กำลังประมวลผลสร้างรายชื่อสมาชิกเริ่มต้นสภา 18 คน...", "info");
                    
                    // ป้อนรายชื่อเริ่มต้นทั้ง 18 คนลงฐานข้อมูลคลาวด์ [cite: 1]
                    for (const m of defaultMembers) {
                        const mId = 'member_' + m.order;
                        const cleanName = m.name.replace(/^\./, "");
                        await setDoc(doc(membersRef, mId), {
                            id: mId,
                            name: cleanName,
                            role: m.role,
                            order: m.order,
                            avatar: m.avatar || ""
                        });
                    }
                    
                    await setDoc(initCheckDoc, { initialized: true });
                    
                    // จัดทำรอบลงชื่อดั้งเดิมให้
                    const sessionsRef = getSessionsCollection();
                    const defaultSessionId = 'session_default';
                    await setDoc(doc(sessionsRef, defaultSessionId), {
                        id: defaultSessionId,
                        name: "การประชุมหรือปฏิบัติงานครั้งแรกของสัปดาห์",
                        date: new Date().toISOString().split('T')[0],
                        created_at: Date.now(),
                        attendance: {}
                    });
                }
            } catch (err) {
                console.error("ระบบเก็บข้อมูลคลาวด์ขัดข้อง ย้ายไปออฟไลน์:", err);
                isFirebaseOnline = false;
                initializeDatabaseIfNeeded();
            }
        }

        function loadLocalData() {
            members = JSON.parse(localStorage.getItem('blue_local_members')) || [];
            sessions = JSON.parse(localStorage.getItem('blue_local_sessions')) || [];
            applyFilters();
            updateStats();
            updateSessionDropdown();
            renderManageMembers();
            updateMemberLoginDropdown();
            updatePersonalStats();
        }

        // สังเกตการณ์การอัปเดตแบบเรียลไทม์ (เมื่อมีเครื่องอื่นแก้ไขข้อมูลจะปรับปรุงทันที)
        function setupDataListeners() {
            if (!isFirebaseOnline) return;

            // สังเกตการณ์รหัสผ่านเพื่อสอดคล้องความปลอดภัย
            onSnapshot(getSettingsDoc(), (snapshot) => {
                if (snapshot.exists()) {
                    adminPasscode = snapshot.data().passcode || "1234";
                }
            }, (error) => {
                console.error("การเชื่อมรหัสผ่านล้มเหลว:", error);
            });

            // สังเกตการณ์และซิงค์ข้อมูลรายชื่อสมาชิกสภา
            onSnapshot(getMembersCollection(), (snapshot) => {
                members = [];
                snapshot.forEach((doc) => {
                    const data = doc.data();
                    if (data.id) {
                        members.push(data);
                    }
                });
                members.sort((a, b) => a.order - b.order);
                applyFilters();
                updateStats();
                renderManageMembers();
                updateMemberLoginDropdown();
                updatePersonalStats();
            }, (error) => {
                console.error("การเชื่อมคลาวด์รายชื่อขัดข้อง:", error);
            });

            // สังเกตการณ์และซิงค์รอบเช็คชื่อ/การประชุม
            onSnapshot(getSessionsCollection(), (snapshot) => {
                sessions = [];
                snapshot.forEach((doc) => {
                    const data = doc.data();
                    if (data.id) {
                        sessions.push(data);
                    }
                });
                sessions.sort((a, b) => b.created_at - a.created_at);
                
                // ถ้ารอบประชุมหายไปหมด ให้รีบสร้างขึ้นมาใหม่ทันที
                if (sessions.length === 0) {
                    const sId = 'session_' + Date.now();
                    const defaultSess = {
                        id: sId,
                        name: "การปฏิบัติงานและเช็คชื่อประจำวัน",
                        date: new Date().toISOString().split('T')[0],
                        created_at: Date.now(),
                        attendance: {}
                    };
                    setDoc(doc(getSessionsCollection(), sId), defaultSess);
                }
                
                updateSessionDropdown();
                updatePersonalStats();
            }, (error) => {
                console.error("การเชื่อมคลาวด์รอบประชุมขัดข้อง:", error);
            });
        }

        // อัปเดตรอบกิจกรรมที่เลือกใน Dropdown
        function updateSessionDropdown() {
            const selectEl = document.getElementById('session-select');
            const activeSessions = sessions.filter(s => s.id && s.name);
            
            if (activeSessions.length === 0) {
                selectEl.innerHTML = `<option value="">-- ไม่มีรอบกิจกรรม --</option>`;
                currentSessionId = "";
                applyFilters();
                updateStats();
                return;
            }

            if (!currentSessionId || !activeSessions.some(s => s.id === currentSessionId)) {
                currentSessionId = activeSessions[0].id;
            }

            let htmlStr = "";
            activeSessions.forEach(s => {
                const isSelected = s.id === currentSessionId ? "selected" : "";
                htmlStr += `<option value="${s.id}" ${isSelected}>${s.name} (${formatDate(s.date)})</option>`;
            });
            
            selectEl.innerHTML = htmlStr;
            applyFilters();
            updateStats();
        }

        // ปรับแต่ง Dropdown สำหรับเพื่อนเพื่อเข้าระบบ
        function updateMemberLoginDropdown() {
            const selectEl = document.getElementById('member-login-select');
            let htmlStr = `<option value="">-- กรุณาเลือกชื่อของคุณ --</option>`;
            members.forEach(m => {
                htmlStr += `<option value="${m.id}">${m.name} (${m.role})</option>`;
            });
            selectEl.innerHTML = htmlStr;
        }

        // ประมวลผลรูปโปรไฟล์อัติโนมัติในกรณีไม่มีรูปภาพ
        function getAvatarUrl(member) {
            if (member.avatar && member.avatar.trim() !== "") {
                return member.avatar;
            }
            // ใช้ API สร้างภาพโปรไฟล์จากอักษรย่อไทย
            const cleanName = member.name.replace("นางสาว", "").replace("นาย", "").replace(/^\./, "").trim();
            const initials = cleanName.substring(0, 2);
            return `https://ui-avatars.com/api/?name=${encodeURIComponent(initials)}&background=3b82f6&color=ffffff&size=128&font-size=0.45&bold=true`;
        }

        // ฟังก์ชันคำนวณและเรนเดอร์ตารางรายชื่อผู้ใช้
        window.applyFilters = function() {
            const searchVal = document.getElementById('search-input').value.toLowerCase().trim();
            const currentSession = sessions.find(s => s.id === currentSessionId);
            const attendanceMap = currentSession ? (currentSession.attendance || {}) : {};

            let filtered = members.filter(m => {
                const matchesRole = filterRole === 'all' || m.role === filterRole;
                const matchesSearch = m.name.toLowerCase().includes(searchVal) || m.role.toLowerCase().includes(searchVal);
                return matchesRole && matchesSearch;
            });

            const desktopBody = document.getElementById('desktop-tbody');
            const mobileCardContainer = document.getElementById('mobile-card-container');
            const emptyState = document.getElementById('empty-state');

            if (filtered.length === 0) {
                desktopBody.innerHTML = "";
                mobileCardContainer.innerHTML = "";
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');

            // 1. เรนเดอร์หน้าจอสำหรับคอมพิวเตอร์ (Desktop Table View)
            let desktopHtml = "";
            filtered.forEach(m => {
                const status = attendanceMap[m.id] || 'pending';
                const isMe = loggedInMemberId === m.id;
                
                desktopHtml += `
                    <tr class="hover:bg-blue-50/30 transition-all ${isMe ? 'bg-blue-50/50 border-l-4 border-blue-600' : ''}">
                        <td class="py-4 px-6 text-center font-bold ${isMe ? 'text-blue-600' : 'text-slate-400'} text-sm">
                            ${isMe ? '<i class="fa-solid fa-circle-user animate-bounce mr-1"></i>' : m.order}
                        </td>
                        <td class="py-4 px-6 w-24">
                            <div class="relative inline-block">
                                <img src="${getAvatarUrl(m)}" alt="${m.name}" onerror="this.src='https://placehold.co/100x100/3b82f6/ffffff?text=User'" class="w-12 h-12 rounded-full object-cover border-2 border-white shadow-sm">
                                ${isMe || isAuthorizedMode ? `
                                    <button onclick="openEditAvatarModal('${m.id}')" class="absolute -bottom-1 -right-1 bg-blue-600 text-white w-5.5 h-5.5 rounded-full flex items-center justify-center text-[10px] shadow hover:bg-blue-700 transition-colors" title="ปรับแต่งรูปภาพ">
                                        <i class="fa-solid fa-camera"></i>
                                    </button>
                                ` : ''}
                            </div>
                        </td>
                        <td class="py-4 px-6">
                            <div class="font-semibold ${isMe ? 'text-blue-700 font-bold' : 'text-slate-800'} text-sm flex items-center">
                                ${m.name}
                                ${isMe ? ' <span class="ml-2 bg-blue-600 text-white text-[9px] px-2 py-0.5 rounded-full font-bold">นี่คือคุณ</span>' : ''}
                            </div>
                        </td>
                        <td class="py-4 px-6">
                            <span class="px-2.5 py-1 rounded-full text-xs font-semibold ${getRoleBadgeClass(m.role)}">
                                ${m.role}
                            </span>
                        </td>
                        <td class="py-4 px-6">
                            <div class="flex items-center justify-center space-x-1.5">
                                ${isAuthorizedMode ? `
                                    <!-- ปุ่มสามารถกดเช็คได้เฉพาะหัวหน้าสภาเท่านั้น (เน้นกดทีละคน) -->
                                    <button onclick="setStatus('${m.id}', 'working')" class="status-btn px-3.5 py-1.5 rounded-xl text-xs font-bold transition-all border ${status === 'working' ? 'bg-blue-600 border-blue-600 text-white shadow-md' : 'border-slate-200 text-slate-600 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-check mr-1 text-emerald-300"></i>ทำงาน
                                    </button>
                                    <button onclick="setStatus('${m.id}', 'not_working')" class="status-btn px-3.5 py-1.5 rounded-xl text-xs font-bold transition-all border ${status === 'not_working' ? 'bg-rose-600 border-rose-600 text-white shadow-md' : 'border-slate-200 text-slate-600 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-xmark mr-1 text-rose-300"></i>ไม่ทำงาน
                                    </button>
                                    <button onclick="setStatus('${m.id}', 'leave')" class="status-btn px-3.5 py-1.5 rounded-xl text-xs font-bold transition-all border ${status === 'leave' ? 'bg-amber-500 border-amber-500 text-white shadow-md' : 'border-slate-200 text-slate-600 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-pause mr-1 text-amber-200"></i>ลา
                                    </button>
                                ` : `
                                    <!-- เพื่อนหรือบุคคลทั่วไป ดูสถานะได้อย่างเดียวจากการอัปเดตของหัวหน้า -->
                                    <span class="text-xs px-4 py-1.5 rounded-xl font-bold flex items-center space-x-1 border ${getStaticStatusClass(status)}">
                                        ${getStaticStatusText(status)}
                                    </span>
                                `}
                            </div>
                        </td>
                    </tr>
                `;
            });
            desktopBody.innerHTML = desktopHtml;

            // 2. เรนเดอร์หน้าจอสำหรับโทรศัพท์เคลื่อนที่ (Mobile Card View)
            let mobileHtml = "";
            filtered.forEach(m => {
                const status = attendanceMap[m.id] || 'pending';
                const isMe = loggedInMemberId === m.id;
                
                mobileHtml += `
                    <div class="bg-white border ${isMe ? 'border-blue-300 ring-2 ring-blue-500/10' : 'border-slate-100'} rounded-2xl p-4 shadow-sm space-y-3 transition-all">
                        <div class="flex items-center space-x-3">
                            <div class="relative">
                                <img src="${getAvatarUrl(m)}" alt="${m.name}" onerror="this.src='https://placehold.co/100x100/3b82f6/ffffff?text=User'" class="w-12 h-12 rounded-full object-cover border-2 border-slate-100 shadow-sm">
                                ${isMe || isAuthorizedMode ? `
                                    <button onclick="openEditAvatarModal('${m.id}')" class="absolute -bottom-1 -right-1 bg-blue-600 text-white w-5.5 h-5.5 rounded-full flex items-center justify-center text-[10px] shadow" title="ปรับแต่งรูป">
                                        <i class="fa-solid fa-camera"></i>
                                    </button>
                                ` : ''}
                            </div>
                            <div class="flex-grow">
                                <div class="flex items-center">
                                    <h4 class="font-bold ${isMe ? 'text-blue-700 font-bold' : 'text-slate-800'} text-sm">${m.name}</h4>
                                    ${isMe ? ' <span class="ml-1.5 bg-blue-600 text-white text-[8px] px-1.5 py-0.5 rounded-full font-bold">นี่คือคุณ</span>' : ''}
                                </div>
                                <span class="text-[10px] font-bold text-slate-400">ลำดับตำแหน่ง: ${m.order}</span>
                            </div>
                            <span class="px-2 py-0.5 rounded-full text-[10px] font-semibold ${getRoleBadgeClass(m.role)}">
                                ${m.role}
                            </span>
                        </div>
                        <div class="pt-1 border-t border-slate-50">
                            ${isAuthorizedMode ? `
                                <div class="grid grid-cols-3 gap-1.5">
                                    <button onclick="setStatus('${m.id}', 'working')" class="status-btn py-2.5 rounded-xl text-xs font-bold transition-all border flex flex-col items-center justify-center space-y-1 ${status === 'working' ? 'bg-blue-600 border-blue-600 text-white shadow-md' : 'border-slate-200 text-slate-500 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-check"></i>
                                        <span class="text-[10px]">ทำงาน</span>
                                    </button>
                                    <button onclick="setStatus('${m.id}', 'not_working')" class="status-btn py-2.5 rounded-xl text-xs font-bold transition-all border flex flex-col items-center justify-center space-y-1 ${status === 'not_working' ? 'bg-rose-600 border-rose-600 text-white shadow-md' : 'border-slate-200 text-slate-500 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-xmark"></i>
                                        <span class="text-[10px]">ไม่ทำงาน</span>
                                    </button>
                                    <button onclick="setStatus('${m.id}', 'leave')" class="status-btn py-2.5 rounded-xl text-xs font-bold transition-all border flex flex-col items-center justify-center space-y-1 ${status === 'leave' ? 'bg-amber-500 border-amber-500 text-white shadow-md' : 'border-slate-200 text-slate-500 hover:bg-slate-50'}">
                                        <i class="fa-solid fa-circle-pause"></i>
                                        <span class="text-[10px]">ลา</span>
                                    </button>
                                </div>
                            ` : `
                                <div class="w-full text-center py-2 border border-dashed rounded-xl bg-slate-50/50 flex items-center justify-center space-x-1.5">
                                    <span class="text-xs px-4 py-1 rounded-full font-bold border ${getStaticStatusClass(status)}">
                                        ${getStaticStatusText(status)}
                                    </span>
                                </div>
                            `}
                        </div>
                    </div>
                `;
            });
            mobileCardContainer.innerHTML = mobileHtml;
        }

        // คลาสกำหนดสไตล์สถานะการดูข้อมูลสภานักเรียน
        function getStaticStatusClass(status) {
            switch(status) {
                case 'working':
                    return 'bg-emerald-50 border-emerald-200 text-emerald-700';
                case 'not_working':
                    return 'bg-rose-50 border-rose-200 text-rose-700';
                case 'leave':
                    return 'bg-amber-50 border-amber-200 text-amber-700';
                default:
                    return 'bg-slate-50 border-slate-200 text-slate-400';
            }
        }

        function getStaticStatusText(status) {
            switch(status) {
                case 'working':
                    return '<i class="fa-solid fa-circle-check mr-1 text-emerald-500"></i> ทำงานอยู่';
                case 'not_working':
                    return '<i class="fa-solid fa-circle-xmark mr-1 text-rose-500"></i> ไม่ทำงาน';
                case 'leave':
                    return '<i class="fa-solid fa-circle-pause mr-1 text-amber-500"></i> ขอลาพัก';
                default:
                    return '<i class="fa-solid fa-spinner fa-spin mr-1 text-slate-400"></i> ยังไม่ได้บันทึก';
            }
        }

        // คำนวณสรุปยอดตัวเลข Dashboard สถิติรายวัน
        function updateStats() {
            const currentSession = sessions.find(s => s.id === currentSessionId);
            const attendanceMap = currentSession ? (currentSession.attendance || {}) : {};
            
            let working = 0, leave = 0, not_working = 0;

            members.forEach(m => {
                const status = attendanceMap[m.id];
                if (status === 'working') working++;
                else if (status === 'leave') leave++;
                else if (status === 'not_working') not_working++;
            });

            const total = members.length;
            const percentage = total > 0 ? Math.round((working / total) * 100) : 0;

            document.getElementById('stat-total').textContent = `${total} คน`;
            document.getElementById('stat-working').textContent = `${working} คน`;
            document.getElementById('stat-leave').textContent = `${leave} คน`;
            document.getElementById('stat-not-working').textContent = `${not_working} คน`;
            
            const pctBadge = document.getElementById('stat-percent');
            pctBadge.textContent = `${percentage}% ปฏิบัติงาน`;
            
            if (percentage >= 80) {
                pctBadge.className = "text-xs bg-emerald-50 text-emerald-700 px-2 rounded-full font-bold";
            } else if (percentage >= 50) {
                pctBadge.className = "text-xs bg-amber-50 text-amber-700 px-2 rounded-full font-bold";
            } else {
                pctBadge.className = "text-xs bg-rose-50 text-rose-700 px-2 rounded-full font-bold";
            }
        }

        // แสดงข้อมูลสถิติเฉพาะของฉันและจำนวนครั้งที่ขาดทำงาน
        function updatePersonalStats() {
            const container = document.getElementById('personal-stats-section');
            if (!loggedInMemberId) {
                container.classList.add('hidden');
                return;
            }

            container.classList.remove('hidden');

            let workingCount = 0;
            let leaveCount = 0;
            let absentCount = 0; // "ไม่ทำงาน" จะนับเป็นการขาดทำงาน

            sessions.forEach(s => {
                const attendance = s.attendance || {};
                const status = attendance[loggedInMemberId];
                if (status === 'working') {
                    workingCount++;
                } else if (status === 'leave') {
                    leaveCount++;
                } else if (status === 'not_working') {
                    absentCount++;
                }
            });

            document.getElementById('personal-stat-working').textContent = `${workingCount} ครั้ง`;
            document.getElementById('personal-stat-leave').textContent = `${leaveCount} ครั้ง`;
            document.getElementById('personal-stat-absent').textContent = `${absentCount} ครั้ง`;

            // ตั้งค่าปุ่มแก้ไขรูปโปรไฟล์ให้ผูกกับบัญชีตนเองโดยตรง
            const editBtn = document.getElementById('personal-edit-avatar-btn');
            editBtn.onclick = function() {
                openEditAvatarModal(loggedInMemberId);
            };
        }

        // ฟังก์ชันสั่งเช็คสถานะการทำงานแบบระบุรายคน (กดแล้วอัปเดตแบบเรียลไทม์)
        window.setStatus = async function(memberId, status) {
            if (!isAuthorizedMode) {
                showToast("เฉพาะหัวหน้าฝ่ายสภานักเรียนเท่านั้นที่มีสิทธิ์เช็คชื่อเพื่อนๆ ได้ครับ", "error");
                return;
            }

            if (!currentSessionId) {
                showToast("ไม่มีรอบกิจกรรมที่เลือกอยู่ โปรดระบุรอบการปฏิบัติงานก่อนเช็คชื่อ", "info");
                return;
            }

            const currentSession = sessions.find(s => s.id === currentSessionId);
            if (!currentSession) return;

            const attendance = { ...(currentSession.attendance || {}) };
            
            // หากเป็นการกดซ้ำปุ่มเดิมให้รีเซ็ตกลับเป็นค่าเริ่มต้น (ยกเลิกสถานะ)
            if (attendance[memberId] === status) {
                delete attendance[memberId];
            } else {
                attendance[memberId] = status;
            }

            if (!isFirebaseOnline) {
                currentSession.attendance = attendance;
                localStorage.setItem('blue_local_sessions', JSON.stringify(sessions));
                loadLocalData();
                return;
            }

            try {
                const sessionRef = doc(getSessionsCollection(), currentSessionId);
                await setDoc(sessionRef, { attendance }, { merge: true });
            } catch (err) {
                console.error("อัปเดตข้อมูลล้มเหลว:", err);
                showToast("ระบบคลาวด์ขัดข้อง ดำเนินการออฟไลน์ชั่วคราว", "info");
            }
        }

        // สั่งเคลียร์และรีเซ็ตสถานะทุกคนในรอบการเช็คชื่อปัจจุบัน
        window.triggerResetAllStatuses = function() {
            if (!isAuthorizedMode) return;
            if (!currentSessionId) return;
            
            showConfirm(
                "รีเซ็ตการเช็คชื่อทั้งหมด",
                "คุณต้องการล้างข้อมูลการเช็คชื่อของทุกคนในรอบกิจกรรมปัจจุบันใช่หรือไม่?",
                async () => {
                    if (!isFirebaseOnline) {
                        const currentSession = sessions.find(s => s.id === currentSessionId);
                        if (currentSession) {
                            currentSession.attendance = {};
                            localStorage.setItem('blue_local_sessions', JSON.stringify(sessions));
                            loadLocalData();
                        }
                        showToast("รีเซ็ตสถานะทั้งหมดเรียบร้อย", "success");
                        closeConfirm();
                        return;
                    }

                    try {
                        const sessionRef = doc(getSessionsCollection(), currentSessionId);
                        await setDoc(sessionRef, { attendance: {} }, { merge: true });
                        showToast("เคลียร์ข้อมูลการเช็คชื่อสำเร็จ", "success");
                        closeConfirm();
                    } catch (err) {
                        console.error("ล้างประวัติขัดข้อง:", err);
                    }
                }
            );
        }

        // สั่งพิมพ์เอกสารรายงานเช็คชื่อออกในรูปแบบ Excel UTF-8 CSV
        window.exportToCSV = function() {
            const currentSession = sessions.find(s => s.id === currentSessionId);
            if (!currentSession) {
                showToast("ไม่มีข้อมูลการเช็คชื่อสภาให้ดาวน์โหลด", "error");
                return;
            }

            const attendanceMap = currentSession.attendance || {};
            let csvContent = "\uFEFF"; // ป้องกันการแสดงผลอักษรไทยเพี้ยนใน MS Excel
            csvContent += `รายงานการบันทึกสถานะปฏิบัติงานฝ่ายกิจกรรม\n`;
            csvContent += `รอบกิจกรรม/หัวข้อ,${currentSession.name}\n`;
            csvContent += `วันที่ปฏิบัติงาน,${formatDate(currentSession.date)}\n`;
            csvContent += `ผู้พัฒนาระบบคลาวด์,นายธนัชนนท์ ทาทอง (รองประธาน)\n\n`;
            csvContent += "ลำดับที่,ชื่อ-นามสกุล,ตำแหน่งงาน,สถานะทำงานวันนี้\n";

            members.forEach(m => {
                const rawStatus = attendanceMap[m.id] || "pending";
                let statusThai = "ยังไม่ได้ประเมิน";
                if (rawStatus === 'working') statusThai = "ทำงาน";
                else if (rawStatus === 'not_working') statusThai = "ไม่ทำงาน";
                else if (rawStatus === 'leave') statusThai = "ลา";

                csvContent += `"${m.order}","${m.name}","${m.role}","${statusThai}"\n`;
            });

            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const url = URL.createObjectURL(blob);
            const link = document.createElement("a");
            link.setAttribute("href", url);
            link.setAttribute("download", `รายงานสภาฝ่ายกิจกรรม_${currentSession.name.replace(/ /g, '_')}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            showToast("ส่งออกไฟล์รายงานเรียบร้อย!", "success");
        }

        // Helper สำหรับเปิดปิดการมองเห็นรหัสผ่าน
        window.togglePasswordVisibility = function(inputId, eyeId) {
            const input = document.getElementById(inputId);
            const eye = document.getElementById(eyeId);
            if (input.type === 'password') {
                input.type = 'text';
                eye.classList.remove('fa-eye-slash');
                eye.classList.add('fa-eye');
            } else {
                input.type = 'password';
                eye.classList.remove('fa-eye');
                eye.classList.add('fa-eye-slash');
            }
        }
        
        // ยืนยันรหัสเข้าสู่โหมดหัวหน้าฝ่ายกิจกรรม
        window.openAuthModal = function() {
            if (isAuthorizedMode || loggedInMemberId) {
                // หากเข้าระบบอะไรอยู่ให้กดเพื่อล้างล็อกเอาท์ทันที
                isAuthorizedMode = false;
                loggedInMemberId = null;
                localStorage.removeItem('blue_logged_in_member');
                updateAuthStatusUI();
                showToast("ออกจากระบบแล้ว ยินดีต้อนรับสู่โหมดเพื่อนๆ สำหรับเยี่ยมชม", "info");
                return;
            }
            document.getElementById('auth-modal').classList.remove('opacity-0', 'pointer-events-none');
            // รีเซ็ตฟิลด์รหัสผ่าน
            document.getElementById('auth-passcode').value = "";
            document.getElementById('auth-passcode').type = "password";
            document.getElementById('auth-passcode-eye').className = "fa-solid fa-eye-slash";
            document.getElementById('auth-passcode').focus();
        }

        window.closeAuthModal = function() {
            document.getElementById('auth-modal').classList.add('opacity-0', 'pointer-events-none');
        }

        window.submitAuth = function() {
            const code = document.getElementById('auth-passcode').value.trim();
            if (code === adminPasscode) {
                isAuthorizedMode = true;
                loggedInMemberId = null; // ล้างล็อกอินสมาชิกธรรมดาออก
                localStorage.removeItem('blue_logged_in_member');
                
                updateAuthStatusUI();
                closeAuthModal();
                showToast("ยินดีต้อนรับหัวหน้าฝ่ายกิจกรรม! ปลดล็อกการแก้ไขเช็คชื่อแล้ว", "success");
            } else {
                showToast("รหัสความปลอดภัยผู้ดูแลสภานักเรียนไม่ถูกต้อง", "error");
            }
        }

        // ยืนยันเข้าระบบสมาชิก (สำหรับให้เพื่อนเปลี่ยนรูปภาพของตัวเองได้)
        window.openMemberLoginModal = function() {
            document.getElementById('member-login-modal').classList.remove('opacity-0', 'pointer-events-none');
            document.getElementById('member-password-input').value = "";
            document.getElementById('member-password-input').type = "password";
            document.getElementById('member-password-eye').className = "fa-solid fa-eye-slash";
        }

        window.closeMemberLoginModal = function() {
            document.getElementById('member-login-modal').classList.add('opacity-0', 'pointer-events-none');
        }

        // เข้าระบบสมาชิก โดยต้องระบุรหัสความปลอดภัย "ฉันจะตั้งใจทำงาน55"
        window.submitMemberLogin = function() {
            const selectEl = document.getElementById('member-login-select');
            const selectedMemberId = selectEl.value;
            const enteredPassword = document.getElementById('member-password-input').value.trim();
            
            if (!selectedMemberId) {
                showToast("โปรดเลือกรายชื่อบัญชีของคุณก่อนเข้าระบบ", "error");
                return;
            }

            if (enteredPassword !== memberPasscode) {
                showToast("รหัสผ่านสมาชิกสภาไม่ถูกต้อง (รหัสผ่าน: ฉันจะตั้งใจทำงาน...)", "error");
                return;
            }

            loggedInMemberId = selectedMemberId;
            isAuthorizedMode = false; // เคลียร์สิทธิ์หัวหน้าออก
            localStorage.setItem('blue_logged_in_member', selectedMemberId);

            const m = members.find(item => item.id === selectedMemberId);
            
            updateAuthStatusUI();
            closeMemberLoginModal();
            showToast(`ยินดีต้อนรับคุณ "${m ? m.name : ''}" เข้าสู่ระบบ!`, "success");
        }

        // ปรับแต่งหน้าจอ UI ตามสิทธิ์ของผู้เข้าใช้ปัจจุบัน
        function updateAuthStatusUI() {
            const roleBadge = document.getElementById('role-badge');
            const roleIcon = document.getElementById('role-icon');
            const roleText = document.getElementById('role-text');
            const authActionBtn = document.getElementById('auth-action-btn');
            const memberLoginBtn = document.getElementById('member-login-btn');

            const adminSess = document.getElementById('admin-session-btns');
            const adminBatch = document.getElementById('admin-batch-btns');
            const adminMembers = document.getElementById('admin-manage-members-btn');

            if (isAuthorizedMode) {
                roleBadge.className = "bg-emerald-600 px-3 py-1.5 rounded-xl text-xs font-bold text-white flex items-center space-x-1.5 shadow-md";
                roleIcon.className = "fa-solid fa-user-shield text-emerald-100 animate-bounce";
                roleText.textContent = "โหมดควบคุม (หัวหน้าฝ่าย)";
                
                authActionBtn.innerHTML = `<i class="fa-solid fa-sign-out-alt"></i> <span>ออกจากระบบ</span>`;
                authActionBtn.className = "bg-rose-600 hover:bg-rose-700 text-white px-3.5 py-1.5 rounded-xl text-xs font-bold shadow-md transition-all flex items-center space-x-1.5";
                
                memberLoginBtn.classList.add('hidden'); // ซ่อนล็อกอินเพื่อน
                
                adminSess.classList.remove('hidden');
                adminBatch.classList.remove('hidden');
                adminMembers.classList.remove('hidden');
            } else if (loggedInMemberId) {
                const myMember = members.find(m => m.id === loggedInMemberId);
                const nameShort = myMember ? myMember.name.split(' ')[0] : 'สมาชิก';

                roleBadge.className = "bg-blue-600 px-3 py-1.5 rounded-xl text-xs font-bold text-white flex items-center space-x-1.5 shadow-md pulse-glowing";
                roleIcon.className = "fa-solid fa-user-check text-blue-100";
                roleText.textContent = `สมาชิก: ${nameShort}`;

                authActionBtn.innerHTML = `<i class="fa-solid fa-sign-out-alt"></i> <span>ออกจากระบบ</span>`;
                authActionBtn.className = "bg-rose-600 hover:bg-rose-700 text-white px-3.5 py-1.5 rounded-xl text-xs font-bold shadow-md transition-all flex items-center space-x-1.5";
                
                memberLoginBtn.classList.add('hidden'); // ซ่อนล็อกอินเพื่อนเพื่อแทนที่
                
                adminSess.classList.add('hidden');
                adminBatch.classList.add('hidden');
                adminMembers.classList.add('hidden');
            } else {
                roleBadge.className = "bg-blue-50 border border-blue-100 px-3 py-1.5 rounded-xl text-xs font-semibold text-blue-700 flex items-center space-x-1.5";
                roleIcon.className = "fa-solid fa-eye text-blue-500 animate-pulse";
                roleText.textContent = "โหมดดูอย่างเดียว (เพื่อน)";
                
                authActionBtn.innerHTML = `<i class="fa-solid fa-lock"></i> <span>หัวหน้าฝ่าย</span>`;
                authActionBtn.className = "bg-blue-600 text-white hover:bg-blue-700 px-3 py-1.5 rounded-xl text-xs font-bold shadow-sm transition-all flex items-center space-x-1.5";
                
                memberLoginBtn.classList.remove('hidden'); // แสดงปุ่มล็อกอินเพื่อนปกติ

                adminSess.classList.add('hidden');
                adminBatch.classList.add('hidden');
                adminMembers.classList.add('hidden');
            }
            applyFilters();
            updatePersonalStats();
        }

        // ----------------------------------------
        // โมดอลและระบบปรับปรุงรูปภาพโปรไฟล์ (Avatar Settings)
        // ----------------------------------------
        let targetAvatarMemberId = "";

        // สร้างหน้าจอปรับแต่งโปรไฟล์ด้วยอิโมจิ
        window.openEditAvatarModal = function(memberId) {
            targetAvatarMemberId = memberId;
            const target = members.find(m => m.id === memberId);
            if (!target) return;

            document.getElementById('avatar-modal-title').innerHTML = `<i class="fa-solid fa-wand-magic-sparkles mr-2"></i>ปรับแต่งโปรไฟล์: ${target.name}`;
            
            const currentAvatar = getAvatarUrl(target);
            document.getElementById('avatar-live-preview').src = currentAvatar;
            document.getElementById('edit-avatar-url').value = target.avatar || "";

            // เรนเดอร์ ตารางแต่งสี
            const bgGrid = document.getElementById('avatar-bg-grid');
            let bgHtml = "";
            avatarBgs.forEach((bg, index) => {
                const isSelected = selectedBgColor === bg;
                bgHtml += `
                    <button onclick="setAvatarBgColor('${index}')" class="w-8 h-8 rounded-full border-2 ${isSelected ? 'border-blue-600 ring-2 ring-blue-400' : 'border-white'} shadow-sm transition-all" style="background: ${bg};"></button>
                `;
            });
            bgGrid.innerHTML = bgHtml;

            // เรนเดอร์ ตารางแต่งอิโมจิ
            const emojiGrid = document.getElementById('avatar-emoji-grid');
            let emojiHtml = "";
            avatarEmojis.forEach((emoji) => {
                const isSelected = selectedEmojiChar === emoji;
                emojiHtml += `
                    <button onclick="setAvatarEmoji('${emoji}')" class="p-2 hover:bg-blue-50 rounded-xl transition-all ${isSelected ? 'bg-blue-100 ring-1 ring-blue-500 scale-110' : ''}">${emoji}</button>
                `;
            });
            emojiGrid.innerHTML = emojiHtml;

            document.getElementById('edit-avatar-modal').classList.remove('opacity-0', 'pointer-events-none');
        }

        window.closeEditAvatarModal = function() {
            document.getElementById('edit-avatar-modal').classList.add('opacity-0', 'pointer-events-none');
            targetAvatarMemberId = "";
        }

        // บันทึกสถานะการเลือกสีพื้นหลังของอวาตาร์
        window.setAvatarBgColor = function(index) {
            selectedBgColor = avatarBgs[index];
            generateLiveEmojiAvatar();
            openEditAvatarModal(targetAvatarMemberId); // อัปเดตกริดเพื่อให้เห็นขอบสีฟ้าที่ถูกเลือก
        }

        // บันทึกสถานะการเลือกตัวอิโมจิอวาตาร์
        window.setAvatarEmoji = function(emoji) {
            selectedEmojiChar = emoji;
            generateLiveEmojiAvatar();
            openEditAvatarModal(targetAvatarMemberId); // อัปเดตกริด
        }

        // ประกอบและสร้างอวาตาร์ในรูป Raw SVG Data URL (ความคมชัดสูง ไม่กินแบนด์วิธ)
        function generateLiveEmojiAvatar() {
            const svgText = `<svg xmlns="http://www.w3.org/2000/svg" width="120" height="120" viewBox="0 0 120 120"><rect width="120" height="120" rx="60" fill="${selectedBgColor.match(/#\w+/g)[0]}"/><text x="60" y="85" font-size="64" text-anchor="middle" font-family="'Segoe UI Emoji', 'Apple Color Emoji', sans-serif">${selectedEmojiChar}</text></svg>`;
            const svgUrl = `data:image/svg+xml;utf8,${encodeURIComponent(svgText)}`;
            
            document.getElementById('edit-avatar-url').value = svgUrl;
            document.getElementById('avatar-live-preview').src = svgUrl;
        }

        // แสดงพรีวิวผลลัพธ์รูปภาพจากลิงก์ขณะพิมพ์
        window.previewLiveAvatar = function(url) {
            const previewImg = document.getElementById('avatar-live-preview');
            if (url && url.trim() !== "") {
                previewImg.src = url;
            } else {
                previewImg.src = "https://placehold.co/100x100/3b82f6/ffffff?text=User";
            }
        }

        // บันทึกการแก้ไขรูปโปรไฟล์ลงคลาวด์/Local Storage
        document.getElementById('save-avatar-btn').addEventListener('click', async () => {
            if (!targetAvatarMemberId) return;

            const urlInput = document.getElementById('edit-avatar-url').value.trim();

            if (!isFirebaseOnline) {
                const idx = members.findIndex(m => m.id === targetAvatarMemberId);
                if (idx !== -1) {
                    members[idx].avatar = urlInput;
                }
                localStorage.setItem('blue_local_members', JSON.stringify(members));
                loadLocalData();
                closeEditAvatarModal();
                showToast("เปลี่ยนรูปโปรไฟล์ส่วนตัวของคุณสำเร็จ!", "success");
                return;
            }

            try {
                const memberRef = doc(getMembersCollection(), targetAvatarMemberId);
                await updateDoc(memberRef, { avatar: urlInput });
                closeEditAvatarModal();
                showToast("อัปเดตแต่งรูปโปรไฟล์ของคุณขึ้นคลาวด์แล้ว!", "success");
            } catch (err) {
                console.error("เปลี่ยนรูปโปรไฟล์ขัดข้อง:", err);
                showToast("ไม่สามารถแก้ไขข้อมูลผ่านฐานข้อมูลหลักได้", "error");
            }
        });

        // ฟังก์ชันสำหรับหัวหน้าจัดการบัญชีรหัสผ่าน (Passcode Change)
        window.changeAdminPasscode = async function() {
            const newPass = document.getElementById('change-passcode-input').value.trim();
            if (!newPass || newPass.length < 4) {
                showToast("รหัสความปลอดภัยต้องมีความยาว 4 หลักขึ้นไป", "error");
                return;
            }

            if (!isFirebaseOnline) {
                adminPasscode = newPass;
                localStorage.setItem('blue_admin_passcode', newPass);
                document.getElementById('change-passcode-input').value = "";
                showToast("เปลี่ยนรหัสผ่านระดับอุปกรณ์หัวหน้าแล้ว", "success");
                return;
            }

            try {
                await setDoc(getSettingsDoc(), { passcode: newPass });
                adminPasscode = newPass;
                document.getElementById('change-passcode-input').value = "";
                showToast("แก้ไขบันทึกรหัสใหม่สู่คลาวด์แล้ว", "success");
            } catch (err) {
                console.error("การแก้ไขรหัสผ่านขัดข้อง:", err);
                showToast("ระบบขัดข้องในการอัปเกรดรหัสผ่าน", "error");
            }
        }

        // ----------------------------------------
        // โมดอลยืนยันผลพิเศษ (Custom Popups replacement)
        // ----------------------------------------
        let activeConfirmCallback = null;
        window.showConfirm = function(title, message, callback, isDanger = true) {
            document.getElementById('confirm-title').textContent = title;
            document.getElementById('confirm-message').textContent = message;
            activeConfirmCallback = callback;

            const okBtn = document.getElementById('confirm-ok-btn');
            if (isDanger) {
                okBtn.className = "flex-1 bg-rose-600 hover:bg-rose-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors";
            } else {
                okBtn.className = "flex-1 bg-blue-600 hover:bg-blue-700 text-white py-2.5 rounded-xl text-xs font-semibold transition-colors";
            }

            const modal = document.getElementById('confirm-modal');
            modal.classList.remove('opacity-0', 'pointer-events-none');
            modal.querySelector('div').classList.remove('scale-95');
        }

        window.closeConfirm = function() {
            const modal = document.getElementById('confirm-modal');
            modal.classList.add('opacity-0', 'pointer-events-none');
            modal.querySelector('div').classList.add('scale-95');
            activeConfirmCallback = null;
        }

        document.getElementById('confirm-ok-btn').addEventListener('click', () => {
            if (activeConfirmCallback) activeConfirmCallback();
        });

        // ----------------------------------------
        // การจัดการรอบประชุมและสภานักเรียน (Sessions Controls)
        // ----------------------------------------
        window.openCreateSessionModal = function() {
            if (!isAuthorizedMode) return;
            document.getElementById('create-session-modal').classList.remove('opacity-0', 'pointer-events-none');
            document.getElementById('new-session-date').value = new Date().toISOString().split('T')[0];
            document.getElementById('new-session-name').focus();
        }

        window.closeCreateSessionModal = function() {
            document.getElementById('create-session-modal').classList.add('opacity-0', 'pointer-events-none');
        }

        window.createNewSession = async function() {
            if (!isAuthorizedMode) return;
            const name = document.getElementById('new-session-name').value.trim();
            const date = document.getElementById('new-session-date').value;

            if (!name) {
                showToast("กรุณากรอกชื่อรอบกิจกรรมด้วยครับ", "error");
                return;
            }

            const sId = 'session_' + Date.now();
            const newSession = {
                id: sId,
                name: name,
                date: date || new Date().toISOString().split('T')[0],
                created_at: Date.now(),
                attendance: {}
            };

            if (!isFirebaseOnline) {
                sessions.unshift(newSession);
                localStorage.setItem('blue_local_sessions', JSON.stringify(sessions));
                currentSessionId = sId;
                loadLocalData();
                closeCreateSessionModal();
                document.getElementById('new-session-name').value = "";
                showToast(`สร้างรอบกิจกรรม "${name}" เรียบร้อย`, "success");
                return;
            }

            try {
                const sessionRef = doc(getSessionsCollection(), sId);
                await setDoc(sessionRef, newSession);
                currentSessionId = sId;
                closeCreateSessionModal();
                document.getElementById('new-session-name').value = "";
                showToast(`บันทึกรอบใหม่ "${name}" ลงระบบคลาวด์แล้ว`, "success");
            } catch (err) {
                console.error("สร้างรอบกิจกรรมล้มเหลว:", err);
            }
        }

        window.triggerDeleteCurrentSession = function() {
            if (!isAuthorizedMode) return;
            if (!currentSessionId) return;
            const currentSession = sessions.find(s => s.id === currentSessionId);
            if (!currentSession) return;

            showConfirm(
                "ลบรอบกิจกรรมถาวร",
                `คุณต้องการลบข้อมูลรอบกิจกรรม "${currentSession.name}" ออกจากสารระบบระบบคลาวด์หรือไม่?`,
                async () => {
                    if (!isFirebaseOnline) {
                        sessions = sessions.filter(s => s.id !== currentSessionId);
                        localStorage.setItem('blue_local_sessions', JSON.stringify(sessions));
                        currentSessionId = "";
                        loadLocalData();
                        closeConfirm();
                        showToast("ลบข้อมูลรอบกิจกรรมเรียบร้อย", "success");
                        return;
                    }

                    try {
                        await deleteDoc(doc(getSessionsCollection(), currentSessionId));
                        currentSessionId = "";
                        closeConfirm();
                        showToast("ลบรอบกิจกรรมออกจากฐานข้อมูลแล้ว", "success");
                    } catch (err) {
                        console.error("การลบข้อมูลล้มเหลว:", err);
                    }
                }
            );
        }

        // ----------------------------------------
        // แผนกจัดการเพิ่มลดรายชื่อ (Members Lists Settings)
        // ----------------------------------------
        window.openManageMembersModal = function() {
            if (!isAuthorizedMode) return;
            document.getElementById('manage-members-modal').classList.remove('opacity-0', 'pointer-events-none');
            renderManageMembers();
        }

        window.closeManageMembersModal = function() {
            document.getElementById('manage-members-modal').classList.add('opacity-0', 'pointer-events-none');
        }

        function renderManageMembers() {
            const tbody = document.getElementById('manage-members-tbody');
            let html = "";
            members.forEach(m => {
                html += `
                    <tr class="hover:bg-slate-50 border-b border-slate-100">
                        <td class="py-3 text-center font-bold text-slate-400">${m.order}</td>
                        <td class="py-3 text-center">
                            <img src="${getAvatarUrl(m)}" alt="Avatar" onerror="this.src='https://placehold.co/100x100/3b82f6/ffffff?text=User'" class="w-8 h-8 rounded-full object-cover border mx-auto">
                        </td>
                        <td class="py-3 font-semibold text-slate-700">${m.name}</td>
                        <td class="py-3">
                            <span class="px-2.5 py-0.5 rounded-full text-[10px] font-bold ${getRoleBadgeClass(m.role)}">
                                ${m.role}
                            </span>
                        </td>
                        <td class="py-3 text-center">
                            <div class="flex justify-center space-x-1.5">
                                <button onclick="triggerEditMemberField('${m.id}')" class="text-blue-500 hover:text-blue-700 text-xs px-1.5" title="แก้ไขข้อมูล">
                                    <i class="fa-solid fa-pen"></i>
                                </button>
                                <button onclick="triggerRemoveMember('${m.id}', '${m.name}')" class="text-rose-500 hover:text-rose-700 text-xs px-1.5" title="ลบออก">
                                    <i class="fa-solid fa-trash-can"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            });
            tbody.innerHTML = html;
        }

        window.triggerEditMemberField = function(mId) {
            const target = members.find(m => m.id === mId);
            if (!target) return;
            
            document.getElementById('new-member-name').value = target.name;
            document.getElementById('new-member-role').value = target.role;
            document.getElementById('new-member-avatar').value = target.avatar || "";
            
            document.getElementById('new-member-name').focus();
            
            const saveBtn = document.querySelector('[onclick="addMember()"]');
            saveBtn.setAttribute('data-edit-id', mId);
            saveBtn.innerHTML = `<i class="fa-solid fa-save"></i> <span>แก้ไขรายชื่อ</span>`;
        }

        window.addMember = async function() {
            if (!isAuthorizedMode) return;
            const name = document.getElementById('new-member-name').value.trim();
            const role = document.getElementById('new-member-role').value;
            const avatar = document.getElementById('new-member-avatar').value.trim();

            if (!name) {
                showToast("โปรดระบุชื่อสมาชิกด้วยครับ", "error");
                return;
            }

            const saveBtn = document.querySelector('[onclick="addMember()"]');
            const editId = saveBtn.getAttribute('data-edit-id');

            if (editId) {
                const updatedMember = { id: editId, name, role, avatar };
                const existing = members.find(m => m.id === editId);
                if (existing) {
                    updatedMember.order = existing.order;
                }

                if (!isFirebaseOnline) {
                    const idx = members.findIndex(m => m.id === editId);
                    if (idx !== -1) members[idx] = updatedMember;
                    localStorage.setItem('blue_local_members', JSON.stringify(members));
                    loadLocalData();
                    clearForm();
                    showToast(`แก้ไขข้อมูลของ "${name}" สำเร็จ`, "success");
                    return;
                }

                try {
                    await setDoc(doc(getMembersCollection(), editId), updatedMember);
                    clearForm();
                    showToast(`อัปเดตข้อมูลของ "${name}" เรียบร้อย`, "success");
                } catch (err) {
                    console.error("บันทึกการแก้ไขล้มเหลว:", err);
                }
            } else {
                const nextOrder = members.length > 0 ? Math.max(...members.map(m => m.order)) + 1 : 1;
                const mId = 'member_' + Date.now();
                const newMember = { id: mId, name, role, order: nextOrder, avatar };

                if (!isFirebaseOnline) {
                    members.push(newMember);
                    localStorage.setItem('blue_local_members', JSON.stringify(members));
                    loadLocalData();
                    clearForm();
                    showToast(`เพิ่มคุณ "${name}" สำเร็จแล้ว`, "success");
                    return;
                }

                try {
                    await setDoc(doc(getMembersCollection(), mId), newMember);
                    clearForm();
                    showToast(`เพิ่มคุณ "${name}" ลงระบบเรียบร้อย`, "success");
                } catch (err) {
                    console.error("ลงทะเบียนสมาชิกใหม่ขัดข้อง:", err);
                }
            }
        }

        function clearForm() {
            document.getElementById('new-member-name').value = "";
            document.getElementById('new-member-avatar').value = "";
            const saveBtn = document.querySelector('[onclick="addMember()"]');
            saveBtn.removeAttribute('data-edit-id');
            saveBtn.innerHTML = `<i class="fa-solid fa-plus"></i> <span>บันทึกรายชื่อ</span>`;
        }

        window.triggerRemoveMember = function(mId, name) {
            if (!isAuthorizedMode) return;
            showConfirm(
                "ลบรายชื่อผู้ปฏิบัติหน้าที่",
                `คุณแน่ใจว่าต้องการถอนคุณ "${name}" ออกจากระบบโครงสร้างเช็คชื่อของสภานักเรียนหรือไม่?`,
                async () => {
                    if (!isFirebaseOnline) {
                        members = members.filter(m => m.id !== mId);
                        localStorage.setItem('blue_local_members', JSON.stringify(members));
                        loadLocalData();
                        closeConfirm();
                        showToast(`ถอดถอนคุณ "${name}" สำเร็จ`, "success");
                        return;
                    }

                    try {
                        await deleteDoc(doc(getMembersCollection(), mId));
                        closeConfirm();
                        showToast(`ถอดถอนคุณ "${name}" เรียบร้อยแล้ว`, "success");
                    } catch (err) {
                        console.error("ลบสมาชิกสภาขัดข้อง:", err);
                    }
                }
            );
        }

        // ----------------------------------------
        // สไตล์สกินตามระดับตำแหน่งหน้าที่
        // ----------------------------------------
        window.setFilterRole = function(role) {
            filterRole = role;
            ['all', 'head', 'deputy', 'sec', 'member'].forEach(id => {
                const btn = document.getElementById(`filter-btn-${id}`);
                if (btn) {
                    btn.classList.remove('bg-blue-600', 'text-white', 'shadow-sm');
                    btn.classList.add('text-slate-600', 'hover:text-slate-950');
                }
            });

            let activeId = 'all';
            if (role === 'หัวหน้าฝ่าย') activeId = 'head';
            else if (role === 'รองหัวหน้าฝ่าย') activeId = 'deputy';
            else if (role === 'เลขาฝ่าย') activeId = 'sec';
            else if (role === 'สมาชิก') activeId = 'member';

            const activeBtn = document.getElementById(`filter-btn-${activeId}`);
            if (activeBtn) {
                activeBtn.classList.add('bg-blue-600', 'text-white', 'shadow-sm');
                activeBtn.classList.remove('text-slate-600', 'hover:text-slate-950');
            }

            applyFilters();
        }

        function getRoleBadgeClass(role) {
            switch(role) {
                case 'หัวหน้าฝ่าย':
                    return 'bg-blue-100 text-blue-700 border border-blue-200';
                case 'รองหัวหน้าฝ่าย':
                    return 'bg-sky-100 text-sky-700 border border-sky-200';
                case 'เลขาฝ่าย':
                    return 'bg-indigo-100 text-indigo-700 border border-indigo-200';
                default:
                    return 'bg-slate-100 text-slate-700 border border-slate-200';
            }
        }

        function formatDate(dateStr) {
            if (!dateStr) return "";
            const parts = dateStr.split('-');
            if (parts.length !== 3) return dateStr;
            const year = parseInt(parts[0]) + 543;
            const monthNames = ["ม.ค.", "ก.พ.", "มี.ค.", "เม.ย.", "พ.ค.", "มิ.ย.", "ก.ค.", "ส.ค.", "ก.ย.", "ต.ค.", "พ.ย.", "ธ.ค."];
            const month = monthNames[parseInt(parts[1]) - 1];
            const day = parseInt(parts[2]);
            return `${day} ${month} ${year}`;
        }

        function showToast(message, type = "info") {
            const toast = document.getElementById('toast');
            const icon = document.getElementById('toast-icon');
            const msgSpan = document.getElementById('toast-message');

            msgSpan.textContent = message;

            if (type === "success") {
                icon.className = "fa-solid fa-circle-check text-green-400 text-lg";
                toast.className = "fixed bottom-5 right-5 z-50 bg-slate-900 text-white py-3.5 px-5 rounded-xl shadow-xl flex items-center space-x-2.5 transition-all duration-300 transform translate-y-0 opacity-100";
            } else if (type === "error") {
                icon.className = "fa-solid fa-circle-exclamation text-rose-400 text-lg";
                toast.className = "fixed bottom-5 right-5 z-50 bg-slate-900 text-white py-3.5 px-5 rounded-xl shadow-xl flex items-center space-x-2.5 transition-all duration-300 transform translate-y-0 opacity-100";
            } else {
                icon.className = "fa-solid fa-circle-info text-blue-400 text-lg";
                toast.className = "fixed bottom-5 right-5 z-50 bg-slate-900 text-white py-3.5 px-5 rounded-xl shadow-xl flex items-center space-x-2.5 transition-all duration-300 transform translate-y-0 opacity-100";
            }

            setTimeout(() => {
                toast.className = "fixed bottom-5 right-5 z-50 bg-slate-900 text-white py-3.5 px-5 rounded-xl shadow-lg flex items-center space-x-2.5 transition-all duration-300 transform translate-y-10 opacity-0 pointer-events-none";
            }, 3500);
        }

        // ----------------------------------------
        // ฟังก์ชันเริ่มรันแอปพลิเคชัน (Initialization on Load)
        // ----------------------------------------
        window.onload = async function() {
            try {
                // เรียกคืนสถานะผู้ใช้งานล็อกอินจาก localStorage หากเคยลงชื่อไว้
                const storedMemberId = localStorage.getItem('blue_logged_in_member');
                if (storedMemberId) {
                    loggedInMemberId = storedMemberId;
                }

                await authenticateUser();
                await initializeDatabaseIfNeeded();
                setupDataListeners();
                
                document.getElementById('session-select').addEventListener('change', (e) => {
                    currentSessionId = e.target.value;
                    applyFilters();
                    updateStats();
                });

                // อัปเดตสกินสถานะผู้ควบคุม
                updateAuthStatusUI();

                showToast("ยินดีต้อนรับเข้าสู่ระบบเช็คชื่อสภาฝ่ายกิจกรรม", "success");
            } catch (err) {
                console.error("การโหลดระบบเริ่มต้นล้มเหลว:", err);
                isFirebaseOnline = false;
                initializeDatabaseIfNeeded();
            }
        }
    </script>
</body>
</html>

                   
