<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>TaskEarn USDT - Earn with BSC Network</title>
    <!-- Tailwind + Font Awesome -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        * {
            -webkit-tap-highlight-color: transparent;
        }
        body {
            font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, Helvetica, sans-serif;
            background: #0f172a;
        }
        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-track {
            background: #1e293b;
        }
        ::-webkit-scrollbar-thumb {
            background: #3b82f6;
            border-radius: 10px;
        }
        .task-card {
            transition: all 0.2s ease;
        }
        .task-card:active {
            transform: scale(0.99);
        }
        .toast-notify {
            animation: slideUp 0.3s ease forwards;
        }
        @keyframes slideUp {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
        .btn-press {
            transition: transform 0.05s linear;
        }
        .btn-press:active {
            transform: scale(0.96);
        }
        .slide-up-modal {
            animation: modalSlide 0.3s ease;
        }
        @keyframes modalSlide {
            from {
                transform: translateY(100%);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
        .network-badge {
            background: linear-gradient(135deg, #0052ff, #00d4ff);
        }
        input:focus {
            outline: none;
            border-color: #3b82f6;
        }
        
        .wallet-container {
            background-color: #1e293b;
            border-radius: 1.5rem;
            padding: 1.5rem;
        }
        .btn-withdraw-initiate {
            background-color: #10b981;
            transition: all 0.2s;
        }
        .btn-withdraw-initiate:hover {
            background-color: #059669;
        }
        .history-item {
            background-color: #0f172a;
            border-radius: 0.75rem;
        }
    </style>
</head>
<body class="bg-[#0f172a] text-white select-none relative">

    <div id="toastContainer" class="fixed top-16 left-4 right-4 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <div class="flex flex-col min-h-screen">
        <div class="p-4 flex items-center justify-between border-b border-gray-800 bg-[#1e293b] sticky top-0 z-20">
            <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-full flex items-center justify-center font-bold shadow-lg">
                    <span class="text-sm">TE</span>
                </div>
                <div>
                    <p class="text-sm font-semibold tracking-wide">Lumina</p>
                    <p class="text-xs text-green-400 flex items-center gap-1"><i class="fa-solid fa-circle-check text-[10px]"></i> Active Member</p>
                </div>
            </div>
            <button id="notificationBtn" class="bg-gray-700/80 hover:bg-gray-600 p-2 rounded-xl text-sm transition-all btn-press relative">
                <i class="fa-solid fa-bell"></i>
            </button>
        </div>

        <!-- Balance Card - Buttons Removed -->
        <div class="m-4 mt-5 p-5 rounded-2xl bg-gradient-to-r from-blue-600 via-blue-700 to-indigo-800 shadow-xl relative overflow-hidden">
            <div class="absolute top-0 right-0 w-32 h-32 bg-white/5 rounded-full -mr-10 -mt-10"></div>
            <p class="text-xs font-medium opacity-90 tracking-wide">AVAILABLE BALANCE</p>
            <h1 class="text-4xl font-extrabold mt-1 tracking-tight" id="balanceAmount">12.50</h1>
            <p class="text-sm font-medium -mt-1 opacity-90">USDT</p>
        </div>

        <div class="px-4 pb-28 flex-1">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-xl font-bold bg-gradient-to-r from-blue-300 to-white bg-clip-text text-transparent" id="sectionTitle">✨ Active Missions</h2>
                <span id="taskCounterBadge" class="bg-blue-500/30 text-blue-300 text-xs px-2 py-1 rounded-full">3 available</span>
            </div>
            <div id="taskListContainer" class="space-y-3"></div>
        </div>

        <div class="fixed bottom-0 left-0 right-0 bg-[#1e293b]/90 backdrop-blur-md border-t border-gray-700 flex justify-around py-2 pb-3 z-30">
            <div data-tab="home" class="bottom-nav-item flex flex-col items-center cursor-pointer transition-all duration-150 py-1 px-3 rounded-xl text-white">
                <i class="fa-solid fa-house text-xl"></i>
                <span class="text-[10px] font-medium mt-1">Home</span>
            </div>
            <div data-tab="tasks" class="bottom-nav-item flex flex-col items-center cursor-pointer transition-all duration-150 py-1 px-3 rounded-xl text-gray-400">
                <i class="fa-solid fa-list-check text-xl"></i>
                <span class="text-[10px] font-medium mt-1">Tasks</span>
            </div>
            <div data-tab="wallet" class="bottom-nav-item flex flex-col items-center cursor-pointer transition-all duration-150 py-1 px-3 rounded-xl text-gray-400">
                <i class="fa-solid fa-wallet text-xl"></i>
                <span class="text-[10px] font-medium mt-1">Wallet</span>
            </div>
            <div data-tab="profile" class="bottom-nav-item flex flex-col items-center cursor-pointer transition-all duration-150 py-1 px-3 rounded-xl text-gray-400">
                <i class="fa-solid fa-user text-xl"></i>
                <span class="text-[10px] font-medium mt-1">Profile</span>
            </div>
        </div>
    </div>

    <div id="historyModal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 flex items-end justify-center hidden transition-all duration-300">
        <div class="bg-[#0f172a] w-full max-w-md rounded-t-2xl p-5 border-t border-gray-700 slide-up-modal max-h-[70vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-gray-800 pb-3">
                <h3 class="text-xl font-bold"><i class="fa-regular fa-clock mr-2"></i>Transaction History</h3>
                <button id="closeHistoryBtn" class="text-gray-400 hover:text-white text-xl">&times;</button>
            </div>
            <div id="historyList" class="mt-4 space-y-3"></div>
        </div>
    </div>

    <div id="withdrawModal" class="fixed inset-0 bg-black/90 backdrop-blur-md z-50 flex items-center justify-center hidden transition-all">
        <div class="bg-[#1e293b] rounded-2xl p-6 w-[90%] max-w-md border border-gray-700 shadow-2xl slide-up-modal">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-bold bg-gradient-to-r from-blue-400 to-cyan-400 bg-clip-text text-transparent">💸 Withdraw USDT</h3>
                <button id="closeWithdrawBtn" class="text-gray-400 hover:text-white text-2xl">&times;</button>
            </div>
            
            <div class="mb-5 p-3 rounded-xl network-badge bg-gradient-to-r from-blue-600 to-cyan-600">
                <div class="flex items-center justify-between">
                    <div class="flex items-center gap-2">
                        <i class="fa-brands fa-ethereum text-white text-xl"></i>
                        <div>
                            <p class="text-xs font-bold text-white/80">Network</p>
                            <p class="text-white font-bold text-sm">BSC (BEP-20)</p>
                        </div>
                    </div>
                    <div class="text-right">
                        <p class="text-[10px] text-white/70">Min: 0.2 USDT</p>
                        <p class="text-[10px] text-white/70">Fee: 0.05 USDT</p>
                    </div>
                </div>
            </div>
            
            <div class="mb-4">
                <label class="text-xs font-medium text-gray-300 mb-1 block">Withdrawal Address (BEP-20)</label>
                <div class="flex items-center bg-[#0f172a] rounded-xl p-3 border border-gray-700 focus-within:border-blue-500 transition">
                    <i class="fa-regular fa-address-card text-blue-400 mr-2"></i>
                    <input type="text" id="withdrawAddress" placeholder="0x... or BSC address" class="bg-transparent flex-1 outline-none text-white text-sm placeholder:text-gray-500">
                </div>
            </div>
            
            <div class="mb-4">
                <label class="text-xs font-medium text-gray-300 mb-1 block">Withdrawal Amount (USDT)</label>
                <div class="flex items-center bg-[#0f172a] rounded-xl p-3 border border-gray-700 focus-within:border-blue-500 transition">
                    <span class="text-blue-400 font-bold mr-2">$</span>
                    <input type="number" id="withdrawAmount" placeholder="Minimum 0.2" class="bg-transparent flex-1 outline-none text-white text-lg" step="0.01" min="0.2">
                </div>
                <p class="text-[10px] text-gray-500 mt-1">Fee of 0.05 USDT will be deducted from this amount</p>
            </div>
            
            <div class="bg-[#0f172a] rounded-xl p-3 mb-5 border border-gray-700">
                <div class="flex justify-between text-sm mb-2">
                    <span class="text-gray-400">Withdrawal Amount:</span>
                    <span id="summaryAmount" class="text-white font-medium">0.00 USDT</span>
                </div>
                <div class="flex justify-between text-sm mb-2">
                    <span class="text-gray-400">Network Fee (deducted):</span>
                    <span class="text-orange-400">0.05 USDT</span>
                </div>
                <div class="flex justify-between text-sm font-bold pt-2 border-t border-gray-700 mt-1">
                    <span class="text-gray-300">You Will Receive:</span>
                    <span id="summaryTotal" class="text-green-400 text-base">0.00 USDT</span>
                </div>
            </div>
            
            <div id="withdrawError" class="text-red-400 text-xs mb-3 text-center hidden bg-red-500/10 p-2 rounded-lg"></div>
            
            <div class="flex gap-3">
                <button id="confirmWithdrawBtn" class="flex-1 bg-gradient-to-r from-blue-600 to-cyan-600 hover:from-blue-500 hover:to-cyan-500 py-3 rounded-xl font-bold transition-all btn-press shadow-lg">Confirm Withdrawal</button>
                <button id="cancelWithdrawBtn" class="flex-1 bg-gray-700 hover:bg-gray-600 py-3 rounded-xl font-bold btn-press">Cancel</button>
            </div>
            <p class="text-[10px] text-center text-gray-500 mt-3">⏱️ Processing time: 2-10 minutes</p>
        </div>
    </div>

    <script>
        let balance = 12.50;
        const NETWORK_FEE = 0.05;
        const MIN_WITHDRAW = 0.20;
        
        let tasks = [
            { id: 1, title: "Join Lumina Channel", reward: 0.05, icon: "fa-brands fa-telegram", bg: "bg-black", platform: "Telegram", status: "available", link: "https://t.me/gloal_lumina", actionText: "Start" },
            { id: 2, title: "Join Prime X Group", reward: 0.03, icon: "fa-brands fa-telegram", bg: "bg-[#229ED9]", platform: "Telegram", status: "available", link: "https://t.me/primexgroup", actionText: "Join" },
            { id: 3, title: "Explore BASE Ecosystem", reward: 0.08, icon: "fa-brands fa-ethereum", bg: "bg-purple-600", platform: "BASE", status: "available", link: "https://base.org", actionText: "Explore" },
            { id: 4, title: "Retweet Campaign", reward: 0.04, icon: "fa-solid fa-retweet", bg: "bg-cyan-600", platform: "X", status: "available", link: "https://twitter.com/intent/retweet", actionText: "Retweet" },
        ];

        let completedTasks = new Set();
        let transactionHistory = [];
        
        function getTimeStamp() {
            return new Date().toLocaleString();
        }
        
        function addHistory(type, amount, details = {}) {
            let record = { type, amount, date: getTimeStamp(), ...details };
            transactionHistory.unshift(record);
            updateHistoryModalUI();
            if (type === "earn") {
                showToast(`${amount.toFixed(2)} USDT earned!`, "success");
            } else if (type === "withdraw") {
                const received = amount - NETWORK_FEE;
                showToast(`Withdrew ${amount.toFixed(2)} USDT, received ${received.toFixed(2)} USDT after ${NETWORK_FEE} USDT fee`, "info");
            }
            saveAppData();
        }
        
        function showToast(message, type = "success") {
            const container = document.getElementById("toastContainer");
            const toast = document.createElement("div");
            toast.className = `toast-notify bg-gray-800/95 backdrop-blur-md border-l-4 ${type === 'success' ? 'border-green-500' : 'border-blue-500'} rounded-xl p-3 shadow-xl flex items-center gap-3 pointer-events-auto`;
            toast.innerHTML = `
                <i class="${type === 'success' ? 'fa-regular fa-circle-check text-green-400' : 'fa-solid fa-circle-info text-blue-400'} text-lg"></i>
                <span class="text-sm font-medium">${message}</span>
                <button class="ml-auto text-gray-400 hover:text-white text-xs closeToast">✕</button>
            `;
            container.appendChild(toast);
            toast.querySelector(".closeToast").addEventListener("click", () => toast.remove());
            setTimeout(() => toast.remove(), 3000);
        }
        
        function updateBalanceUI() {
            document.getElementById("balanceAmount").innerText = balance.toFixed(2);
        }
        
        function renderTasks() {
            const container = document.getElementById("taskListContainer");
            if (!container) return;
            let availableCount = 0;
            const tasksHtml = tasks.map(task => {
                const isCompleted = completedTasks.has(task.id);
                let buttonText = isCompleted ? "✓ Completed" : (task.actionText || "Start");
                let buttonClasses = isCompleted ? "bg-green-600/70 cursor-default opacity-80" : "bg-blue-600 hover:bg-blue-500 btn-press";
                if (!isCompleted) availableCount++;
                
                return `
                    <div class="task-card p-4 rounded-xl bg-[#1e293b] flex items-center justify-between border border-gray-800 transition-all ${isCompleted ? 'opacity-70' : ''}">
                        <div class="flex items-center gap-4">
                            <div class="w-12 h-12 ${task.bg} rounded-xl flex items-center justify-center text-xl shadow-md">
                                <i class="${task.icon}"></i>
                            </div>
                            <div>
                                <h3 class="text-sm font-semibold tracking-wide">${task.title}</h3>
                                <p class="text-xs text-blue-400 font-mono">+ ${task.reward.toFixed(2)} USDT</p>
                                <p class="text-[10px] text-gray-500">${task.platform || 'Task'}</p>
                            </div>
                        </div>
                        <button class="task-action-btn px-4 py-1.5 rounded-full text-xs font-bold transition-all shadow ${buttonClasses}" data-id="${task.id}" ${isCompleted ? 'disabled' : ''}>
                            ${buttonText}
                        </button>
                    </div>
                `;
            }).join('');
            container.innerHTML = tasksHtml;
            document.getElementById("taskCounterBadge").innerHTML = `${availableCount} available`;
            
            document.querySelectorAll('.task-action-btn').forEach(btn => {
                if (btn.disabled) return;
                btn.addEventListener('click', () => {
                    const taskId = parseInt(btn.getAttribute('data-id'));
                    const task = tasks.find(t => t.id === taskId);
                    if (task && !completedTasks.has(task.id)) startTask(task);
                });
            });
        }
        
        function startTask(task) {
            if (task.link) {
                if (confirm(`📱 Complete: ${task.title}\nYou'll be redirected to ${task.platform}. After completion, click OK to verify.`)) {
                    window.open(task.link, '_blank');
                    setTimeout(() => {
                        if (confirm(`✅ Did you complete "${task.title}"? Earn +${task.reward.toFixed(2)} USDT`)) {
                            completeTask(task);
                        } else {
                            showToast("❌ Verification cancelled", "info");
                        }
                    }, 800);
                }
            } else {
                if (confirm(`✨ Complete "${task.title}" for +${task.reward.toFixed(2)} USDT?`)) {
                    completeTask(task);
                }
            }
        }
        
        function completeTask(task) {
            if (completedTasks.has(task.id)) return;
            balance += task.reward;
            updateBalanceUI();
            completedTasks.add(task.id);
            addHistory("earn", task.reward, { taskTitle: task.title });
            renderTasks();
            showToast(`🎉 +${task.reward.toFixed(2)} USDT for "${task.title}"`, "success");
            saveAppData();
        }
        
        function attemptWithdraw(address, requestedAmount) {
            if (!address || address.trim() === "") {
                return { success: false, msg: "❌ Please enter a valid BEP-20 withdrawal address" };
            }
            if (!address.match(/^(0x[a-fA-F0-9]{40}|[a-zA-Z0-9]{42,})$/)) {
                return { success: false, msg: "❌ Invalid address format. Use BSC (BEP-20) address" };
            }
            if (isNaN(requestedAmount) || requestedAmount < MIN_WITHDRAW) {
                return { success: false, msg: `⚠️ Minimum withdrawal is ${MIN_WITHDRAW} USDT` };
            }
            
            const amountReceived = requestedAmount - NETWORK_FEE;
            
            if (amountReceived <= 0) {
                return { success: false, msg: `⚠️ Withdrawal amount must be greater than the ${NETWORK_FEE} USDT fee. Minimum is ${MIN_WITHDRAW} USDT` };
            }
            
            if (requestedAmount > balance) {
                return { success: false, msg: `💸 Insufficient balance! You have ${balance.toFixed(2)} USDT, but requested ${requestedAmount.toFixed(2)} USDT` };
            }
            
            balance -= requestedAmount;
            updateBalanceUI();
            
            addHistory("withdraw", requestedAmount, { 
                address: address.substring(0, 6) + "..." + address.substring(address.length-4),
                network: "BSC (BEP-20)",
                fee: NETWORK_FEE,
                amountReceived: amountReceived
            });
            saveAppData();
            
            return { 
                success: true, 
                msg: `✅ Withdrawal initiated!\n\nRequested: ${requestedAmount.toFixed(2)} USDT\nNetwork Fee: ${NETWORK_FEE.toFixed(2)} USDT\nYou will receive: ${amountReceived.toFixed(2)} USDT\n\nSent to: ${address.substring(0,6)}...${address.substring(address.length-4)}` 
            };
        }
        
        function updateHistoryModalUI() {
            const historyDiv = document.getElementById("historyList");
            if (!historyDiv) return;
            if (transactionHistory.length === 0) {
                historyDiv.innerHTML = `<div class="text-center text-gray-500 py-6 text-sm">✨ No transactions yet. Complete tasks!</div>`;
                return;
            }
            historyDiv.innerHTML = transactionHistory.map(t => {
                const isEarn = t.type === "earn";
                const sign = isEarn ? "+" : "-";
                const color = isEarn ? "text-green-400" : "text-orange-400";
                let extraDetails = "";
                if (t.type === "withdraw" && t.address) {
                    extraDetails = `<p class="text-[10px] text-gray-500 mt-1">${t.network} | To: ${t.address}<br>Fee: ${NETWORK_FEE.toFixed(2)} USDT | Received: ${(t.amount - NETWORK_FEE).toFixed(2)} USDT</p>`;
                } else if (t.taskTitle) {
                    extraDetails = `<p class="text-[10px] text-gray-500 mt-1">📋 ${t.taskTitle}</p>`;
                }
                return `
                    <div class="flex justify-between items-start border-b border-gray-800 pb-3">
                        <div class="flex-1">
                            <p class="text-sm font-medium flex items-center gap-2">
                                <i class="${isEarn ? 'fa-regular fa-circle-up' : 'fa-regular fa-circle-down'} ${color}"></i>
                                ${t.type === "earn" ? "Task Reward" : "Withdrawal"}
                            </p>
                            ${extraDetails}
                            <p class="text-[10px] text-gray-500">${t.date}</p>
                        </div>
                        <p class="font-mono font-bold ${color}">${sign} ${t.amount.toFixed(2)} USDT</p>
                    </div>
                `;
            }).join('');
        }
        
        function renderWalletHistory() {
            if (transactionHistory.length === 0) {
                return `<div class="history-item p-4 flex justify-between items-center border border-gray-800/50">
                            <div><p class="text-sm font-bold"><i class="fa-solid fa-circle-arrow-down text-green-400"></i> Welcome Bonus</p>
                            <p class="text-[10px] text-gray-500">${new Date().toLocaleString()}</p></div>
                            <p class="text-green-400 font-bold">+ 12.50 USDT</p>
                        </div>`;
            }
            return transactionHistory.map(t => `
                <div class="history-item p-4 flex justify-between items-center border border-gray-800/50">
                    <div>
                        <p class="text-sm font-bold"><i class="fa-solid ${t.type === 'earn' ? 'fa-circle-arrow-down text-green-400' : 'fa-circle-arrow-up text-orange-400'}"></i> ${t.type === 'earn' ? (t.taskTitle || 'Reward') : 'Withdrawal'}</p>
                        ${t.type === 'withdraw' ? `<p class="text-[9px] text-gray-600">Fee: ${NETWORK_FEE.toFixed(2)} USDT</p>` : ''}
                        <p class="text-[10px] text-gray-500">${t.date}</p>
                    </div>
                    <p class="${t.type === 'earn' ? 'text-green-400' : 'text-orange-400'} font-bold">${t.type === 'earn' ? '+' : '-'} ${t.amount.toFixed(2)} USDT</p>
                </div>
            `).join('');
        }
        
        function saveAppData() {
            localStorage.setItem("taskEarnData", JSON.stringify({ balance, completedTasks: Array.from(completedTasks), transactionHistory }));
        }
        
        function loadAppData() {
            const saved = localStorage.getItem("taskEarnData");
            if (saved) {
                const data = JSON.parse(saved);
                if (typeof data.balance === "number") balance = data.balance;
                if (Array.isArray(data.completedTasks)) completedTasks = new Set(data.completedTasks);
                if (Array.isArray(data.transactionHistory)) transactionHistory = data.transactionHistory;
                updateBalanceUI();
                renderTasks();
                updateHistoryModalUI();
            } else {
                renderTasks();
            }
        }
        
        function setActiveTab(tabId) {
            const navItems = document.querySelectorAll(".bottom-nav-item");
            const mainContent = document.getElementById("taskListContainer");
            const sectionTitle = document.getElementById("sectionTitle");
            const taskBadge = document.getElementById("taskCounterBadge");
            const balanceCard = document.querySelector(".m-4.mt-5.p-5.rounded-2xl");
        
            navItems.forEach(item => {
                item.classList.remove("text-white");
                item.classList.add("text-gray-400");
                if(item.getAttribute("data-tab") === tabId) item.classList.add("text-white");
            });
        
            if (tabId === "wallet") {
                sectionTitle.classList.add("hidden");
                taskBadge.classList.add("hidden");
                balanceCard.style.display = "block";
                mainContent.innerHTML = `
                    <div class="wallet-container shadow-2xl">
                        <div class="flex items-center gap-3 mb-6"><i class="fa-solid fa-wallet text-blue-400 text-2xl"></i><h2 class="text-xl font-bold">Wallet Overview</h2></div>
                        <div class="p-5 rounded-2xl bg-[#162031] border border-gray-800 mb-6">
                            <div class="flex justify-between items-start mb-2">
                                <div class="flex items-center gap-2 text-green-400"><i class="fa-solid fa-arrow-up-from-bracket"></i><span class="font-bold">Withdraw USDT</span></div>
                                <span class="text-[10px] bg-gray-800 text-gray-400 px-2 py-1 rounded-md">BSC (BEP-20)</span>
                            </div>
                            <p class="text-xs text-gray-400 mb-5">Withdraw your earnings directly to your BSC wallet address</p>
                            <button id="walletWithdrawBtn" class="w-full btn-withdraw-initiate py-3 rounded-xl font-bold flex items-center justify-center gap-2 shadow-lg btn-press"><i class="fa-solid fa-paper-plane"></i>Withdrawal USDT</button>
                        </div>
                        <div class="flex justify-between items-center mb-4"><div class="flex items-center gap-2 text-gray-300"><i class="fa-solid fa-clock-rotate-left text-sm"></i><span class="text-sm font-semibold">Transaction History</span></div><span class="text-xs text-gray-600">${transactionHistory.length || 1}</span></div>
                        <div class="space-y-3">${renderWalletHistory()}</div>
                    </div>
                `;
                document.getElementById("walletWithdrawBtn")?.addEventListener("click", openWithdrawModal);
            } else if (tabId === "profile") {
                sectionTitle.classList.remove("hidden");
                taskBadge.classList.remove("hidden");
                balanceCard.style.display = "block";
                sectionTitle.innerText = "My Profile";
                mainContent.innerHTML = `
                    <div class="bg-[#1e293b] p-5 rounded-2xl text-center border border-gray-700">
                        <div class="w-20 h-20 bg-gradient-to-br from-blue-500 to-indigo-600 rounded-full mx-auto flex items-center justify-center text-2xl font-bold shadow-xl mb-3">TE</div>
                        <p class="text-lg font-bold">Lumina</p>
                        <p class="text-xs text-green-400 mb-2"><i class="fa-regular fa-envelope"></i> t.me/global_lumina</p>
                        <div class="grid grid-cols-2 gap-3 mt-4 text-sm">
                            <div class="bg-[#0f172a] p-2 rounded-xl"><span class="block text-blue-400 font-bold">${completedTasks.size}</span><span class="text-[10px]">Tasks Done</span></div>
                            <div class="bg-[#0f172a] p-2 rounded-xl"><span class="block text-blue-400 font-bold">${balance.toFixed(2)}</span><span class="text-[10px]">USDT Balance</span></div>
                        </div>
                    </div>
                `;
            } else {
                sectionTitle.classList.remove("hidden");
                taskBadge.classList.remove("hidden");
                balanceCard.style.display = "block";
                sectionTitle.innerText = tabId === "home" ? "✨ Active Missions" : "Available Tasks";
                renderTasks();
            }
        }
        
        const historyModal = document.getElementById("historyModal");
        const withdrawModal = document.getElementById("withdrawModal");
        const closeHistoryBtn = document.getElementById("closeHistoryBtn");
        const closeWithdrawBtn = document.getElementById("closeWithdrawBtn");
        const cancelWithdrawBtn = document.getElementById("cancelWithdrawBtn");
        const confirmWithdrawBtn = document.getElementById("confirmWithdrawBtn");
        const withdrawAddress = document.getElementById("withdrawAddress");
        const withdrawAmount = document.getElementById("withdrawAmount");
        const withdrawError = document.getElementById("withdrawError");
        const summaryAmount = document.getElementById("summaryAmount");
        const summaryTotal = document.getElementById("summaryTotal");
        
        // Only history button in modal, no history button in balance card
        if (closeHistoryBtn) closeHistoryBtn.addEventListener("click", () => historyModal.classList.add("hidden"));
        
        function openWithdrawModal() {
            withdrawModal.classList.remove("hidden");
            withdrawAddress.value = "";
            withdrawAmount.value = "";
            withdrawError.classList.add("hidden");
            updateSummary();
        }
        
        function updateSummary() {
            let amount = parseFloat(withdrawAmount.value);
            if (isNaN(amount)) amount = 0;
            const amountReceived = amount - NETWORK_FEE;
            summaryAmount.innerText = amount.toFixed(2) + " USDT";
            summaryTotal.innerText = (amountReceived > 0 ? amountReceived.toFixed(2) : "0.00") + " USDT";
            if (amountReceived <= 0 && amount > 0) {
                summaryTotal.classList.add("text-red-400");
                summaryTotal.classList.remove("text-green-400");
            } else {
                summaryTotal.classList.add("text-green-400");
                summaryTotal.classList.remove("text-red-400");
            }
        }
        
        closeWithdrawBtn?.addEventListener("click", () => withdrawModal.classList.add("hidden"));
        cancelWithdrawBtn?.addEventListener("click", () => withdrawModal.classList.add("hidden"));
        withdrawAmount?.addEventListener("input", updateSummary);
        
        confirmWithdrawBtn?.addEventListener("click", () => {
            const address = withdrawAddress.value.trim();
            const amount = parseFloat(withdrawAmount.value);
            const result = attemptWithdraw(address, amount);
            if (result.success) {
                showToast(result.msg, "success");
                withdrawModal.classList.add("hidden");
                updateHistoryModalUI();
                saveAppData();
                if (document.querySelector(".bottom-nav-item.text-white")?.getAttribute("data-tab") === "wallet") setActiveTab("wallet");
            } else {
                withdrawError.innerText = result.msg;
                withdrawError.classList.remove("hidden");
            }
        });
        
        document.querySelectorAll(".bottom-nav-item").forEach(item => {
            item.addEventListener("click", () => setActiveTab(item.getAttribute("data-tab")));
        });
        
        window.onclick = (e) => {
            if(e.target === historyModal) historyModal.classList.add("hidden");
            if(e.target === withdrawModal) withdrawModal.classList.add("hidden");
        };
        
        document.getElementById("notificationBtn")?.addEventListener("click", () => showToast("🔔 All systems operational. Complete tasks to earn USDT!", "info"));
        
        loadAppData();
        setActiveTab("home");
        updateBalanceUI();
    </script>
</body>
</html>
```
