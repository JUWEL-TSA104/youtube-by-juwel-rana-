<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Webinar Invite Generator</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <style>
        body { font-family: 'Inter', sans-serif; }
        /* Custom scrollbar for textarea */
        textarea::-webkit-scrollbar { width: 8px; }
        textarea::-webkit-scrollbar-track { background: #f1f1f1; }
        textarea::-webkit-scrollbar-thumb { background: #d1d5db; border-radius: 4px; }
        textarea::-webkit-scrollbar-thumb:hover { background: #9ca3af; }
    </style>
<script type="importmap">
{
  "imports": {
    "react-dom/": "https://esm.sh/react-dom@^19.2.4/",
    "react/": "https://esm.sh/react@^19.2.4/",
    "react": "https://esm.sh/react@^19.2.4",
    "lucide-react": "https://esm.sh/lucide-react@^0.563.0"
  }
}
</script>
</head>
<body class="bg-gradient-to-br from-indigo-100 via-purple-100 to-pink-100 min-h-screen text-gray-800">

    <div class="max-w-6xl mx-auto px-4 py-8">
        
        <!-- Header -->
        <header class="mb-10 text-center relative">
            <!-- Desktop Clock -->
            <div class="absolute top-0 right-0 hidden md:flex items-center gap-2 bg-white/80 backdrop-blur px-4 py-2 rounded-full shadow-sm text-sm font-mono text-purple-700 border border-purple-100">
                <i data-lucide="globe" class="w-4 h-4"></i>
                <span id="liveClockDesktop">Loading...</span>
            </div>
            
            <h1 class="text-4xl md:text-5xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-purple-600 to-pink-600 mb-3 pt-8 md:pt-0">
                Webinar Invite Generator
            </h1>
            <p class="text-gray-600 text-lg">Select a speaker to automatically set the host and topic.</p>
            
            <!-- Mobile Clock -->
            <div class="md:hidden mt-4 inline-flex items-center gap-2 bg-white/80 px-4 py-1 rounded-full text-sm font-mono text-purple-700 shadow-sm">
                <i data-lucide="clock" class="w-4 h-4"></i>
                <span id="liveClockMobile">Loading...</span>
            </div>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 items-start">
            
            <!-- Left Column: Configuration Form -->
            <div class="bg-white/90 backdrop-blur-sm rounded-2xl p-6 shadow-xl border border-white/50">
                <div class="flex items-center gap-2 mb-6 border-b border-gray-100 pb-4">
                    <div class="bg-purple-100 p-2 rounded-lg">
                        <i data-lucide="users" class="w-6 h-6 text-purple-600"></i>
                    </div>
                    <h2 class="text-xl font-bold text-gray-800">Configuration</h2>
                </div>

                <div class="space-y-5">
                    <!-- Profile Selector -->
                    <div class="space-y-2">
                        <label class="text-sm font-bold text-gray-700 block">Select Speaker</label>
                        <div class="relative">
                            <select id="profileSelect" class="w-full px-4 py-3 rounded-xl border border-gray-300 bg-white focus:ring-2 focus:ring-purple-500 focus:border-purple-500 transition-all appearance-none text-gray-700 font-medium">
                                <optgroup label="Select Speaker" id="speakerOptions">
                                    <!-- Options populated by JS -->
                                </optgroup>
                                <option value="custom">-- Custom / Manual Entry --</option>
                            </select>
                            <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-4 text-gray-500">
                                <svg class="h-4 w-4 fill-current" viewBox="0 0 20 20"><path d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"/></svg>
                            </div>
                        </div>
                    </div>

                    <div class="h-px bg-gray-200 my-4"></div>

                    <!-- Input Fields -->
                    <div class="space-y-4">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="space-y-1.5">
                                <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                    <i data-lucide="user" class="w-3.5 h-3.5"></i> Speaker Name
                                </label>
                                <input type="text" id="speakerName" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                    <i data-lucide="briefcase" class="w-3.5 h-3.5"></i> Speaker Title
                                </label>
                                <input type="text" id="speakerTitle" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all">
                            </div>
                        </div>

                        <div class="space-y-1.5">
                            <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                <i data-lucide="users" class="w-3.5 h-3.5"></i> Host Name
                            </label>
                            <input type="text" id="hostName" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all">
                        </div>

                        <div class="space-y-1.5">
                            <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                <i data-lucide="file-text" class="w-3.5 h-3.5"></i> Topic
                            </label>
                            <input type="text" id="topic" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all">
                        </div>

                        <div class="space-y-1.5">
                            <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                <i data-lucide="link" class="w-3.5 h-3.5"></i> Webinar Link
                            </label>
                            <input type="text" id="link" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all text-blue-600">
                        </div>

                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="space-y-1.5">
                                <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                    <i data-lucide="calendar" class="w-3.5 h-3.5"></i> Date
                                </label>
                                <input type="date" id="date" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all font-mono text-sm">
                            </div>
                            <div class="space-y-1.5">
                                <label class="text-xs font-semibold text-gray-500 uppercase tracking-wide flex items-center gap-1">
                                    <i data-lucide="clock" class="w-3.5 h-3.5"></i> Time
                                </label>
                                <input type="time" id="time" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:ring-2 focus:ring-purple-400 focus:border-transparent outline-none transition-all font-mono text-sm">
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Right Column: Live Preview -->
            <div class="flex flex-col h-full shadow-2xl rounded-2xl overflow-hidden bg-white">
                <div class="bg-gradient-to-r from-gray-900 to-gray-800 text-white p-4 flex justify-between items-center">
                    <h2 class="font-bold flex items-center gap-2">
                        <i data-lucide="file-text" class="w-5 h-5 text-purple-400"></i>
                        Live Preview
                    </h2>
                    <span class="text-xs bg-gray-700 px-2 py-1 rounded text-gray-300 font-mono">Auto-updates</span>
                </div>

                <div class="flex-1 bg-gray-100 flex flex-col relative">
                    <textarea 
                        id="previewArea"
                        readonly
                        class="flex-1 p-6 m-0 w-full h-[500px] lg:h-auto text-sm leading-relaxed text-gray-900 bg-gray-50 resize-none outline-none font-mono border-none focus:ring-0"
                        spellcheck="false"
                    ></textarea>
                    
                    <div class="absolute bottom-4 right-4 left-4">
                       <button 
                        id="copyBtn"
                        class="w-full py-4 px-6 rounded-xl font-bold text-lg shadow-lg flex items-center justify-center gap-3 transition-all duration-300 transform active:scale-95 bg-gradient-to-r from-purple-600 to-pink-600 text-white hover:from-purple-700 hover:to-pink-700"
                      >
                        <i data-lucide="copy" class="w-6 h-6" id="copyIcon"></i>
                        <span id="copyText">Copy Invitation Text</span>
                      </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Application Logic -->
    <script>
        // Data Configuration
        const SPEAKER_PROFILES = [
            {
                id: 'ashika',
                speaker: "ASHIKA YEASMIN",
                host: "TAHMIDUR RAHMAN",
                topic: "DIGITAL EARNING OPPORTUNITY",
                title: "Trainer"
            },
            {
                id: 'shohid',
                speaker: "SHOHID UDDIN",
                host: "AKRIZA SULTANA",
                topic: "INDUSTY INTRODUCTION",
                title: "Trainer"
            },
            {
                id: 'ruth',
                speaker: "RUTH CHOWDHURY",
                host: "TAHMIDUR RAHMAN",
                topic: "FOREVER MARKETING PLAN",
                title: "Trainer"
            },
            {
                id: 'sinthia',
                speaker: "Sinthia Asha",
                host: "Tahmidur Rahman",
                topic: "Product knowledge",
                title: "Trainer"
            },
            {
                id: 'shahid_study',
                speaker: "Shahid Uddin",
                host: "shahnaj sahin",
                topic: "How to study your business",
                title: "Trainer"
            },
            {
                id: 'ayon',
                speaker: "Ayon Jahir",
                host: "Sumaiya",
                topic: "WHY NOT FOREVER LIVINIG PRODUCTS?",
                title: "Trainer"
            }
        ];

        // State
        const state = {
            speakerName: "",
            speakerTitle: "",
            hostName: "",
            topic: "",
            link: "https://youtu.be/TC_IA3Jc2OU",
            date: new Date().toISOString().split('T')[0], // Today YYYY-MM-DD
            time: "21:00" // 9 PM
        };

        // DOM Elements
        const els = {
            profileSelect: document.getElementById('profileSelect'),
            speakerOptions: document.getElementById('speakerOptions'),
            speakerName: document.getElementById('speakerName'),
            speakerTitle: document.getElementById('speakerTitle'),
            hostName: document.getElementById('hostName'),
            topic: document.getElementById('topic'),
            link: document.getElementById('link'),
            date: document.getElementById('date'),
            time: document.getElementById('time'),
            previewArea: document.getElementById('previewArea'),
            copyBtn: document.getElementById('copyBtn'),
            copyText: document.getElementById('copyText'),
            copyIcon: document.getElementById('copyIcon'),
            liveClockDesktop: document.getElementById('liveClockDesktop'),
            liveClockMobile: document.getElementById('liveClockMobile')
        };

        // Helper Functions
        function formatDateForPreview(dateStr) {
            if (!dateStr) return '';
            const d = new Date(dateStr);
            if (isNaN(d.getTime())) return dateStr;
            const day = d.getDate();
            const month = d.toLocaleString('en-US', { month: 'long' });
            const year = d.getFullYear();

            const getOrdinal = (n) => {
                if (n > 3 && n < 21) return 'th';
                switch (n % 10) {
                    case 1:  return "st";
                    case 2:  return "nd";
                    case 3:  return "rd";
                    default: return "th";
                }
            };
            return `${day}${getOrdinal(day)} ${month}, ${year}`;
        }

        function formatTimeForPreview(timeStr) {
            if (!timeStr) return '';
            const [hStr, mStr] = timeStr.split(':');
            const h = parseInt(hStr, 10);
            const m = parseInt(mStr, 10);
            if (isNaN(h) || isNaN(m)) return timeStr;
            
            const ampm = h >= 12 ? 'PM' : 'AM';
            const h12 = h % 12 || 12;
            
            if (m === 0) return `${h12}${ampm}`;
            return `${h12}:${m.toString().padStart(2, '0')}${ampm}`;
        }

        function generateTemplate() {
            const formattedDate = formatDateForPreview(state.date);
            const formattedTime = formatTimeForPreview(state.time);

            return `✅PERSONAL GUIDELINE✨

🎯Get Ready To Become a Business Owner🎀

🎯🎯Topic:   🌠${state.topic}

‼️Congratulations‼️
You're Selected To Attend This 
Webinar❤️‍🔥❤️‍🔥❤️‍🔥

                Host🥀
🎤${state.hostName}

          Speaker🥀
🎤${state.speakerName}
     ${state.speakerTitle}

🗣️We are inviting you to a schedule YouTube premier ‼️

📲Join Youtube premier👇

${state.link}

     Date: "${formattedDate}"
  ⏰⏰⏰START AT ${formattedTime}‼️

Participate On Time Everyone & Don't Miss This Opportunity💯`;
        }

        function updateUI() {
            els.speakerName.value = state.speakerName;
            els.speakerTitle.value = state.speakerTitle;
            els.hostName.value = state.hostName;
            els.topic.value = state.topic;
            els.link.value = state.link;
            els.date.value = state.date;
            els.time.value = state.time;
            
            els.previewArea.value = generateTemplate();
        }

        function loadProfile(id) {
            const profile = SPEAKER_PROFILES.find(p => p.id === id);
            if (profile) {
                state.speakerName = profile.speaker;
                state.hostName = profile.host;
                state.topic = profile.topic;
                state.speakerTitle = profile.title || "Trainer";
                updateUI();
            }
        }

        // Event Listeners
        function setupEventListeners() {
            // Dropdown
            els.profileSelect.addEventListener('change', (e) => {
                const val = e.target.value;
                if (val !== 'custom') {
                    loadProfile(val);
                }
            });

            // Input fields sync
            const inputs = ['speakerName', 'speakerTitle', 'hostName', 'topic', 'link', 'date', 'time'];
            inputs.forEach(id => {
                els[id].addEventListener('input', (e) => {
                    state[id] = e.target.value;
                    // If modifying specific fields, set select to custom (visually)
                    if (['speakerName', 'hostName', 'topic'].includes(id)) {
                        els.profileSelect.value = 'custom';
                    }
                    els.previewArea.value = generateTemplate();
                });
            });

            // Copy Button
            els.copyBtn.addEventListener('click', async () => {
                try {
                    await navigator.clipboard.writeText(els.previewArea.value);
                    
                    // Feedback UI
                    const originalBg = els.copyBtn.className;
                    els.copyBtn.classList.remove('from-purple-600', 'to-pink-600', 'hover:from-purple-700', 'hover:to-pink-700');
                    els.copyBtn.classList.add('bg-green-500', 'hover:bg-green-600');
                    els.copyText.innerText = "Copied to Clipboard!";
                    
                    // Replace icon logic manually since Lucide renders SVGs
                    // Simple fix: just update text for now, maybe reset later
                    
                    setTimeout(() => {
                        els.copyBtn.classList.remove('bg-green-500', 'hover:bg-green-600');
                        els.copyBtn.classList.add('from-purple-600', 'to-pink-600', 'hover:from-purple-700', 'hover:to-pink-700');
                        els.copyText.innerText = "Copy Invitation Text";
                    }, 2000);
                } catch (err) {
                    console.error('Failed to copy', err);
                }
            });
        }

        // Live Clock
        function startClock() {
            const updateTime = () => {
                const now = new Date();
                const timeString = now.toLocaleTimeString('en-US', { hour12: true });
                els.liveClockDesktop.innerText = timeString;
                els.liveClockMobile.innerText = timeString;
            };
            updateTime();
            setInterval(updateTime, 1000);
        }

        // Initialization
        function init() {
            // Populate select options
            SPEAKER_PROFILES.forEach(p => {
                const opt = document.createElement('option');
                opt.value = p.id;
                opt.innerText = p.speaker;
                els.speakerOptions.appendChild(opt);
            });

            // Load initial profile (Ashika)
            loadProfile('ashika');
            
            // Set up events
            setupEventListeners();
            
            // Start clock
            startClock();
            
            // Initialize Icons
            lucide.createIcons();
        }

        init();
    </script>
</body>
</html>
